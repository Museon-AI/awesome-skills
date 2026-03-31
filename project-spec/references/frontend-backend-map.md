# Frontend ↔ Backend Map

Maps frontend API client files to their backend counterparts. Helps the agent navigate both sides of a feature.

## Template

| Frontend (`lib/api/`) | Backend Endpoint | Service | Domain Model |
|-----------------------|-----------------|---------|--------------|
| `orders.ts` | `api/endpoints/orders.py` | `order_service.py` | `domain/models/order.py` |

## Infrastructure

| Component | File |
|-----------|------|
| HTTP client (base) | `lib/api/client.ts` |
| Auth helpers | `lib/auth.ts` |

<!-- TODO: Fill in your project's frontend-backend mapping -->
