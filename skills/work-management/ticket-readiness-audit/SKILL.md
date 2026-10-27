---
name: ticket-readiness-audit
description: "Decide whether a ticket is ready to claim, needs clarification, should be split, is blocked, or should be sent back. Use before starting implementation. Do not write the acceptance criteria or the evidence contract. If the verdict is split, name the slices and stop. Use work-slice-splitter for the slice plan."
license: MIT
---

# Ticket Readiness Audit

A ticket is ready to claim only when a competent implementer can start without guessing, stay inside scope, see blockers, and recognize done.

## Boundaries

Do not create a ticket, write a PRD, estimate, prioritize, or build a full verification matrix. Pass/fail wording is `acceptance-criteria-tightener`. Proof of done is `verification-contract`. A full slice plan is `work-slice-splitter`. This audit may name slices when the verdict is `SPLIT`, then stop. If an audit already exists for this scope, update it.

## Readiness rule

Do not mark a ticket ready unless the claimability test passes and no higher verdict applies. Do not fill missing outcome, scope, proof path, or ownership from assumptions and then mark it ready.

## Verdicts

Use exactly one, in this order:

1. **DO NOT CLAIM.** Stale, duplicate, invalid, obsolete, or no longer aligned. Send it back for rewrite, closure, or archive.
2. **BLOCKED.** An external dependency, access need, decision, environment, or owner response prevents progress.
3. **SPLIT.** Too broad, and the outcome is clear enough to name slices without inventing intent. Name them and stop.
4. **NEEDS CLARIFICATION.** Missing information prevents a safe start or a safe split. Use this instead of `SPLIT` when slicing would require guessing.
5. **READY.** Nothing above applies, and the claimability test passes.

For every verdict below DO NOT CLAIM, name only the higher-precedence verdicts ruled out.

## Gates and claimability

The ticket should state outcome, value, scope in and out, constraints, dependencies, one observable done signal, an owner, and known blockers or an explicit none.

Any "no" prevents READY:

1. Can an implementer start without guessing the outcome?
2. Can they stay in scope without inventing unspecified details?
3. Is there at least one observable done signal, short of a full verification matrix?
4. Are dependencies, blockers, access, and environment or data needs named or explicitly absent?
5. Is it clear who answers questions mid-work?

## Output

```markdown
## Ticket Readiness Audit

Verdict: READY | NEEDS CLARIFICATION | SPLIT | BLOCKED | DO NOT CLAIM
Ruled out: ...

Ticket snapshot:
- Outcome:
- In scope:
- Out of scope:
- Dependencies and access needs:
- Proof path or done signal:
- Owner or decision maker:

Claimability Test:
1. ...
2. ...
3. ...
4. ...
5. ...

Missing information:
| Missing item | Impact if claimed now | Question to ask | Who answers |
| --- | --- | --- | --- |
| ... | ... | ... | ... |

Split suggestions:
- Slice: outcome; scope boundary; likely proof path; remaining question if any.

Readiness summary:
- ...

Next action:
- ...
```

Always include the missing-information table. Use a none row when nothing is missing. Omit split suggestions unless the verdict is SPLIT.

## When the draft misses the contract

Read [references/examples.md](references/examples.md) and revise once. Read it only when READY coexists with a guessed outcome, or SPLIT invents product intent. A draft that already matches this contract does not need the file.
