# Tactical Patterns

## Aggregate Root Design

The aggregate root is the only object that external code may reference directly. It enforces invariants for the entire aggregate.

Key characteristics:
- `@dataclass(slots=True)` with immutable updates via `replace(self, ...)`
- `version` field for optimistic concurrency control
- State transition methods return a **new instance** — never mutate self
- Validate business rules **before** transition

```python
@dataclass(slots=True)
class Order:
    id: str
    status: OrderStatus
    version: int
    # ... business fields

    def confirm(self) -> "Order":
        if self.status != "pending":
            raise OrderTransitionError(self.status, "confirm")
        return replace(self, status="confirmed", version=self.version + 1)
```

### When to Create What

| Concept | Characteristics | Example |
|---------|----------------|---------|
| **Aggregate Root** | Independent lifecycle, externally referenced, enforces invariants | `Order`, `Campaign` |
| **Child Entity** | Only meaningful within an aggregate root's context | `OrderItem` belongs to `Order` |
| **Value Object** | No ID, compared by value, immutable | `Money`, `Address`, `TimeRange` |
| **Read Model** | Query-optimized, no mutation behavior | `OrderListItem`, `DashboardStats` |

---

## State Machines

For features with 3+ states, use a declarative state machine to define legal transitions.

```python
# Define transitions declaratively
TRANSITIONS = {
    "draft":     {"activate": "active"},
    "active":    {"complete": "completed", "cancel": "cancelled"},
    "completed": {},  # terminal
    "cancelled": {},  # terminal
}

def validate_transition(current: str, event: str) -> str:
    """Returns new state or raises error."""
    targets = TRANSITIONS.get(current, {})
    if event not in targets:
        raise InvalidTransitionError(current, event)
    return targets[event]
```

**Principles**:
- State machine only declares legal transitions — no side effects
- Domain Model calls the state machine for validation, returns new state
- Application Service handles side effects (notifications, billing, task creation)

---

## Optimistic Concurrency Control

Prevents conflicting concurrent updates. Repository includes `version` in the WHERE clause:

```sql
UPDATE orders
SET status = :new_status, version = version + 1
WHERE id = :id AND version = :expected_version
```

If 0 rows affected → another operation updated first → return `None` and let Application Service handle the conflict.

---

## Domain Exceptions

Define semantic exceptions in the Domain layer. Avoid generic `ValueError`.

```python
@dataclass(slots=True)
class OrderTransitionError(Exception):
    current_status: str
    attempted_event: str

    def __str__(self) -> str:
        return f"Cannot {self.attempted_event} from {self.current_status}"
```

The API layer catches domain exceptions and maps them to HTTP responses.

---

## Application Service Orchestration

The service is the entry point for domain operations. Responsibilities:
1. Fetch aggregate via Repository
2. Call aggregate's domain methods
3. Persist changes
4. Trigger side effects (events, async tasks)

```python
class OrderService:
    def __init__(
        self,
        repository: OrderRepository,
        event_publisher: EventPublisher,
    ):
        self._repository = repository
        self._events = event_publisher

    async def confirm(self, order_id: str, expected_version: int) -> Order:
        order = await self._repository.get_by_id(order_id)
        if not order:
            raise NotFoundError(order_id)

        updated = order.confirm()  # domain logic lives in the model
        result = await self._repository.save(updated, expected_version)
        if not result:
            raise ConcurrencyError(order_id)

        await self._events.publish("order.confirmed", {"id": order_id})
        return result
```

**Rules**:
- Service must NOT contain state transition logic (that belongs to Domain Model)
- Service must NOT query the database directly (use Repository interface)
- Endpoint must NOT orchestrate multiple Repository calls (that belongs to Service)

---

## Read Models (CQRS Read Side)

For query scenarios, use dedicated Read Models that bypass the aggregate root.

```python
@dataclass(slots=True)
class OrderListItem:
    """Contains only fields needed for list views. May join across tables."""
    id: str
    customer_name: str
    status: str
    item_count: int
    total_amount: float
    created_at: str
```

Read Model repositories can use simpler query patterns — strict DDD layering is not required for read-only operations.
