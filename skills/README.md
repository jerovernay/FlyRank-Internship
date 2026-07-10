# Skills

Reusable, model-loadable skills for this internship. They live under
[`.claude/skills/`](../.claude/skills/) so Claude Code auto-discovers them; this
file is the human-readable catalog.

An AI assistant working in this repo should **read this file first**, then load
the skills relevant to the task.

## Catalog

| Skill | Load it when | Path |
|---|---|---|
| **framing-ml-problems** | Turning a vague "predict/cluster/rank X" into a defensible problem (decision → action → cost), picking the unit of analysis, checking leakage, enforcing careful language. | [`.claude/skills/framing-ml-problems/`](../.claude/skills/framing-ml-problems/SKILL.md) |
| **flyrank-data** | Any task touching the starter CSV or the warehouse release: the four lanes, table grains, the observable-only / leakage discipline, column gotchas, public-safety rules. | [`.claude/skills/flyrank-data/`](../.claude/skills/flyrank-data/SKILL.md) |

## How to use

- **In Claude Code:** the Skill tool discovers these by their `name:` in the
  SKILL.md frontmatter. Invoke `framing-ml-problems` and `flyrank-data`.
- **In any other assistant (Colab, chat):** paste the relevant `SKILL.md` body
  into context before you start the task.

## Adding a skill

Create `.claude/skills/<name>/SKILL.md` with YAML frontmatter (`name`,
`description` — the description is what the model matches on, so make it
trigger-rich), keep the body short and checklist-driven, then add a row above.
