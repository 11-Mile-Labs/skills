---
name: bug-repro-brief
description: "Turn a messy bug report into facts, assumptions, unknowns, and one reproduction confidence. Use before debugging or filing an unclear failure. Do not find the cause, write the fix, or classify a failing automated check. Use test-failure-signal-brief for a failing test or build."
license: MIT
---

# Bug Repro Brief

Separate observed facts from assumptions, decide how reproducible the bug is, and name the next evidence step. Do not invent missing details.

## Boundaries

Do not diagnose, propose a fix, run reproduction steps unless asked, decide claimability, or assign schedule priority from impact language. A failing automated check is `test-failure-signal-brief`. If a brief already exists for this bug, update it.

## Repro rule

Produce the brief first. Reporter testimony is a fact about the report, not verified product behavior. Mark it `reported, not verified`. Prior session context is not a fact unless it is in the source or re-verified now.

## Confidence

Use exactly one:

| Verdict | Use when |
| --- | --- |
| `CONFIRMED` | The team reproduced it, or supplied evidence has complete steps, environment, and a visible actual result. |
| `INTERMITTENT` | Credible evidence it happened, but reproduction is not deterministic, or a current attempt failed. |
| `NOT REPRODUCIBLE YET` | A documented attempt under recorded conditions did not reproduce it, and there is no credible observation. |
| `PLAUSIBLE` | One credible report is specific enough to attempt, and the behavior is not confirmed. |
| `INSUFFICIENT` | Missing core facts block a meaningful attempt. |

Never emit `CONFIRMED` from a single unverified report. Never emit `NOT REPRODUCIBLE YET` when there is a credible observation; that is `INTERMITTENT` unless deterministic reproduction exists. Third-party evidence that qualifies is `confirmed by supplied evidence`, not `confirmed locally`.

## Severity and priority

Severity is observed or reported impact if the bug is real, with confidence labeled. Priority is scheduling. Assign it only when an owner, deadline, policy, or customer commitment is present. Otherwise write `Priority: not assigned from repro brief`.

For `INTERMITTENT` only, include observations and evidence (verified or reported), what stayed constant versus what varied, the next capture strategy, and this line: `No root cause claimed from intermittent evidence.`

Ask only questions that unblock the next reproduction attempt.

## Output

```markdown
## Bug Repro Brief

Repro confidence: CONFIRMED | INTERMITTENT | NOT REPRODUCIBLE YET | PLAUSIBLE | INSUFFICIENT

Source material:
- ...

Facts:
- ...

Assumptions:
- ...

Unknowns:
- ...

Expected behavior:
- ...

Actual behavior:
- ...

Environment and data conditions:
- ...

Frequency:
- ...

Severity: ...
Priority: ...

Repro steps:
1. ...

Repro attempts run:
- ...

Next repro attempt:
- ...

INTERMITTENT discipline:
- Include only when the verdict is INTERMITTENT.

Evidence to attach or capture:
- ...

Clarifying questions (repro-blocking only):
- ...

Next action:
- ...
```

Required gates say `None`, `not applicable`, or `not assigned from repro brief` instead of disappearing. Omit the intermittent section unless that is the verdict.

## When the draft misses the contract

Read [references/examples.md](references/examples.md) and revise once. Read it only when a report was treated as verified fact, priority was inferred from severity, or the confidence contradicts the guard clauses. A draft that already matches this contract does not need the file.
