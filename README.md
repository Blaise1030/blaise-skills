# blaise skills

Public Cursor skill pack for **`/blaise`** — feature orchestration on one stacked feature branch with GitHub Issues.

```
SPEC → PLAN → BUILD → RELEASE
```

Parallel alternative to Superpowers **slice-driven-development** (many PRs off main). This pack stacks work on one branch, tracks tasks as GitHub Issues, TDD-builds against the umbrella PR’s GitHub Actions, then handles docs/marketing before integrate.

**Sized paths:** `/blaise` classifies **hotfix** / **task** / **feature** first. Small work skips the written SPEC (and often the plan file); only the feature track runs the full SPEC → PLAN → BUILD → RELEASE ceremony.

## Layout

Only **`blaise/SKILL.md`** is exposed as a Cursor skill. Phase docs live beside it and are read on demand:

| Path | Role |
|------|------|
| [`blaise/SKILL.md`](blaise/SKILL.md) | Orchestrator — invoke with `/blaise` |
| [`blaise/spec.md`](blaise/spec.md) | Requirements / design spec |
| [`blaise/plan.md`](blaise/plan.md) | Implementation plan + GitHub Issues + feature branch + draft PR |
| [`blaise/build.md`](blaise/build.md) | TDD + CI watch until issues done |
| [`blaise/release.md`](blaise/release.md) | Docs / marketing + finish branch |

## Install (Cursor)

```bash
git clone https://github.com/Blaise1030/blaise-skills.git ~/.cursor/skills/blaise-skills
ln -s ~/.cursor/skills/blaise-skills/blaise ~/.cursor/skills/blaise
```

Or clone/copy so `blaise/SKILL.md` sits under `~/.cursor/skills/blaise/`. Then invoke **`/blaise`** in Agent chat.

## Superpowers

Optional but recommended. If [Superpowers](https://github.com/obra/superpowers) (or a project copy under `.agents/skills`) is present, `/blaise` defers to:

- SPEC → `brainstorming`
- PLAN → `writing-plans` (task shape; tracker stays GitHub Issues)
- BUILD → `test-driven-development`, `verification-before-completion`, `systematic-debugging`, plus `executing-plans` / `subagent-driven-development`
- RELEASE → `finishing-a-development-branch`

## Requirements

- `gh` authenticated to the target repo
- GitHub Issues enabled (always the tracker)
- PR checks via GitHub Actions for BUILD’s CI loop

## License

MIT
