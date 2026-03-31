# Database Conventions

## Standard Column Patterns

<!-- TODO: Customize to your project -->

Every table should include:

```sql
id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
created_at      TIMESTAMPTZ NOT NULL DEFAULT now(),
updated_at      TIMESTAMPTZ NOT NULL DEFAULT now()
```

### Soft Delete (if applicable)

```sql
deleted_at      TIMESTAMPTZ DEFAULT NULL
```

Add a partial index: `WHERE deleted_at IS NULL`

## Multi-tenancy

<!-- TODO: Choose your multi-tenancy model -->

| Level | Column | Description |
|-------|--------|-------------|
| Organization | `organization_id` | Org-scoped data |
| Workspace | `workspace_id` | Workspace-scoped data |
| Global | (none) | System-wide data |

Always index tenant columns.

## Indexing Strategy

- Every foreign key gets an index
- Composite indexes: most selective column first
- Partial indexes for filtered queries (e.g., `WHERE status = 'active'`)
- JSONB fields: GIN index if queried frequently

## Naming

- Tables: `snake_case`, plural (`orders`, `order_items`)
- Columns: `snake_case`
- Foreign keys: `{referenced_table_singular}_id` (e.g., `order_id`)
- Indexes: `idx_{table}_{columns}` (e.g., `idx_orders_workspace_id`)

## Migration Conventions

<!-- TODO: Add your migration tool and path -->

- Path: `database/migrations/`
- Format: `NNN_description.sql` (sequential numbering)
- Each migration must be idempotent or use transactions
