---
name: plan
description: >-
  PLAN phase of /blaise: turn an approved spec into a stacked implementation
  plan, GitHub Issues (parent + ordered tasks), a feature branch, and an
  umbrella PR. Use when /blaise is in PLAN or the user wants issue-backed
  planning on one feature branch.
disable-model-invocation: true
---

# plan

Input: **approved spec**. Output: plan file + GitHub Issues + `feature/<slug>` + umbrella PR (draft OK).

## Sub-skill

If available, follow **`writing-plans`** for task shape (TDD steps, interfaces, checkboxes). Ignore its multi-PR / subagent handoff menus when they conflict — /blaise uses **one stacked feature branch** and always **GitHub Issues**.

## Steps

### 1. Write the plan

Save to `docs/superpowers/plans/YYYY-MM-DD-<feature>.md` (or project plan dir).

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

1. **Parent issue** — title `[Feature] <name>`; body links spec + plan; lists child task outline.
2. **One issue per task** — body includes:
   - Task section from the plan (verbatim or faithful)
   - Definition of done: tests pass locally, umbrella PR CI green for the commits that close this, issue closable
   - `Blocked by: #N` for dependencies (omit if none)
3. Write every issue number/URL back into the plan.
4. Optional: if the human named a **GitHub Project**, add the issues (`gh project item-add` or UI instructions). Do not create a project unasked.

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

Present plan path, issue list (with blockers), branch, PR URL. Ask:

> Plan, issues, and draft PR are ready. Approve to start BUILD?

**Stop** until explicit approval. Then hand off to [build](../build/SKILL.md).

## Anti-patterns

| Mistake | Fix |
|---------|-----|
| One PR per task off main | Stay on `feature/<slug>`; stack commits |
| Tracker other than GitHub Issues | Always `gh issue` |
| "Foundation/types-only" first task | Fold into walking skeleton |
| Creating issues before plan self-review | Review plan, then create |
| Opening BUILD without approval | Wait |
