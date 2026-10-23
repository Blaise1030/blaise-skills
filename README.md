# blaise skills

Public Cursor skill pack for **`/blaise`** — feature orchestration on one stacked feature branch with GitHub Issues.

```
SPEC → PLAN → BUILD → RELEASE
```

Parallel alternative to Superpowers **slice-driven-development** (many PRs off main). This pack stacks work on `feature/<slug>`, tracks tasks as GitHub Issues, TDD-builds against the umbrella PR’s GitHub Actions, then handles docs/marketing before integrate.

## Skills

| Skill | Role |
|-------|------|
| [`blaise`](blaise/SKILL.md) | Orchestrator — invoke with `/blaise` |
| [`blaise-spec`](blaise-spec/SKILL.md) | Requirements / design spec |
| [`blaise-plan`](blaise-plan/SKILL.md) | Implementation plan + GitHub Issues + feature branch + draft PR |
| [`blaise-build`](blaise-build/SKILL.md) | TDD + CI watch until issues done |
| [`blaise-release`](blaise-release/SKILL.md) | Docs / marketing + finish branch |

## Install (Cursor)

Clone into your personal skills directory (or copy the five folders):

```bash
git clone https://github.com/Blaise1030/blaise-skills.git ~/.cursor/skills/blaise-skills
# Cursor discovers SKILL.md under ~/.cursor/skills/** — symlink the skill dirs if your version only scans one level:
ln -s ~/.cursor/skills/blaise-skills/blaise ~/.cursor/skills/blaise
ln -s ~/.cursor/skills/blaise-skills/blaise-spec ~/.cursor/skills/blaise-spec
ln -s ~/.cursor/skills/blaise-skills/blaise-plan ~/.cursor/skills/blaise-plan
ln -s ~/.cursor/skills/blaise-skills/blaise-build ~/.cursor/skills/blaise-build
ln -s ~/.cursor/skills/blaise-skills/blaise-release ~/.cursor/skills/blaise-release
```

Then invoke **`/blaise`** in Agent chat.

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
