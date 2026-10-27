---
name: release-readiness-brief
description: "Call a change ready, ready with watch, needing evidence, needing a decision, or hold. Use when the question is whether to release, deploy, or cut over. Do not design the test plan or the done-proof contract. Use regression-risk-map for what could break and verification-contract for the evidence standard."
license: MIT
---

# Release Readiness Brief

Turn a release decision into one call: ship, ship with watch, gather evidence, decide an open question, or hold.

## Boundaries

Do not review code quality, replace `verification-contract`, or map regressions. That is `regression-risk-map`. If the user only asked for a summary of the changes, do not approve a release. If there is no concrete change or release candidate, ask for the scope first.

## Release rule

Do not mark a release `READY` unless the intended outcome, included scope, proof, rollback or fallback, owner, and watch decision are explicit. If any are missing, use the highest-precedence non-ready verdict and name the gap.

## Verdicts

Use exactly one, in this order:

1. **HOLD.** Unacceptable risk, a known blocker, no rollback for a risky change, or this is not the intended candidate.
2. **NEEDS DECISION.** A product, operational, timing, ownership, rollout, compatibility, or customer-impact decision is missing. Evidence may already exist.
3. **NEEDS EVIDENCE.** The release might be reasonable, but proof is missing or too vague.
4. **READY WITH WATCH.** Acceptable only with named signal, owner, duration, and rollback or mitigation trigger. Not a place to hide missing evidence or an open blocker.
5. **READY.** Named proof, and no watch beyond normal operation.

## Gates

Check intent, scope (including migrations, config, generated assets, docs, and flags), evidence, compatibility, rollback or fallback, watch, communication, and owner. Unknown is not ready.

Evidence names what was checked, where the result came from, and what pass means.

Rollback answers the action, the owner, the trigger, and the time sensitivity. A high-risk irreversible change without an accepted mitigation is `HOLD`.

A watch plan needs signal, owner, duration, and trigger. A missing field is `NEEDS EVIDENCE` or `NEEDS DECISION`, not `READY WITH WATCH`.

## Output

```markdown
## Release Readiness Brief

Verdict: HOLD | NEEDS DECISION | NEEDS EVIDENCE | READY WITH WATCH | READY

Release intent:
- ...

Included scope:
- ...

Excluded scope:
- ...

Evidence ledger:
| Claim or risk | Evidence source | Pass condition | Status |
| --- | --- | --- | --- |
| ... | ... | ... | proven / missing / not applicable |

Open decisions:
- ...

Risk and mitigation:
| Risk | Severity | Mitigation | Release impact |
| --- | --- | --- | --- |
| ... | low / medium / high | ... | proceed / watch / hold |

Rollback or fallback:
- Path:
- Owner:
- Trigger:
- Time sensitivity:

Watch plan:
- Signal:
- Owner:
- Duration:
- Trigger:

Communication:
- ...

Release call:
- ...

Next action:
- Release | Release with watch | Gather evidence | Make decision | Hold
```

Omit an empty optional block only when the omission cannot hide risk. A tiny reversible change stays compact and still has verdict, evidence, fallback, and next action.

## When the draft misses the contract

Read [references/examples.md](references/examples.md) and revise once. Read it only when the verdict skips an open blocker, evidence is "tests pass," or rollback has no trigger. A draft that already matches this contract does not need the file.
