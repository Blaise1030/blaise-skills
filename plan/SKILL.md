---
name: plan
description: >-
  PLAN phase of /blaise: create GitHub Issues, a stacked branch, and an
  umbrella PR — full plan file for feature track, light/single-issue for
  task/hotfix. Use when /blaise is in PLAN or the user wants issue-backed
  planning on one feature branch.
disable-model-invocation: true
---

# plan

Output: GitHub Issues + branch + umbrella PR (draft OK). Plan **file** depth depends on track (see [blaise](../blaise/SKILL.md)).

| Track | Input | Plan file | Issues |
|-------|-------|-----------|--------|
| **feature** | approved written spec | full `writing-plans` style | parent + one per task |
| **task** | approved in-chat design | only if >1 task or steps non-obvious; else skip file | usually one issue |
| **hotfix** | one-sentence fix target (or existing `#N`) | skip | one issue (or reuse `#N`) |

## Sub-skill

If available and a plan file is warranted, follow **`writing-plans`** for task shape. Ignore multi-PR handoff menus — /blaise stacks on one branch. Tracker always **GitHub Issues**.

## Steps

### 1. Write the plan (feature; optional task)

**hotfix:** skip this step.  
**task with a single obvious issue:** skip the file; put acceptance criteria in the issue body.  
**feature (or multi-task):** save to `docs/superpowers/plans/YYYY-MM-DD-<feature>.md` (or project plan dir).

Header must include:

```markdown
# <Feature> Implementation Plan

> **/blaise:** Execute with build on branch `feature/<slug>`. Tracker: GitHub Issues.

**Goal:** …
**Architecture:** …
**Tech Stack:** …
**Spec:** <path>
**Branch:** `feature/<slug>`
**Umbrella PR:** <filled after open>
**Parent issue:** <filled after create>
```

Decompose into **ordered tasks** (not horizontal layers). Each task:

- Independently verifiable with tests
- Lists files, interfaces consumed/produced
- TDD steps: failing test → verify fail → implement → verify pass → commit
- `Depends on: Task N | none`

Task 1 = walking skeleton of the main flow when the feature is user-facing.

Self-review against the spec (coverage, type consistency, no transcript-of-code).

### 2. Create GitHub Issues (always)

Use `gh` against the **current repo**. Never switch to Linear/Jira.

**hotfix / single-issue task:** one issue (or comment on existing `#N`) with acceptance criteria + definition of done. No parent.

**feature / multi-task:**

1. **Parent issue** — title `[Feature] <name>`; body links spec + plan; lists child task outline.
2. **One issue per task** — body includes:
   - Task section from the plan (verbatim or faithful)
   - Definition of done: tests pass locally, umbrella PR CI green for the commits that close this, issue closable
   - `Blocked by: #N` for dependencies (omit if none)
3. Write every issue number/URL back into the plan (if any).
4. Optional: if the human named a **GitHub Project**, add the issues. Do not create a project unasked.

### 3. Feature branch + umbrella PR

```bash
git fetch origin
git checkout <default-branch>
git pull
git checkout -b feature/<slug>
git push -u origin HEAD
gh pr create --draft --title "<Feature>" --body "$(cat <<'EOF'
## Summary
Implements <feature> per spec/plan.

- Spec: <path>
- Plan: <path>
- Parent: #<n>
- Tasks: #a, #b, …

## Status
Stacked on `feature/<slug>`. Issues close as tasks land; CI must stay green.

EOF
)"
```

If the branch already exists (resume), reuse it and the open PR.

Link the PR on the parent issue and in the plan header.

### 4. Gate

Present what exists (plan path if any, issue list, branch, PR URL). Ask:

> Ready to start BUILD?

**hotfix:** if the human already said "just fix it" and the issue/PR exist, proceed without a second gate.  
**task / feature:** **Stop** until explicit approval. Then hand off to [build](../build/SKILL.md).

## Anti-patterns

| Mistake | Fix |
|---------|-----|
| One PR per task off main | Stay on `feature/<slug>`; stack commits |
| Tracker other than GitHub Issues | Always `gh issue` |
| "Foundation/types-only" first task | Fold into walking skeleton |
| Creating issues before plan self-review | Review plan, then create |
| Opening BUILD without approval | Wait |
