---
name: build
description: >-
  BUILD phase of /blaise: TDD implementation of planned GitHub Issues on one
  stacked feature branch, watching the umbrella PR's GitHub Actions until all
  tasks are done. Use when /blaise is in BUILD or the user wants to execute a
  blaise plan against CI.
disable-model-invocation: true
---

# build

Input: approved plan + GitHub Issues + `feature/<slug>` + umbrella PR.  
Output: all tasks implemented (TDD), issues closed, **PR CI green**.

## Sub-skills

| Need | Prefer if available |
|------|---------------------|
| Every behavior | `test-driven-development` (iron law) |
| Before "done/fixed/green" claims | `verification-before-completion` |
| Failures | `systematic-debugging` |
| Execution style | `subagent-driven-development` (fresh agent per task) or `executing-plans` (inline) — ask once if unset; default **inline** on mid-size plans |

## Loop

Work **ready** issues only: every `Blocked by` issue is closed.

For each ready issue, in plan order:

1. **Announce** issue `#N` and move it In Progress (comment or project status if used).
2. **TDD the task** exactly per plan steps — no production code without a failing test first.
3. **Commit** on `feature/<slug>` (message references `#N`).
4. **Push** and watch umbrella PR checks:

   ```bash
   gh pr checks <pr> --watch
   # or: gh run list --branch feature/<slug> && gh run view <id> --log-failed
   ```

5. **CI red** → fix on the same branch (TDD/debug); re-push; watch again. Do not close the issue.
6. **CI green** for the new work and local full test suite green → close issue (`gh issue close #N`), check off plan boxes, update ledger.
7. Recompute ready issues; continue. **Do not stop** between tasks for "should I continue?" unless a global stop condition hits.

Parallelize only independent ready issues if using subagents **and** they share the same feature branch safely (coordinate pushes; prefer serial commits if contention). Default: **serial** on one branch.

## Definition of done (feature BUILD)

All of:

- [ ] Every child issue closed
- [ ] Plan checkboxes complete
- [ ] Local full test suite green (fresh run)
- [ ] Umbrella PR: all required GitHub Actions checks green (fresh `gh pr checks`)
- [ ] PR body lists issues and points at spec/plan

Then hand off to [release](../release/SKILL.md). Do not merge to default branch in BUILD.

## Anti-patterns

| Mistake | Fix |
|---------|-----|
| New PR per issue | Stack on `feature/<slug>` |
| Closing issue while CI red | Fix CI first |
| Tests after implementation | Delete; restart TDD |
| Claiming green from an old run | Re-run / re-watch |
| Merging in BUILD | RELEASE (+ human) owns integrate |
