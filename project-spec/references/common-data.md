# Common Data Resources

Map your project's key data domains here so the agent knows where data lives and how to access it.

## Template

For each data domain, document:

```
### [Domain Name]

| Resource | Backend | Frontend |
|----------|---------|----------|
| List items | `GET /api/v1/items` | `lib/api/items.ts → getItems()` |
| Get single | `GET /api/v1/items/{id}` | `lib/api/items.ts → getItem()` |
| Create | `POST /api/v1/items` | `lib/api/items.ts → createItem()` |

- Service: `application/services/item_service.py`
- Domain Model: `domain/models/item.py`
- DB Table: `items`
```

## Example

### Users & Organizations

| Resource | Backend | Frontend |
|----------|---------|----------|
| Current user profile | `GET /api/v1/me` | `lib/api/auth.ts → getMe()` |
| Organization members | `GET /api/v1/orgs/{id}/members` | `lib/api/orgs.ts → getMembers()` |

<!-- TODO: Add your project's data domains below -->
