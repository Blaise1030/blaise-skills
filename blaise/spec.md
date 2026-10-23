# SPEC

Produce a **written spec** the human explicitly approves. No plan, issues, or product code in this phase.

## When this phase applies

Only the **feature** track (see [SKILL.md](SKILL.md) classify). If track is hotfix or task, **do not enter this phase** — return to the orchestrator path (chat design or straight to PLAN/BUILD).

## Sub-skill

If available, follow **`brainstorming`** (architectural path). This phase only adds /blaise paths and the exit gate.

## Steps

1. **Explore** the repo: existing flows, docs, recent commits, constraints.
2. **Clarify** purpose, users, success criteria, out of scope — one focused question at a time (batch only when the human asked for speed).
3. **Approaches** — for non-trivial work, offer 2–3 options with trade-offs and a recommendation; YAGNI.
4. **Design sections** — architecture, flows (`actor does X → sees Y`), data, errors, testing, non-goals. Get assent per section when large.
5. **Write the spec** to:

   `docs/superpowers/specs/YYYY-MM-DD-<topic>-design.md`

   (If the project already uses another specs dir, use that. /blaise default above.)

6. **Self-review** — kill TBD/TODO, contradictions, ambiguous requirements, oversized scope (split features if needed).
7. **Gate** — ask the human to review the file. **Stop.** Do not invoke PLAN until they approve the written spec.

## Spec must include

- Goal and success criteria
- User-visible flows as `actor does X → sees Y`
- Out of scope
- Constraints (platform, deps, compatibility)
- Test expectations (unit / e2e / manual)
- Open questions resolved or explicitly deferred

## Exit

Hand off to [plan.md](plan.md) only after explicit approval of the spec file.
