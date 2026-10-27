---
name: decision-record-snapshot
description: "Record one decision with its source, scope, and revisit trigger, without inventing missing facts. Use when a choice must stay true after the conversation. Do not stress-test the plan, define done, or split the work. Use devil-advocate before the choice is made."
license: MIT
---

# Decision Record Snapshot

Preserve what was decided, why, where it applies, and when to revisit it. A later reader should know whether the decision is active, proposed, deferred, superseded, or still missing.

## Boundaries

Do not write a PRD or architecture record, decide claimability, define proof of done, or stress-test a choice that has not been made. Stress-testing is `devil-advocate`. If there is only tradeoff talk and no choice, the status is `NEEDS DECISION`. Do not fabricate `DECIDED`.

## Snapshot rule

A valid snapshot preserves status, source, rationale, scope, and revisit trigger. Missing fields stay `unknown` or `not stated`. Do not invent decider, owner, date, source, options, rejected options, scope, reversibility, or supersession.

## Statuses

Use exactly one:

| Status | Use when |
| --- | --- |
| `DECIDED` | A real choice is asserted or source-backed, including a choice not to do something. |
| `PROPOSED` | A preferred option exists and has not been accepted. |
| `NEEDS DECISION` | Options are known, and work cannot proceed safely without a choice. |
| `DEFERRED` | An explicit choice not to decide until a named trigger. |
| `SUPERSEDED` | A prior decision was replaced. Identify the newer decision, or mark it unknown. |

A decision not to do something is `DECIDED` with a negative statement. "Make it final" is not itself a decision. Do not add a `REJECTED` status.

Write the decision as a self-contained active statement with the source nearby. Avoid "we decided," which goes stale when copied.

One snapshot is one decision. Name scope in and scope out. Do not silently widen a later prompt into the original decision.

Reversibility is `easy to reverse`, `hard to reverse`, `one-way door`, or `unknown/not assessed`. Unknown reversibility does not block `DECIDED`. Add a follow-up when the choice looks high-impact.

Decider and owner stay separate. Either may be unknown.

Use the compact snapshot for a tiny, low-risk, current decision. Use the full snapshot when the decision is consequential, disputed, hard to reverse, stale, or superseding.

## Output

Compact:

```markdown
## Decision Record Snapshot

Status: DECIDED | PROPOSED | NEEDS DECISION | DEFERRED | SUPERSEDED
Decision: ...
Source/date: ...
Rationale: ...
Scope in: ...
Scope out: ...
Revisit trigger: ...
```

Full adds context, options considered (`not stated` if absent), rejected options, decider, owner, reversibility, affected work, unknowns, and supersession. For `NEEDS DECISION`, include the options so far and the smallest blocking question.

## When the draft misses the contract

Read [references/examples.md](references/examples.md) and revise once. Read it only when a date, owner, or option was invented, or the statement is "we decided." A draft that already matches this contract does not need the file.
