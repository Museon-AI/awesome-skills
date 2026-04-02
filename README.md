# Awesome Skills

Open-source agent skills for AI coding tools. Works with [OpenClaw](https://openclaw.ai), Claude Code, OpenAI Codex CLI, Cursor, Windsurf, and any tool that reads the [SKILL.md](https://skillsmp.com/) format.

Born from real production use at [Museon AI](https://github.com/Museon-AI) — these skills helped our team maintain consistent architecture across 50k+ lines of Python/TypeScript code with AI-assisted development.

## Skills

| Skill | Description | Best For |
|-------|-------------|----------|
| [ddd-design](ddd-design/) | DDD decision trees, tactical patterns, code templates, review checklist | Python backends with domain complexity |
| [project-spec](project-spec/) | Living documentation template for your project's architecture, conventions, and data flow | Any full-stack project |
| [codex-review](codex-review/) | Run OpenAI Codex CLI code reviews from your AI agent | Any project with Codex CLI installed |

## Quick Start

### 1. Clone & copy

```bash
git clone https://github.com/Museon-AI/awesome-skills.git

# Copy a skill to your project
cp -r awesome-skills/ddd-design .claude/skills/
```

### 2. Done

Your AI agent will automatically discover and use the skill. No configuration needed.

## Integration

### OpenClaw

Install via CLI or copy manually:

```bash
# Via CLI (if published to ClawHub)
openclaw skills install ddd-design

# Or copy manually to workspace or global skills
cp -r ddd-design <workspace>/skills/
# or
cp -r ddd-design ~/.openclaw/skills/
```

OpenClaw loads skills from `<workspace>/skills/`, `~/.openclaw/skills/`, and other [configurable paths](https://docs.openclaw.ai/tools/skills).

### Claude Code

Copy skills to `.claude/skills/` (project-level) or `~/.claude/skills/` (global):

```
your-project/
└── .claude/
    └── skills/
        └── ddd-design/
            ├── SKILL.md
            └── references/
```

### OpenAI Codex CLI

Copy skills to `~/.codex/skills/`:

```
~/.codex/
└── skills/
    └── ddd-design/
        ├── SKILL.md
        └── references/
```

### Cursor

Add to `.cursor/rules/` or reference via `@docs`:

```
your-project/
└── .cursor/
    └── rules/
        └── ddd-design.md    # paste content from SKILL.md
```

### Windsurf

Add to `.windsurf/rules/`:

```
your-project/
└── .windsurf/
    └── rules/
        └── ddd-design.md
```

### Generic (any AI tool)

Skills are plain markdown. Paste them into whatever context mechanism your tool provides, or reference them in conversation:

> "Follow the patterns in ddd-design/SKILL.md for this feature"

## Customization

### ddd-design

**Ready to use as-is** for Python backends using dataclasses, FastAPI, and SQLAlchemy. To adapt:

- **Different ORM**: Update the repository templates in `references/templates.md`
- **Different framework**: Swap FastAPI examples in templates and checklist
- **Different language**: The patterns (aggregate root, state machine, CQRS) are language-agnostic; rewrite the code examples

### project-spec

This is a **template** — fill in the `<!-- TODO -->` sections with your project's specifics:

1. `architecture.md` — your project structure and layer paths
2. `common-data.md` — your data domains and API mappings
3. `conventions.md` — your naming, git, and API conventions
4. `frontend-backend-map.md` — your frontend-backend file mapping
5. `frontend-toolkit.md` — your reusable hooks, utils, contexts
6. `database.md` — your DB conventions and migration approach
7. `infrastructure.md` — your auth, external services, deployment

## Philosophy

**Why give AI agents structured skills?**

- **Consistency**: Without skills, agents reinvent patterns each conversation. With skills, they follow your established patterns.
- **Onboarding**: New team members (human or AI) get up to speed by reading the spec.
- **Living docs**: Unlike wikis that rot, skills live next to the code and get updated as part of development.
- **Tool-agnostic**: SKILL.md is an open standard. No vendor lock-in.

**Principles:**

1. **Prescriptive, not descriptive** — tell the agent what to do, not just what exists
2. **Decision trees over rules** — help the agent make judgment calls
3. **Templates over examples** — copy-paste scaffolding beats vague guidance
4. **Checklists for review** — concrete yes/no items catch violations

## Contributing

PRs welcome! If you've built a skill that helps your team, consider sharing it:

1. Create a new directory at the repo root
2. Include a `SKILL.md` entry point
3. Put detailed content in `references/`
4. Update this README's skills table

## License

MIT — see [LICENSE](LICENSE)
