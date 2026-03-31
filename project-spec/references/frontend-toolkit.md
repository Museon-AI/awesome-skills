# Frontend Toolkit Quick Reference

Document your project's reusable utilities so the agent knows what already exists before writing new code.

## Utilities

<!-- TODO: List your utility functions -->

| Utility | File | Usage |
|---------|------|-------|
| `formatDate(date, tz)` | `lib/utils/date.ts` | Timezone-aware date formatting |
| `cn(...classes)` | `lib/utils/cn.ts` | Tailwind class merging |

## Custom Hooks

<!-- TODO: List your custom hooks -->

| Hook | File | Purpose |
|------|------|---------|
| `useCurrentUser()` | `hooks/use-current-user.ts` | Get authenticated user |

## Contexts

<!-- TODO: List your React contexts -->

| Context | File | Provides |
|---------|------|----------|
| `UserContext` | `contexts/user-context.tsx` | Current user + permissions |

## UI Components

<!-- TODO: List non-obvious shared components -->

Base component library: `components/ui/` (e.g., shadcn/ui, Radix, etc.)

## Type Definitions

<!-- TODO: List shared types -->

| Type | File |
|------|------|
| `User` | `lib/types/user.ts` |
