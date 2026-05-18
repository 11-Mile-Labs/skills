---
name: test-failure-signal-brief
description: "Use when a test, build check, validation run, or automated quality gate fails and the next step is to classify the failure signal before editing."
argument-hint: "[optional: failing command output, check summary, or test failure note]"
license: MIT
---

# Test Failure Signal Brief

Use this skill to turn failing check output into a small signal brief before changing code. The goal is to classify what the failure currently proves, what it does not prove, and the safest next investigation step.

This skill stops before debugging or fixing. It prevents reflexive edits, weak flaky claims, assertion rewrites without intent evidence, and broad investigations from one failure.

## When To Use

Use this skill when a test, build check, validation run, quality gate, or focused reproduction command fails and the next step is unclear.

Use it before:

- Changing product code because a test failed.
- Rewriting a test expectation.
- Calling a failure flaky.
- Deciding whether a failure is setup-related.
- Summarizing a failed run for a reviewer or next worker.

Do not use this skill to:

- Debug root cause.
- Patch product code.
- Rewrite assertions.
- Write new tests.
- Run a broad regression map.
- Define completion evidence.
- Turn external bug reports into repro briefs.

Deliver the brief, name the next investigation step, then stop unless the user explicitly asks to proceed.

## The Signal Rule

A failure verdict is only a source-backed working classification for the next investigation step, not a final root-cause claim.

If the minimum evidence is missing or stale, choose `INSUFFICIENT SIGNAL` and state the candidate signal separately.

## Verdicts And Precedence

Use exactly one verdict for each distinct failure signal, applying this precedence:

1. **INSUFFICIENT SIGNAL:** required evidence is missing, stale, contradictory, or too vague to classify safely.
2. **ENVIRONMENT/SETUP:** the check did not reach the behavior under test, or the failure is caused by setup, dependency, configuration, permissions, missing fixtures, or command preconditions.
3. **FLAKY/INTERMITTENT:** the same check on comparable code and input has recorded pass and fail outcomes across at least two runs, or supplied history shows intermittent behavior with conditions.
4. **TEST BUG:** the test expectation, fixture, harness, or assertion conflicts with a stronger intent source.
5. **PRODUCT BUG:** the failure contradicts a source-backed intended product behavior.

Conservative verdicts win. A single failure is never enough for `FLAKY/INTERMITTENT`.

`TEST BUG` outranks `PRODUCT BUG` unless there is a stronger source for intended behavior than the failing test expectation.

## Minimum Evidence

Capture these fields when available:

- Command or method.
- Working directory or environment, if relevant.
- Failing check name or scope.
- Exact assertion, error, or failure excerpt.
- Result or exit status.
- Freshness: current run, timestamp, commit, or explicit unknown.

If freshness is unknown, choose `INSUFFICIENT SIGNAL` for action and add a candidate signal if useful.

Good:

- `Verdict: INSUFFICIENT SIGNAL (stale failure evidence). Candidate signal: likely TEST BUG if the supplied assertion still matches current test source.`

Bad:

- `Verdict: PRODUCT BUG from yesterday's pasted failure.` Bad because stale evidence cannot safely classify current product behavior.

## Source Discipline

The skill may read narrow, relevant files when available. Do not read broadly.

Before issuing a non-`INSUFFICIENT SIGNAL` verdict, cite enough source for the classification:

- For `PRODUCT BUG` or `TEST BUG`: failure excerpt plus at least one intent source such as test source, assertion and fixture, acceptance text, product contract, docs, existing passing test, or contract-defining comment.
- For `ENVIRONMENT/SETUP`: startup, dependency, configuration, permission, missing fixture, or precondition error may be enough.
- For `FLAKY/INTERMITTENT`: recorded comparable pass/fail evidence with conditions is required.

A failing assertion alone shows what the test expects, not whether the expectation is correct.

Understanding intended behavior requires source-backed expectation, not intuition.

## Missing Evidence Requests

When evidence is missing, ask only for missing blockers. Ask for at most three items at a time from this checklist:

- Full command and working directory.
- Full assertion or error excerpt with line numbers.
- Failing test or check source.
- Product source, contract, doc, or acceptance text for intended behavior.
- Recent change scope.
- Current run timestamp, commit, or freshness marker.

Do not ask for the whole repository or broad context by default.

## Multi-Failure Runs

Classify distinct failure signals independently.

Render at most the first three distinct failure signals. If more remain, add a `More failures` note and ask which remaining signal to prioritize.

If many failures share one setup or precondition cause, group them under one `ENVIRONMENT/SETUP` signal instead of repeating the same brief.

## Output Format

Use the same headings every time. Keep each section to one line when the failure is tiny.

```markdown
## Test Failure Signal Brief

Verdict: INSUFFICIENT SIGNAL | ENVIRONMENT/SETUP | FLAKY/INTERMITTENT | TEST BUG | PRODUCT BUG
Candidate signal: <only when verdict is INSUFFICIENT SIGNAL and a useful candidate exists>

Evidence captured:
- Command/method:
- Scope:
- Failure excerpt:
- Freshness:
- Intent source:

What this proves:
- <source-backed conclusion>

What this does not prove:
- <limits of the signal>

Immediate surface:
- <at most one failing check, module, file, fixture, or contract>

Safest next step:
- <one state-preserving next investigation step>

Do not do:
- <verdict-scoped guardrail>
```

Do not include a fix proposal, patch, assertion rewrite, new test, debugger plan, or broad rerun loop in the brief.

## Do-Not-Do Mapping

Use only the guardrails relevant to the verdict and prompt pressure.

| Verdict | Required guardrail |
| --- | --- |
| `INSUFFICIENT SIGNAL` | Do not classify or edit before missing or current signal is supplied. |
| `ENVIRONMENT/SETUP` | Do not change product code before setup, dependency, fixture, permission, or precondition failure is resolved. |
| `FLAKY/INTERMITTENT` | Do not call it fixed from one pass, and do not rerun until a preferred result appears. |
| `TEST BUG` | Do not rewrite assertions without a stronger intent source than the failing expectation. |
| `PRODUCT BUG` | Do not broaden from this failure into unrelated surfaces, and do not skip focused reproduction. |

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

## Tune When

Tune this skill when outputs:

- Propose fixes, patches, assertion rewrites, new tests, debugger steps, or broad reruns inside the brief.
- Mark `FLAKY/INTERMITTENT` from one failure.
- Mark `PRODUCT BUG` or `TEST BUG` without a source-backed intent reference.
- Treat setup, dependency, fixture, permission, or command errors as product behavior.
- Classify stale or freshness-unknown output as current.
- Ask for broad context instead of at most three missing blockers.
- Render more than three distinct failure signals.
- Broaden one failure into a regression map.
- Omit verdict-scoped `Do not do` guidance.
- Include bad examples without an explicit `Bad because ...` reason.
