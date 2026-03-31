# Architecture: Where to Put Code

> Adapt paths below to your project structure.

## Project Structure

```
your-project/
├── apps/
│   ├── web/          # Frontend (Next.js / React / Vue / etc.)
│   └── api/          # Backend (FastAPI / Express / etc.)
└── database/         # Migrations and schemas
```

## Backend Layers

### Creating a New Feature (full flow)

```
1. Domain Model          → domain/models/my_feature.py
2. Repository Interface  → domain/repositories/my_feature_repository.py
3. Repository Impl       → infrastructure/repositories/my_feature_repository_impl.py
4. Application Service   → application/services/my_feature_service.py
5. API Schema            → schemas/my_feature.py
6. API Endpoint          → api/endpoints/my_feature.py
7. Dependency Injection  → application/dependencies.py
```

### Layer Responsibilities

| Layer | Path | Does | Does NOT |
|-------|------|------|----------|
| **API** | `api/endpoints/` | HTTP handling, param validation, response mapping | Business logic |
| **Application** | `application/services/` | Orchestrates business flows | Direct DB access |
| **Domain** | `domain/` | Business rules, repository interfaces | Import external tech |
| **Infrastructure** | `infrastructure/` | DB implementation, external API clients | Depend on API layer |
| **Schemas** | `schemas/` | Request/Response Pydantic models | Business logic |

<!-- TODO: Add your background task paths and patterns -->

### Background Tasks

| Scenario | Choice | Path |
|----------|--------|------|
| Multi-step, long-running, needs retry | Workflow engine | `TODO` |
| Simple async, polling | Task queue | `TODO` |

## Frontend Structure

```
web/
├── app/                    # Routes
│   ├── (auth)/             # Login/signup
│   ├── (dashboard)/        # Main app (authenticated)
│   └── (public)/           # Public pages
├── components/             # Shared components
│   └── ui/                 # Base UI components
├── lib/
│   ├── api/                # API client functions
│   ├── utils/              # Utilities
│   ├── types/              # Type definitions
│   └── constants/          # Constants
├── hooks/                  # Custom hooks
└── contexts/               # React Context
```

### Frontend Placement Rules

1. **New route page** → `app/(dashboard)/feature-name/page.tsx`
2. **Page-specific components** → same directory; shared → `components/`
3. **API calls** → `lib/api/feature-name.ts`
4. **State management** → hooks first; cross-component → Context
5. **UI primitives** → reuse `components/ui/` before creating new ones

## Database

<!-- TODO: Fill in your schema source and migration path -->

- Schema source: `TODO`
- Migrations: `TODO`
