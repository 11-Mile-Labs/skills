---
name: work-slice-splitter
description: "Split broad work into independently claimable outcome slices. Use when a request, epic, or plan is too big to start. Do not decide what to cut or whether a ticket is claimable. Use scope-check to cut scope and ticket-readiness-audit before claiming a slice."
license: MIT
---

# Work Slice Splitter

Turn broad work into independently claimable outcome slices. This creates a slice plan. It does not decide whether each slice is ready to claim.

## Boundaries

Do not use this when the work is already one claimable outcome, slicing would orphan in-flight changes, the request is a research spike with no outcome, or an urgent hotfix has one safe outcome. Cutting scope is `scope-check`. Claimability of a resulting slice is `ticket-readiness-audit`. If a readiness audit already suggested slices, reconcile those suggestions instead of replacing them silently.

## Slice rule

Each slice has an observable user, business, or enabling outcome, scope in and out, dependencies or blockers, one lightweight done signal, and a sequence label. Do not split below an observable outcome. A done signal is a result a reviewer can observe, not an activity such as "write tests."

## Verdicts

Use exactly one:

| Verdict | Use when |
| --- | --- |
| `SLICE` | At least one independent outcome can be sliced safely. Clear slices may ship while the unclear remainder is deferred. |
| `CLARIFY BEFORE SLICING` | Unknowns affect every slice, or product intent is too vague to form a safe outcome. |
| `DO NOT SPLIT` | One claimable outcome, tiny work, an urgent single-outcome hotfix, or splitting would be busywork. |

For `CLARIFY BEFORE SLICING`, do not invent tentative slices. State why slicing is unsafe, the known facts, at most three blocking questions, and what would unlock slicing.

## Shape

Prefer vertical outcome slices over component chores. Allowed: user-facing outcomes, business outcomes, enabling slices that preserve observable behavior, and risk-reducing prerequisites. Not allowed: backend, frontend, and tests as separate slices with no outcome, activity chores, slices that share a done signal, or slices that cannot be reviewed independently.

Target 2-5 slices. More than 5 needs a reason such as independent workflows or blockers. Merge slices that share a done signal or cannot be stopped independently.

Always name the first safe slice, chosen in order: blocker-reducing prerequisite, lowest blast radius, earliest user or business value. If several qualify, name the criterion used.

Sequence is `first safe`, `parallelizable`, `blocked`, `later`, or `deferred-needs-clarification`.

```markdown
Slice: ...
Outcome: ...
Scope in: ...
Scope out: ...
Dependencies/blockers: ...
Done signal: ...
Sequence: first safe | parallelizable | blocked | later | deferred-needs-clarification
```

## Output

```markdown
## Work Slice Splitter

Verdict: SLICE | CLARIFY BEFORE SLICING | DO NOT SPLIT

Source facts:
- ...

Slice plan:
1. ...

Deferred / needs clarification:
- ...

First safe slice:
- ...

Parallelizable slices:
- ...

Blocked slices:
- ...

Prior slice reconciliation:
| Prior suggestion | Action | Reason |
| --- | --- | --- |
| ... | kept / refined / dropped / added | ... |

Next step:
- Run ticket-readiness-audit on each slice before claiming it.
```

For `DO NOT SPLIT`, replace the slice plan with the reason and the single done signal if known. Omit prior-slice reconciliation when there are no prior suggestions.

## When the draft misses the contract

Read [references/examples.md](references/examples.md) and revise once. Read it only when a slice is a component chore, slices share a done signal, or product intent was invented. A draft that already matches this contract does not need the file.
