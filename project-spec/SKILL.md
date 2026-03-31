# Project Spec — Living Documentation for AI Coding Agents

A template for maintaining a "living spec" that AI coding agents can reference when working in your codebase. Keeps agents aligned with your team's conventions, architecture, and data flow — without repeating yourself every conversation.

## How It Works

1. Fill in the reference files below with your project's specifics
2. Point your AI tool to this directory (see [Integration Guide](../../README.md#integration))
3. The agent reads the spec before writing code, ensuring consistency

## Quick Navigation

| I want to know... | Reference |
|-------------------|-----------|
| Where data comes from (APIs, services) | [references/common-data.md](references/common-data.md) |
| Where to put new code | [references/architecture.md](references/architecture.md) |
| Frontend ↔ Backend API mapping | [references/frontend-backend-map.md](references/frontend-backend-map.md) |
| Naming and coding conventions | [references/conventions.md](references/conventions.md) |
| Frontend utilities, hooks, contexts | [references/frontend-toolkit.md](references/frontend-toolkit.md) |
| Database schema and conventions | [references/database.md](references/database.md) |
| Auth, infra, external services | [references/infrastructure.md](references/infrastructure.md) |

## Output Format

When the agent finds an answer in the spec, it should:

1. **Answer directly**: file paths, function names, code examples
2. **Cite the source**: "Per conventions.md..." so the human can verify
3. **Show compliant code**: if code is involved, follow the spec

## Keeping It Alive

**When to update the spec:**
- New migration added → check `database.md`
- New domain model → check `common-data.md` and `frontend-backend-map.md`
- New API client → check `frontend-backend-map.md`
- New endpoint/service → check relevant references
- New hooks/contexts/utils → check `frontend-toolkit.md`
- New external integration → check `infrastructure.md`

The agent should proactively ask: "Should I update the project spec for this change?"

---

*This is a living document. Update the references as your team makes new decisions.*
