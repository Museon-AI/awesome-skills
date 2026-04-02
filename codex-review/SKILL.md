---
name: codex-review
description: Run OpenAI Codex CLI code reviews from your AI coding agent. Supports reviewing uncommitted changes, branch diffs, and specific commits with optional focus prompts.
---

# Codex Code Review Skill

Invoke the local `codex review` CLI to review code changes in the current repository and present the results.

## Usage

```
/codex-review                        # review all uncommitted changes (staged + unstaged + untracked)
/codex-review --base main            # review current branch diff against main
/codex-review --commit <SHA>         # review a specific commit
/codex-review <focus prompt>         # review with a custom focus, e.g. "focus on security vulnerabilities"
/codex-review --base main <focus>    # combine branch diff with a custom focus
```

## Execution Steps

1. **Parse arguments** — determine the review mode from user input
2. **Build command** — assemble the `codex review` command according to the rules below
3. **Run review** — execute the command and capture output (may take 30s–2min)
4. **Present results** — show the full review output to the user without trimming or rewriting

## Command Construction

| User Input | Command |
|------------|---------|
| No arguments | `codex review --uncommitted` |
| `--base <branch>` | `codex review --base <branch>` |
| `--commit <sha>` | `codex review --commit <sha>` |
| `<custom focus>` | `codex review "<custom focus>"` |
| `--base <branch> <focus>` | `codex review --base <branch> "<focus>"` |
| `--commit <sha> <focus>` | `codex review --commit <sha> "<focus>"` |

**Key rule**: `--uncommitted` and custom prompts are mutually exclusive.
- With a custom focus prompt: pass the prompt directly, do NOT add `--uncommitted`
- Without arguments: use `--uncommitted`

## Notes

- `codex review` is non-interactive — it outputs review results and exits
- If the user is not logged in, prompt them to run `codex login`
- Warn the user that reviews may take 30s–2min before executing
- Present `codex` output as-is — do not trim, summarize, or rewrite the review content
