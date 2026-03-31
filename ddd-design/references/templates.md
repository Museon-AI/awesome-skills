# Code Templates

Copy and replace `MyFeature` / `my_feature` to scaffold a new module.

## Domain Model (Aggregate Root)

```python
# domain/models/my_feature.py
from __future__ import annotations
from dataclasses import dataclass, replace
from typing import Literal

MyFeatureStatus = Literal["draft", "active", "completed", "cancelled"]

@dataclass(slots=True)
class MyFeature:
    id: str
    workspace_id: str
    name: str
    status: MyFeatureStatus
    version: int
    created_at: str
    updated_at: str

    def activate(self) -> MyFeature:
        if self.status != "draft":
            raise MyFeatureTransitionError(self.status, "activate")
        return replace(self, status="active", version=self.version + 1)

    def complete(self) -> MyFeature:
        if self.status != "active":
            raise MyFeatureTransitionError(self.status, "complete")
        return replace(self, status="completed", version=self.version + 1)

    def cancel(self) -> MyFeature:
        if self.status in ("completed", "cancelled"):
            raise MyFeatureTransitionError(self.status, "cancel")
        return replace(self, status="cancelled", version=self.version + 1)


@dataclass(slots=True)
class MyFeatureTransitionError(Exception):
    current_status: str
    attempted_event: str

    def __str__(self) -> str:
        return f"Cannot {self.attempted_event} from {self.current_status}"
```

## Repository Interface

```python
# domain/repositories/my_feature_repository.py
from abc import ABC, abstractmethod
from domain.models.my_feature import MyFeature

class MyFeatureRepository(ABC):
    @abstractmethod
    async def get_by_id(self, feature_id: str) -> MyFeature | None: ...

    @abstractmethod
    async def list_by_workspace(self, workspace_id: str) -> list[MyFeature]: ...

    @abstractmethod
    async def create(self, feature: MyFeature) -> MyFeature: ...

    @abstractmethod
    async def update(self, feature: MyFeature, expected_version: int) -> MyFeature | None:
        """Returns None on version conflict."""
        ...
```

## Repository Implementation

```python
# infrastructure/repositories/my_feature_repository_impl.py
from sqlalchemy import text
from domain.models.my_feature import MyFeature
from domain.repositories.my_feature_repository import MyFeatureRepository

class SQLMyFeatureRepository(MyFeatureRepository):
    """Adapt to your database layer (SQLAlchemy, Supabase, Prisma, etc.)."""

    def __init__(self, db):
        self._db = db

    async def get_by_id(self, feature_id: str) -> MyFeature | None:
        result = await self._db.execute(
            text("SELECT * FROM my_features WHERE id = :id"),
            {"id": feature_id},
        )
        row = result.mappings().first()
        return self._to_model(row) if row else None

    async def update(self, feature: MyFeature, expected_version: int) -> MyFeature | None:
        result = await self._db.execute(
            text("""
                UPDATE my_features
                SET status = :status, name = :name,
                    version = :version, updated_at = now()
                WHERE id = :id AND version = :expected_version
                RETURNING *
            """),
            {
                "id": feature.id,
                "status": feature.status,
                "name": feature.name,
                "version": feature.version,
                "expected_version": expected_version,
            },
        )
        row = result.mappings().first()
        return self._to_model(row) if row else None

    @staticmethod
    def _to_model(row: dict) -> MyFeature:
        return MyFeature(
            id=str(row["id"]),
            workspace_id=str(row["workspace_id"]),
            name=row["name"],
            status=row["status"],
            version=row["version"],
            created_at=str(row["created_at"]),
            updated_at=str(row["updated_at"]),
        )
```

## Application Service

```python
# application/services/my_feature_service.py
from domain.models.my_feature import MyFeature
from domain.repositories.my_feature_repository import MyFeatureRepository

class MyFeatureService:
    def __init__(self, repository: MyFeatureRepository):
        self._repository = repository

    async def get(self, feature_id: str) -> MyFeature | None:
        return await self._repository.get_by_id(feature_id)

    async def activate(self, feature_id: str, expected_version: int) -> MyFeature:
        feature = await self._repository.get_by_id(feature_id)
        if not feature:
            raise ValueError(f"Feature {feature_id} not found")

        updated = feature.activate()  # domain logic in Model
        result = await self._repository.update(updated, expected_version)
        if not result:
            raise ValueError(f"Version conflict for {feature_id}")
        return result
```

## Schema (Pydantic)

```python
# schemas/my_feature.py
from pydantic import BaseModel, Field

class MyFeatureResponse(BaseModel):
    id: str
    workspace_id: str
    name: str
    status: str
    version: int

class CreateMyFeatureRequest(BaseModel):
    workspace_id: str
    name: str

class UpdateMyFeatureRequest(BaseModel):
    expected_version: int = Field(..., ge=0)
    name: str | None = None
```

## Endpoint (FastAPI)

```python
# api/endpoints/my_feature.py
from fastapi import APIRouter, Depends, HTTPException
from application.services.my_feature_service import MyFeatureService
from schemas.my_feature import MyFeatureResponse

router = APIRouter(prefix="/my-features", tags=["my-features"])

@router.get("/{feature_id}", response_model=MyFeatureResponse)
async def get_feature(
    feature_id: str,
    service: MyFeatureService = Depends(get_my_feature_service),
):
    feature = await service.get(feature_id)
    if not feature:
        raise HTTPException(status_code=404, detail="Not found")
    return MyFeatureResponse(**vars(feature))
```

## Dependency Injection

```python
# application/dependencies.py
from fastapi import Depends
from domain.repositories.my_feature_repository import MyFeatureRepository
from infrastructure.repositories.my_feature_repository_impl import SQLMyFeatureRepository
from application.services.my_feature_service import MyFeatureService

def get_my_feature_repository(db=Depends(get_db)) -> MyFeatureRepository:
    return SQLMyFeatureRepository(db)

def get_my_feature_service(
    repository: MyFeatureRepository = Depends(get_my_feature_repository),
) -> MyFeatureService:
    return MyFeatureService(repository)
```
