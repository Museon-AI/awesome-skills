# Naming & Coding Conventions

## File Naming

### Backend

| Type | Pattern | Example |
|------|---------|---------|
| Domain Model | `snake_case.py` | `order_item.py` |
| Repository Interface | `*_repository.py` | `order_repository.py` |
| Repository Impl | `*_repository_impl.py` | `order_repository_impl.py` |
| Service | `*_service.py` | `order_service.py` |
| API Endpoint | `snake_case.py` | `orders.py` |
| Schema | `snake_case.py` | `order.py` |

### Frontend

| Type | Pattern | Example |
|------|---------|---------|
| Route folder | kebab-case | `app/(dashboard)/order-history/` |
| React Component | PascalCase.tsx | `OrderCard.tsx` |
| API client | kebab-case.ts | `orders.ts` |
| Hook | use-kebab-case.ts | `use-orders.ts` |
| Context | kebab-case-context.tsx | `order-context.tsx` |

## Variable Naming

**Python**: Classes `PascalCase`, functions/variables `snake_case`, constants `UPPER_SNAKE_CASE`

**TypeScript**: Components/types `PascalCase`, functions/variables `camelCase`, constants `UPPER_SNAKE_CASE`

## Import Order

```typescript
import { useState } from 'react';                // 1. Framework
import { format } from 'date-fns';               // 2. Third-party
import { Button } from '@/components/ui/button';  // 3. Internal (absolute)
import { OrderCard } from './order-card';          // 4. Relative
```

## Git Conventions

Branches: `feat/feature-name`, `fix/bug-description`, `refactor/what-changed`

<!-- TODO: Add your commit message format -->

## API Design

### URL Structure

```
/api/v1/{resource}
/api/v1/{resource}/{id}
/api/v1/{resource}/{id}/{sub-resource}
```

Paths use `kebab-case`, no trailing slash.

### HTTP Verbs

| Operation | Verb | Status |
|-----------|------|--------|
| List | GET | 200 |
| Get one | GET | 200 |
| Create | POST | 201 |
| Update | PATCH | 200 |
| Delete | DELETE | 204 |

Non-CRUD actions: `POST` + verb-noun kebab-case (e.g., `/generate-report`)

### Error Handling

Use consistent error response format:

```json
{"detail": "Resource not found"}
```

| Scenario | Status Code |
|----------|-------------|
| Not found | 404 |
| No permission | 403 |
| Validation error | 422 |
| Business logic error | 400 |

<!-- TODO: Add your project-specific conventions -->
