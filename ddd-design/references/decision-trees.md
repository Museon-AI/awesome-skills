# Decision Trees

## Decision 1: How Much DDD Does This Feature Need?

```
New feature →
  ├─ Has state transitions (draft → active → completed)?
  │   ├─ Multiple aggregates need coordination? → Full DDD
  │   └─ No → Lightweight DDD (Domain Model + Repository + Service)
  ├─ Has complex business rules (beyond data storage)? → Lightweight DDD
  ├─ Read-only / display? → Read Model + Repository
  └─ Simple CRUD → Service + Repository, Domain Model optional
```

### File Structures by Level

**Full DDD** (complex domain with state machines, multiple entities):
```
domain/models/my_feature/        # sub-package
  ├── my_aggregate.py            # aggregate root + state transitions
  ├── child_entity.py
  └── state_machines.py
domain/repositories/my_feature/
  └── my_aggregate_repository.py
infrastructure/repositories/my_feature/
  └── my_aggregate_repository_impl.py
application/services/my_feature/
  ├── my_feature_service.py
  └── scanner.py                 # if async polling needed
schemas/my_feature.py
api/endpoints/my_feature.py
```

**Lightweight DDD** (single files):
```
domain/models/my_feature.py
domain/repositories/my_feature_repository.py
infrastructure/repositories/my_feature_repository_impl.py
application/services/my_feature_service.py
schemas/my_feature.py
api/endpoints/my_feature.py
```

**Simple CRUD** (Domain Model optional):
```
domain/repositories/my_feature_repository.py       # optional
infrastructure/repositories/my_feature_repository_impl.py
application/services/my_feature_service.py          # thin service
schemas/my_feature.py
api/endpoints/my_feature.py
```

---

## Decision 2: Sub-package or Single File?

- **3+ tightly related aggregates** → sub-package: `domain/models/my_feature/`
- **1-2 models** → single file: `domain/models/my_feature.py`
- **Likely to be split into a microservice later** → sub-package (keep boundaries clear)

---

## Decision 3: Repository Method Design

| Operation Type | Approach |
|----------------|----------|
| Simple CRUD (get, list, create, update) | Generic naming, must return Domain Model |
| Conditional update ("only if status=pending") | Semantic method name + optimistic lock: `cancel_if_pending(id, expected_version)` |
| Multi-table atomic operation | Single Repository method with transaction: `submit_checkout()` → updates order + creates payment |
| Cross-aggregate operation | **Not in Repository** — orchestrate in Application Service |

---

## Decision 4: State Management Strategy

| Scenario | Approach |
|----------|----------|
| 3+ states with a transition graph | Declarative state machine |
| 2 states (active/inactive) | Boolean field + Domain Model validation method |
| Transitions trigger side effects (notify, charge, spawn tasks) | Side effects in Application Service, **never** in Domain Model |

---

## Decision 5: Async Task Selection

| Scenario | Choice |
|----------|--------|
| Non-blocking immediate execution | Task queue (Celery, Cloud Tasks, etc.) |
| Multi-step + retry + concurrency control | Workflow engine (Prefect, Temporal, etc.) |
| Periodic scanning / polling | Scanner pattern in Service, triggered by scheduler |
| Delayed execution (e.g., check payment after 30min) | Task queue with scheduled delay |

Adapt the specific tools to your stack — the decision criteria remain the same.
