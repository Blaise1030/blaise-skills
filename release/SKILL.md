---
name: release
description: >-
  RELEASE phase of /blaise: when the feature is implementation-complete,
  update documentation and marketing materials, then finish the feature branch
  (merge/PR/keep). Use when /blaise is in RELEASE or the feature is ready to
  ship externally.
disable-model-invocation: true
---

# release

Input: BUILD complete (issues closed, umbrella PR CI green).  
Output: release artifacts (docs/marketing as needed) + human-chosen integrate path.

## Sub-skill

For branch integration options, follow **`finishing-a-development-branch`** (verify tests → present merge / push-PR / keep menu). This skill runs **before** that menu when docs or marketing are in scope.

## Steps

### 1. Confirm BUILD exit

Fresh evidence only:

- Local full test suite
- `gh pr checks` on the umbrella PR — all required green

If not green, return to [build](../build/SKILL.md).

### 2. Release scope check

Ask once (skip if the human already specified):

> RELEASE — what should we produce beyond merging?
> 1. User-facing docs only  
> 2. Docs + marketing (changelog, launch note, social/blog draft)  
> 3. Skip artifacts — just finish the branch  
> 4. Custom: …

### 3. Documentation (if in scope)

Update what the feature actually changed:

- README / product docs / API docs / `docs/` guides
- In-app copy or help only if the spec called for it
- Keep accurate; no vapor features

Prefer same branch commits so the umbrella PR carries docs. Title commits `docs: …`.

### 4. Marketing materials (if in scope)

Draft only what they asked for, in-repo or as gist/files they prefer:

| Artifact | Default location |
|----------|------------------|
| Changelog entry | `CHANGELOG.md` or repo convention |
| Launch / release notes | `docs/releases/YYYY-MM-DD-<feature>.md` |
| Blog or social draft | `docs/releases/<feature>-announce.md` (clearly marked draft) |

Pin claims to shipped behavior in the PR diff. No aspirational roadmap language.

### 5. Parent issue + PR

- Update umbrella PR body: docs/marketing links, "Ready for review/ship"
- Comment on parent issue with release artifact paths
- Mark draft PR ready when appropriate: `gh pr ready`

### 6. Finish the branch

Invoke **`finishing-a-development-branch`**: present the standard menu (merge locally / push PR / keep). Wait for the human.

After merge (if chosen): close the **parent** GitHub issue; ensure child issues are closed.

### 7. /blaise complete

Ledger final state: phase RELEASE done, PR/issue URLs, artifact paths. Stop.

## Anti-patterns

| Mistake | Fix |
|---------|-----|
| Marketing before CI green | Confirm BUILD first |
| Promising unshipped behavior | Diff-backed claims only |
| Merging without the finish menu | Human chooses |
| Closing parent issue mid-BUILD | Only after RELEASE integrate or explicit waive |
