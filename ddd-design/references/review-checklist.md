# DDD Review Checklist

Items marked `[MUST]` are blocking — fix before merge. Items marked `[SHOULD]` are recommended.

## Layer Compliance

- [ ] **[MUST]** Endpoint contains no business logic (only: validate params -> call Service -> return response)
- [ ] **[MUST]** Application Service does not query the database directly (uses Repository interface)
- [ ] **[MUST]** Domain Model does not import external tech (no database, HTTP, framework imports)
- [ ] **[MUST]** Infrastructure does not depend on API or Application layers

## Domain Model

- [ ] **[MUST]** Uses `@dataclass(slots=True)`
- [ ] **[MUST]** State transitions return a new instance via `replace(self, ...)` — never mutate self
- [ ] **[MUST]** Validates business rules before transition; raises domain exception on illegal transition
- [ ] **[SHOULD]** Uses declarative state machine for 3+ states
- [ ] **[SHOULD]** Has `version` field for optimistic concurrency

## Repository

- [ ] **[MUST]** Interface defined in `domain/repositories/`, implementation in `infrastructure/repositories/`
- [ ] **[MUST]** Returns Domain Model, never raw dict
- [ ] **[SHOULD]** Conditional updates use semantic method names (not generic `update()`)
- [ ] **[SHOULD]** Optimistic lock methods return `None` on conflict (not raise exception)

## Application Service

- [ ] **[MUST]** Dependencies injected via constructor (Repository, Client, etc.)
- [ ] **[MUST]** State transition logic lives in Domain Model; Service only orchestrates
- [ ] **[SHOULD]** Cross-aggregate operations coordinated in Service, not in Repository
- [ ] **[SHOULD]** Side effects (event publishing, async tasks) triggered in Service

## Schema

- [ ] **[MUST]** Separate Request and Response models
- [ ] **[SHOULD]** Update requests include `expected_version` field when optimistic locking applies

## Endpoint

- [ ] **[MUST]** Uses dependency injection to get Service — never instantiates directly
- [ ] **[MUST]** Has authentication/authorization checks
- [ ] **[SHOULD]** Uses versioned API paths (e.g., `/api/v2/`)

## Common Violations

| Violation | Correct Approach |
|-----------|-----------------|
| Endpoint queries database directly | Extract to Repository |
| Service imports database client | Access data through Repository interface |
| Domain Model makes HTTP requests | Move side effects to Application Service |
| Repository orchestrates multiple aggregates | Move to Application Service |
| Business logic scattered across Service | Consolidate in Domain Model |
