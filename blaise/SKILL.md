---
name: blaise
description: >-
  Orchestrate work through SPEC → PLAN → BUILD → RELEASE on one stacked
  feature branch with GitHub Issues, scaled by size (hotfix / task / feature).
  Use when the user invokes /blaise, asks to run the blaise workflow, or wants
  a gated cycle from requirements to ship — including small issues that should
  skip the written spec. Parallel alternative to slice-driven-development.
disable-model-invocation: true
---

# /blaise — Feature Orchestration

Ship work on **one stacked branch**, tracked by **GitHub Issues**, through phases scaled to size.

```
classify → (SPEC?) → (PLAN?) → BUILD → RELEASE
```

**Parallel alternative** to `slice-driven-development` (many PRs off main). Here: one branch, ordered issues when needed, umbrella PR, TDD + CI, then optional release artifacts.

## Classify first (HARD GATE)

Before SPEC or any phase, **classify and say it out loud** so the human can override:

> "This looks like a **task** — I'll skip the written spec and use a short in-chat design + one GitHub issue."

| Track | What it is | Path |
|-------|------------|------|
| **hotfix** | Clear bug/fix in an existing flow; roughly one commit; acceptance already obvious (or an existing issue states it) | Skip SPEC. Skip plan file. Branch + TDD + CI. One GitHub issue (create or use existing). RELEASE = finish branch only (no docs/marketing unless asked). |
| **task** | Bounded change to code that already exists; a few files; success criteria fit in a short paragraph | Skip written SPEC. Short **in-chat design** → human yes. Light PLAN: usually **one** GitHub issue (parent optional); plan file only if >1 task or non-obvious steps. Then BUILD → RELEASE (docs only if behavior is user-visible and they want it). |
| **feature** | New capability/subsystem, multiple flows, unclear requirements, or will need several stacked tasks | Full [spec](spec.md) → [plan](plan.md) → [build](build.md) → [release](release.md) |

**Signals → track**

| Prefer | When you see |
|--------|----------------|
| hotfix | "fix typo", "null check", "CI flake", existing `#N` with a one-line ask, repro already known |
| task | "add flag", "extend endpoint", "small UI tweak", single user flow, repo already has the pattern |
| feature | "add X", new surface area, data model changes, multiple personas/flows, partner still exploring |

When in doubt between two tracks, take the **heavier** one. Ratchet **up** mid-flight if hidden complexity appears (stop, say so, upgrade). Do not silently downgrade.

**Human override wins.** If they say "just fix it" / "no spec", treat as hotfix or task. If they say "full blaise" / "write a spec", use feature.

### Anti-pattern: forcing SPEC on small work

| Thought | Reality |
|---------|---------|
| "/blaise always means write a design doc" | /blaise means classify, then only the phases that track needs |
| "Safer to always SPEC" | Wrong track wastes their time; task/hotfix still have an approval gate (chat design or obvious fix) |
| "There's an issue number so it's a feature" | Issue size ≠ feature track — read the issue body |
| "I'll SPEC lightly in a file anyway" | Task track: no spec file. Chat design only |

## Phase map (by track)

Read the phase file when entering that phase (only `/blaise` is a discoverable skill):

| Phase | File | hotfix | task | feature |
|-------|------|:------:|:----:|:-------:|
| SPEC | [spec.md](spec.md) | — | — (chat design in boot) | required |
| PLAN | [plan.md](plan.md) | issue + branch + PR only | light | full |
| BUILD | [build.md](build.md) | required | required | required |
| RELEASE | [release.md](release.md) | finish only | finish; docs optional | full menu |

Never skip a gate that the **chosen track** still requires. Resume at the earliest incomplete step for that track.

## Superpowers references

If Superpowers (or the project's `.agents/skills`) is installed, **read and follow** these as sub-skills — do not reinvent them:

| Phase | Prefer |
|-------|--------|
| SPEC (feature) | `brainstorming` architectural path |
| Chat design (task) | `brainstorming` bounded path (in-chat only; no spec file) |
| PLAN | `writing-plans` for task shape when a plan file exists; tracker always GitHub Issues |
| BUILD | `test-driven-development`, `verification-before-completion`, `systematic-debugging`; optional `subagent-driven-development` / `executing-plans` |
| RELEASE | `finishing-a-development-branch`; this pack adds docs/marketing when in scope |

Announce: `Using blaise → <track> → <phase>`.

## Boot sequence

1. **Announce** `/blaise`.
2. **Classify** hotfix | task | feature; state it; allow override.
3. **Resume?** If spec/plan/branch/PR/issues already exist, continue that track — don't re-SPEC.
4. **Repo facts:** default branch, `gh` auth, PR CI checks, test command.
5. **Tracker:** always GitHub Issues (`gh issue`). Optional Project only if the human names one.
6. **Enter the path for the track** (table above). For **task**: do the short in-chat design here, get yes, then PLAN/BUILD as needed. For **hotfix**: confirm the fix target in one sentence, then branch + BUILD.
7. Between required gates: **stop**; do not auto-advance on silence.

## Feature branch contract

- Branch: `feature/<slug>` or `fix/<slug>` (hotfix) from up-to-date default branch
- One umbrella PR (draft early is fine)
- Issues are task units; stack commits on that branch
- `Blocked by: #N` when multiple issues
- Parent/epic issue only when there are **multiple** child tasks (feature, or task with >1 issue). Hotfix: single issue is enough; close it when CI green and finished

## Stop conditions (any phase)

Stop and ask only for: irreversible/destructive ops, security-sensitive actions, push/merge to shared default branch without approval, or a plan so broken every path is a guess. Otherwise decide, ledger the ruling, continue.

## Progress ledger

```markdown
# <Work> — blaise progress
- Track: hotfix | task | feature
- Phase: SPEC | PLAN | BUILD | RELEASE | (skipped …)
- Spec: <path | none — task/hotfix>
- Plan: <path | none>
- Branch: feature/<slug> | fix/<slug>
- Umbrella PR: <url>
- Issues: …
- Ruling: …
```
