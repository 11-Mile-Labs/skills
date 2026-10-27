# Examples

Read this file only when a draft misses the output contract: a required section is missing, a verdict is unsupported, or a field was filled in without a source.

Do not read it when the draft already matches the contract.

## Good And Bad Examples

### Flaky Claims

Good:

```markdown
Verdict: INSUFFICIENT SIGNAL
Evidence captured: one failure of `saves profile`; no comparable passing run supplied.
What this does not prove: that the test is flaky.
Safest next step: rerun the focused check once on the same code and record pass/fail conditions.
Do not do: do not label flaky from a single failure.
```

Bad:

```markdown
Verdict: FLAKY/INTERMITTENT
Evidence captured: test failed once.
```

Bad because one failure does not show comparable pass/fail behavior across runs.

### Assertion Rewrites

Good:

```markdown
Verdict: INSUFFICIENT SIGNAL
Evidence captured: assertion expected `total=10`, actual `total=12`; no source for intended total supplied.
What this proves: the current assertion and behavior disagree.
What this does not prove: whether the product or the assertion is wrong.
Safest next step: inspect the fixture or acceptance text that defines the expected total.
```

Bad:

```markdown
Safest next step: update the expected total to 12.
```

Bad because it rewrites the assertion without a stronger source for intended behavior.

### Scope Broadening

Good:

```markdown
Immediate surface:
- `discount total` calculation in the failing checkout total check.
What this does not prove:
- that tax, shipping, or every checkout path is broken.
```

Bad:

```markdown
What this proves:
- checkout calculations are broken across the app.
```

Bad because it broadens one failing assertion into unrelated surfaces without evidence.

### Setup Failures

Good:

```markdown
Verdict: ENVIRONMENT/SETUP
Evidence captured: command failed before tests ran with "missing fixture file users.csv".
What this proves: the check cannot reach product behavior until the fixture precondition is restored.
Safest next step: restore or point the check at the expected fixture, then rerun the focused check.
```

Bad:

```markdown
Verdict: PRODUCT BUG
Evidence captured: test command failed because users.csv was missing.
```

Bad because a missing fixture is a setup/precondition failure, not evidence of product behavior.

### Stale Evidence

Good:

```markdown
Verdict: INSUFFICIENT SIGNAL
Candidate signal: likely TEST BUG if the current assertion still expects a removed field.
Safest next step: rerun the focused check on current code and inspect the current test source.
```

Bad:

```markdown
Verdict: TEST BUG
Evidence captured: pasted failure from last week.
```

Bad because stale pasted output cannot classify the current failure without a fresh run or current source check.

## Anti-Patterns

- **Fix jump:** proposes code changes before classifying the failure signal.
- **Assertion rewrite reflex:** changes expected values without intended-behavior evidence.
- **Single-failure flaky label:** calls a failure flaky from one failed run.
- **Rerun fishing:** reruns repeatedly until a preferred result appears.
- **Environment blindness:** treats setup, dependency, fixture, permission, or command failures as product behavior.
- **Scope explosion:** turns one failing assertion into a broad product claim.
- **Stale certainty:** classifies old or freshness-unknown output as current.
- **Intent guessing:** infers product intent from intuition rather than source.
- **Broad context grab:** asks for the whole repository instead of missing blockers.
- **Regression-map drift:** expands into affected callers, coverage gaps, or a verification matrix.

## Compact Worked Examples

### INSUFFICIENT SIGNAL

Verdict: INSUFFICIENT SIGNAL
Candidate signal: likely PRODUCT BUG if the supplied failure is from the current run and the acceptance text still requires saved preferences to persist.

Evidence captured: failure excerpt says preference is disabled after reload; command and freshness were not supplied.
What this proves: supplied output describes a persistence mismatch.
What this does not prove: that current code still fails or whether the expectation is current.
Immediate surface: saved preference reload check.
Safest next step: provide or rerun the focused command on current code and include the full failure excerpt.
Do not do: do not classify or edit before current signal is supplied.

### ENVIRONMENT/SETUP

Verdict: ENVIRONMENT/SETUP

Evidence captured: command failed before running checks with `permission denied opening test database`.
What this proves: the run cannot reach product behavior until database access is fixed.
What this does not prove: that persistence logic is broken.
Immediate surface: test database connection precondition.
Safest next step: fix or confirm database access, then rerun the focused persistence check.
Do not do: do not change product code before the setup failure is resolved.

### FLAKY/INTERMITTENT

Verdict: FLAKY/INTERMITTENT

Evidence captured: same focused check on the same branch passed at 10:03 and failed at 10:05 with the same input.
What this proves: the signal is unstable under comparable conditions.
What this does not prove: the root cause of instability.
Immediate surface: focused save-and-reload check.
Safest next step: capture timing, seed, input, and logs for the next comparable run.
Do not do: do not call it fixed from one later pass.

### TEST BUG

Verdict: TEST BUG

Evidence captured: assertion expects `status=archived`; current acceptance text says canceled items remain `status=canceled`.
What this proves: the assertion conflicts with a stronger intent source.
What this does not prove: that unrelated status transitions are wrong.
Immediate surface: canceled-item status assertion.
Safest next step: update the test expectation only after preserving the acceptance-text citation.
Do not do: do not broaden from this failure into unrelated status behavior.

### PRODUCT BUG

Verdict: PRODUCT BUG

Evidence captured: focused check shows saved email preference resets after reload; acceptance text says the saved preference persists after reload.
What this proves: current behavior contradicts the stated persistence requirement.
What this does not prove: that all settings persistence is broken.
Immediate surface: email preference save-and-reload behavior.
Safest next step: reproduce the focused failure once on current code, then debug that path.
Do not do: do not skip focused reproduction or expand to unrelated settings.

## Contrasts: Minimum Evidence

Good:

- `Verdict: INSUFFICIENT SIGNAL (stale failure evidence). Candidate signal: likely TEST BUG if the supplied assertion still matches current test source.`

Bad:

- `Verdict: PRODUCT BUG from yesterday's pasted failure.` Bad because stale evidence cannot safely classify current product behavior.
