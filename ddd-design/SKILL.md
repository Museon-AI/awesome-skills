---
name: ddd-design
description: DDD decision trees, tactical patterns, code templates, and review checklist for Python backend projects. Guides aggregate design, state machines, CQRS, and layered architecture decisions.
---

# DDD Design Guide for AI Coding Agents

Guides domain-driven design decisions in Python backend projects. Helps AI coding agents (Claude Code, Codex, Cursor, etc.) make consistent architectural choices.

## Quick Navigation

| I want to... | Reference |
|--------------|-----------|
| Decide how much DDD a feature needs | [references/decision-trees.md](references/decision-trees.md) |
| Design aggregates, state machines, domain events | [references/tactical-patterns.md](references/tactical-patterns.md) |
| Scaffold a new feature from scratch | [references/templates.md](references/templates.md) |
| Review code for DDD compliance | [references/review-checklist.md](references/review-checklist.md) |

## Core Principles

### 1. Not Everything Needs Full DDD

```
Simple CRUD ──────────── Stateful ──────────── Complex Domain
(Folder, Tag)           (Order, Task)          (Workflow, Campaign)
  |                        |                        |
  └→ Service              └→ Lightweight DDD        └→ Full DDD
     + Repository            (Model + Repo)            (all tactical patterns)
```

### 2. Strict Unidirectional Dependencies

```
API Layer → Application Layer → Domain Layer
                          ↑
              Infrastructure Layer (implements interfaces)
```

- **Domain Layer**: Pure business logic, no framework imports
- **Application Layer**: Orchestrates domain operations
- **Infrastructure Layer**: Database, external APIs — implements domain interfaces
- **API Layer**: HTTP handlers, request/response mapping

### 3. Domain Model Owns Business Rules

State transitions, validations, and invariants live in the Domain Model — not in services, not in endpoints, not in repositories.

---

*Use the reference files for detailed patterns, templates, and checklists.*
