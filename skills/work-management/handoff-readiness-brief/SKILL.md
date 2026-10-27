---
name: handoff-readiness-brief
description: "Make in-progress work resumable without guessing state, protected files, evidence, or the next step. Use before pausing or handing work off. Do not decide whether a ticket is claimable, define done, or map regressions. Use ticket-readiness-audit, verification-contract, or regression-risk-map for those."
license: MIT
---

# Handoff Readiness Brief

A handoff is a continuation aid for the next worker, not a status update.

## Boundaries

Do not decide whether a ticket is claimable (`ticket-readiness-audit`), define done (`verification-contract`), or map regressions (`regression-risk-map`). Do not produce a changelog. If a handoff already exists for this work, update it.

## Handoff rule

READY only when a competent next worker can resume without guessing current state, overwriting protected work, trusting false evidence, or wondering what to do first.

## Verdicts

Use exactly one, in this order:

1. **INCOMPLETE.** The work is not at a coherent pause point. No `Ruled out` line.
2. **BLOCKED.** The state is coherent, and an external dependency, access need, decision, or owner response prevents continuation.
3. **NEEDS REFRESH.** Coherent and not externally blocked, but freshness or verification is stale or unproven against the latest state.
4. **READY.** Nothing above applies, and all four gates pass.

For every verdict below INCOMPLETE, name only the higher-precedence verdicts ruled out. Never mark READY while known failures, blockers, hidden skipped verification, or protected-work risks remain.

## Four gates

| Gate | Required |
| --- | --- |
| Current state | What is true now. Decaying state needs `Verified at` or `Verify before acting`. |
| Resumption surface | Changed files or artifacts. Protected entries name what, why, and how to verify they are still present. |
| Evidence state | Command or method, result, and source. "Tested it" and "all green" are invalid without a source. |
| Next action | The smallest state-preserving first step. A mutating or irreversible first step must be labeled and justified. |

Phrases such as should be fine, mostly working, looks good, tested it, all green, and just need to must be converted into source-backed state, or the point stays unresolved.

Declare one audience: `same-agent continuation`, `human handoff`, or `reviewer handoff`. If unstated, use `human handoff (defaulted)`. Render only that audience's line. Do not ask which audience applies before writing the brief.

## Output

```markdown
## Handoff Readiness Brief

Verdict: INCOMPLETE | BLOCKED | NEEDS REFRESH | READY
Ruled out: ...

Audience: same-agent continuation | human handoff | reviewer handoff

Goal:
- ...

Current state:
- Status:
- Branch / artifact:
- Verified at:
- Verify before acting:
- Stable because:

Resumption surface:
- Changed files / artifacts:
- Protected (do not overwrite):

Evidence state:
| Check | Command or method | Result | Source / excerpt |
| --- | --- | --- | --- |
| ... | ... | passed / failed / not run | ... |

Known failures or blockers:
- ...

Open assumptions / unknowns:
- ...

Audience-specific:
- Already tried: (same-agent continuation only)
- Decision rationale: (human handoff only)
- Review focus: (reviewer handoff only)

Next action:
- First action:
- Stale-state refresh needed before acting:

What not to do:
- ...
```

The evidence table is always present. If nothing ran: `| No checks run | not run | verification skipped because ... | n/a |`. If no failures are known, write `None known from supplied state`. Include a non-omission note only when the request asks to hide failures, blockers, skipped verification, or protected work. Do not prescribe where to store the brief.

## When the draft misses the contract

Read [references/examples.md](references/examples.md) and revise once. Read it only when evidence is unsourced, protected work has no verify step, or READY coexists with a known failure. A draft that already matches this contract does not need the file.
