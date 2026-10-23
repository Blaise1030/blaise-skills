---
name: blaise
description: >-
  Orchestrate feature development through SPEC → PLAN → BUILD → RELEASE on one
  stacked feature branch with GitHub Issues. Use when the user invokes /blaise,
  asks to run the blaise workflow, or wants a full feature cycle (requirements,
  issue-backed plan, TDD build with CI, then release docs/marketing) as a
  parallel alternative to slice-driven-development.
disable-model-invocation: true
---

# /blaise — Feature Orchestration

Ship a feature as **one stacked feature branch**, tracked by **GitHub Issues**, through four hard-gated phases.

```
SPEC → PLAN → BUILD → RELEASE
```

**Parallel alternative** to `slice-driven-development` (many PRs off main). Here: one `feature/<slug>` branch, ordered issues, one umbrella PR, TDD + CI until done, then release artifacts.

## When to use

- User invokes `/blaise` or names this skill
- Multi-step feature that needs requirements → plan → implementation → ship
- Prefer a single feature branch over one-PR-per-slice

**Not for:** tiny bounded fixes (use normal TDD), spikes, or pure bug hunts (use systematic-debugging).

## Phase map

| Phase | Skill | Exit gate |
|-------|-------|-----------|
| 1 SPEC | [blaise-spec](../blaise-spec/SKILL.md) | Human approves written spec |
| 2 PLAN | [blaise-plan](../blaise-plan/SKILL.md) | Human approves plan + issues + feature branch |
| 3 BUILD | [blaise-build](../blaise-build/SKILL.md) | All issue tasks done, umbrella PR CI green |
| 4 RELEASE | [blaise-release](../blaise-release/SKILL.md) | Docs/marketing done; human chooses integrate/ship |

Do phases in order. Never skip an approval gate. Resume at the earliest incomplete phase.

## Superpowers references

If Superpowers (or the project's `.agents/skills`) is installed, **read and follow** these as sub-skills — do not reinvent them:

| Phase | Prefer |
|-------|--------|
| SPEC | `brainstorming` (architectural path → design doc) |
| PLAN | `writing-plans` for task granularity/TDD steps; issue creation is GitHub-only (this pack) |
| BUILD | `test-driven-development`, `verification-before-completion`, `systematic-debugging`; optional `subagent-driven-development` / `executing-plans` for task execution style |
| RELEASE | `finishing-a-development-branch` for merge/PR/keep menu; this pack adds docs/marketing |

Announce: `Using blaise → <phase> (<sub-skill if any>)`.

## Boot sequence

1. **Announce** `/blaise` and detect resume vs fresh start (spec/plan paths, open feature PR, open issues).
2. **Repo facts** (record in the spec): default branch, `gh` auth, PR CI workflow names, test commands, docs locations.
3. **Tracker is always GitHub Issues** in the current repo (`gh issue`). Optional: add cards to a GitHub Project if the human names one — never invent a project.
4. **Enter the earliest incomplete phase** — read that phase's SKILL.md and follow it exactly.
5. Between phases: stop for the exit gate; do not auto-advance on silence.

## Feature branch contract

- Branch: `feature/<slug>` from up-to-date default branch
- One umbrella PR (draft from PLAN onward is fine)
- Issues are the task units; work stacks as commits on that branch (not separate PRs per issue unless the human asks)
- Issue bodies carry `Blocked by: #N` for ordering
- Parent/epic issue links spec + plan + umbrella PR; closes only after RELEASE (or human declines release work and accepts merge)

## Stop conditions (any phase)

Stop and ask only for: irreversible/destructive ops, security-sensitive actions, push/merge to shared default branch without approval, or a plan so broken every path is a guess. Otherwise decide, ledger the ruling, continue.

## Progress ledger

Keep a short in-chat or `docs/blaise/<slug>/progress.md` ledger:

```markdown
# <Feature> — blaise progress
- Phase: SPEC | PLAN | BUILD | RELEASE
- Spec: <path>
- Plan: <path>
- Branch: feature/<slug>
- Umbrella PR: <url>
- Parent issue: <#n>
- Issues: #a (done) → #b (in progress) → #c (blocked)
- Ruling: <decisions>
```
