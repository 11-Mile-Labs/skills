---
name: test-failure-signal-brief
description: "Classify a failing check before editing code or assertions. Use when a test, build, or quality gate fails and the next step is unclear. Do not debug, patch, or turn a user report into a reproduction brief. Use bug-repro-brief for messy reports."
license: MIT
---

# Test Failure Signal Brief

Classify what a failing check currently proves, what it does not prove, and the safest next investigation step. Then stop, unless the user asks to continue.

## Boundaries

Do not debug root cause, patch product code, rewrite assertions, write new tests, map regressions, define completion evidence, or turn an external bug report into a repro brief. That last job is `bug-repro-brief`.

## Signal rule

A verdict is a source-backed working classification for the next step, not a final cause. If the minimum evidence is missing or stale, use `INSUFFICIENT SIGNAL` and state any candidate separately.

## Verdicts

Use exactly one per distinct failure, in this order:

1. **INSUFFICIENT SIGNAL.** Evidence is missing, stale, contradictory, or too vague.
2. **ENVIRONMENT/SETUP.** The check did not reach the behavior under test, or setup, dependency, configuration, permissions, fixtures, or preconditions failed.
3. **FLAKY/INTERMITTENT.** The same check on comparable code and input has recorded pass and fail outcomes across at least two runs, or supplied history shows that.
4. **TEST BUG.** The expectation, fixture, harness, or assertion conflicts with a stronger intent source.
5. **PRODUCT BUG.** The failure contradicts source-backed intended behavior.

A single failure is never `FLAKY/INTERMITTENT`. `TEST BUG` outranks `PRODUCT BUG` unless a stronger source for intended behavior exists than the failing expectation. A failing assertion shows what the test expects, not that the expectation is correct.

Before any verdict other than `INSUFFICIENT SIGNAL`, cite the failure excerpt and the source the classification needs. If freshness is unknown, the action verdict is `INSUFFICIENT SIGNAL`.

Ask for at most three missing blockers. Do not ask for the whole repository.

Classify distinct signals independently. Render at most the first three. If many failures share one setup cause, group them under one `ENVIRONMENT/SETUP` signal.

## Guardrails

| Verdict | Do not |
| --- | --- |
| `INSUFFICIENT SIGNAL` | Classify or edit before current evidence exists. |
| `ENVIRONMENT/SETUP` | Change product code before the setup failure is resolved. |
| `FLAKY/INTERMITTENT` | Call it fixed from one pass, or rerun until a preferred result appears. |
| `TEST BUG` | Rewrite the assertion without a stronger intent source. |
| `PRODUCT BUG` | Broaden into unrelated surfaces, or skip a focused reproduction. |

## Output

```markdown
## Test Failure Signal Brief

Verdict: INSUFFICIENT SIGNAL | ENVIRONMENT/SETUP | FLAKY/INTERMITTENT | TEST BUG | PRODUCT BUG
Candidate signal: ...

Evidence captured:
- Command/method:
- Scope:
- Failure excerpt:
- Freshness:
- Intent source:

What this proves:
- ...

What this does not prove:
- ...

Immediate surface:
- ...

Safest next step:
- ...

Do not do:
- ...
```

Do not include a fix, patch, assertion rewrite, new test, or broad rerun plan.

## When the draft misses the contract

Read [references/examples.md](references/examples.md) and revise once. Read it only when a stale failure was classified as a product bug, a single run was called flaky, or the brief proposes a fix. A draft that already matches this contract does not need the file.
