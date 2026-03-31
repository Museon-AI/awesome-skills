# Infrastructure & External Services

## Authentication

<!-- TODO: Document your auth approach -->

| Aspect | Details |
|--------|---------|
| Auth provider | (e.g., Supabase Auth, Auth0, Firebase) |
| Token type | JWT |
| Frontend handling | Auto-refresh in HTTP client |
| Backend validation | Middleware / dependency injection |

## API Client Behavior

<!-- TODO: Document your HTTP client conventions -->

- Base URL configured via environment variable
- Auth token attached automatically
- Standard error handling (401 → redirect to login)

## External Services

<!-- TODO: List your integrations -->

| Service | Purpose | Config |
|---------|---------|--------|
| (e.g., Stripe) | Payments | `STRIPE_API_KEY` |
| (e.g., S3) | File storage | `AWS_BUCKET_NAME` |
| (e.g., SendGrid) | Email | `SENDGRID_API_KEY` |

## Background Tasks

<!-- TODO: Document your async task infrastructure -->

| System | Use Case | Config |
|--------|----------|--------|
| (e.g., Celery) | General async tasks | `REDIS_URL` |
| (e.g., Prefect) | Multi-step workflows | Prefect Cloud |

## Deployment

<!-- TODO: Document deployment targets -->

| Component | Platform |
|-----------|----------|
| Frontend | (e.g., Vercel) |
| Backend | (e.g., Cloud Run) |
| Database | (e.g., Supabase) |

## Environment Variables

<!-- TODO: List required env vars -->

Essential variables for local development:

```
DATABASE_URL=
API_BASE_URL=
AUTH_SECRET=
```
