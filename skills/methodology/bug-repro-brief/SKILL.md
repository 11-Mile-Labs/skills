---
name: bug-repro-brief
description: "Use when turning messy bug reports, QA notes, support complaints, screenshot descriptions, or intermittent failures into reproduction briefs; when asking whether a bug is deterministic; or when identifying evidence still needed for reproduction."
argument-hint: "[bug report, QA note, or support complaint]"
license: MIT
---

# Bug Repro Brief

## Overview

Use this skill to turn messy bug intake into a clear reproduction brief before debugging, fixing, ticketing, or prioritizing the work.

The goal is to separate observed facts from assumptions, decide how reproducible the bug is, and name the next evidence step without inventing missing details.

## When To Use

Use this when the input is a bug report, support complaint, QA note, screenshot description, intermittent failure, or question like "can we reproduce this?" or "what evidence do we still need?"

Do not use this to:

- Diagnose root cause.
- Propose or implement a fix.
- Execute reproduction steps by default.
- Decide whether the resulting work item is claimable.
- Assign schedule priority from impact language alone.

Once the brief is complete, hand off to debugging if the next action is root-cause work. Hand off to work-item readiness review if the next action is filing or claiming work.

If a prior reproduction brief exists for the same bug, update or reconcile it. Do not create a competing brief.

## The Repro Rule

Do not diagnose, do not propose a fix, do not assign priority from severity, and do not treat unverified claims as facts: produce the brief first.

## Repro Confidence

Use exactly one verdict.

| Verdict | Use When | Next Action |
| --- | --- | --- |
| `CONFIRMED` | Deterministic reproduction has been observed by the team, or supplied evidence includes complete steps, environment/data identifiers, and visible actual result. | Hand to debugging or execution with the minimal repro. |
| `INTERMITTENT` | There is credible evidence of occurrence, but deterministic reproduction is absent or a current attempt failed. | Capture frequency, conditions, timestamps, evidence, and repeated attempts before diagnosis. |
| `NOT REPRODUCIBLE YET` | A documented repro attempt was run under recorded conditions and there is no credible successful observation. | Record failed conditions and choose the next variation or closure decision. |
| `PLAUSIBLE` | One credible report has enough specifics to attempt reproduction, but the behavior is not confirmed. | Run the named next repro attempt or ask for one missing condition. |
| `INSUFFICIENT` | Missing core facts prevent a meaningful repro attempt. | Ask only reproduction-blocking questions. |

Guard clauses:

- Never emit `CONFIRMED` from a single unverified report, no matter how detailed.
- Never emit `NOT REPRODUCIBLE YET` if there is credible evidence of any observation; that is `INTERMITTENT` unless deterministic reproduction exists.

When third-party evidence qualifies as `CONFIRMED`, label it `confirmed by supplied evidence`, not `confirmed locally`. If a recording lacks steps or conditions, use `INTERMITTENT` or `PLAUSIBLE`.

## Facts, Assumptions, Unknowns

- **Fact:** Source-backed statement. Name the source: report quote, screenshot, log excerpt, command output, recording, reproduction by a named person, or similar.
- **Assumption:** Inference not directly stated or verified. Include the rationale.
- **Unknown:** Missing information that blocks or improves the next reproduction attempt.

Reporter testimony is a fact about the report, not verified product behavior. Use `reported, not verified` when a reporter claim is used as evidence.

Prior session context is not a fact unless it appears in the source material or is re-verified during this task. Otherwise mark it as an assumption or exclude it.

## Severity vs Priority

Severity is the observed or reported impact if the bug is real. Derive it from facts and label confidence.

Priority is scheduling or business urgency. Assign priority only when explicit owner, deadline, policy, customer commitment, or other scheduling input is present. Otherwise write `Priority: not assigned from repro brief`.

Never assign priority from severity alone.

## INTERMITTENT Discipline

When the verdict is `INTERMITTENT`, include all four fields:

- Observations and evidence: list sources and tag each as verified or reported.
- Constant vs varied conditions: what stayed the same, what changed, and what remains unknown.
- Next capture strategy: logs, request IDs, timestamps, seed/data shape, account/device/browser, repetition window, or instrumentation.
- Non-diagnosis boundary: `No root cause claimed from intermittent evidence; hypotheses require debugging after capture.`

Omit this section for non-`INTERMITTENT` verdicts.

## Output Format

```markdown
## Bug Repro Brief

Repro confidence: CONFIRMED | INTERMITTENT | NOT REPRODUCIBLE YET | PLAUSIBLE | INSUFFICIENT

Source material:
- <named report, screenshot, recording, log excerpt, command output, or other actual source>

Facts:
- <source-backed statement; use "reported, not verified" for reporter testimony>

Assumptions:
- <inference plus rationale>

Unknowns:
- <missing information that blocks or improves reproduction>

Expected behavior:
- <source-backed expectation, or labeled assumption that needs confirmation>

Actual behavior:
- <observed or reported behavior; tag reported vs verified>

Environment and data conditions:
- <browser, device, account role, data shape, environment, timing>

Frequency:
- <observed rate or "unknown"; for deterministic CONFIRMED, state 100% under listed conditions>

Severity: <impact derived from facts>
Priority: <owner/policy-assigned priority, or "not assigned from repro brief">

Repro steps:
1. <known step, or "not known yet">

Repro attempts run:
- <attempt, conditions, result; required for NOT REPRODUCIBLE YET and useful for INTERMITTENT>

Next repro attempt:
- <one concrete next action with conditions to vary or evidence to capture>

INTERMITTENT discipline:
- Include this subsection only when the verdict is INTERMITTENT.
- Observations and evidence: ...
- Constant vs varied conditions: ...
- Next capture strategy: ...
- Non-diagnosis boundary: no root cause claimed from intermittent evidence.

Evidence to attach or capture:
- <named items: log query, screenshot, request ID, command output, data sample>

Clarifying questions (repro-blocking only):
- <questions that unblock the next repro attempt; use "None" when confirmed>

Next action:
- <reproduce, ask reporter, capture evidence, or hand to debugging>
```

Omit optional sections only when omission cannot hide risk. Required risk gates should say `None`, `not applicable`, or `not assigned from repro brief` instead of disappearing. Do not print an `INTERMITTENT discipline` heading unless the verdict is `INTERMITTENT`.

## Good And Bad Examples

### Facts and Assumptions

Good:

```markdown
Fact: Reporter wrote "Save button does nothing after editing billing address" in the support note; reported, not verified.
Assumption: The failure may involve the billing address form because the reporter named that screen.
```

Bad:

```markdown
Fact: The Save button is broken.
```

Bad because it converts a reported claim into verified behavior.

### Repro Steps

Good:

```markdown
Repro steps:
1. Sign in as an account admin.
2. Open Settings > Billing Address.
3. Edit the street field and press Save.

Repro confidence: PLAUSIBLE
Next repro attempt: Run these steps in the reporter's account role and capture console output plus request ID.
```

Bad:

```markdown
Repro confidence: CONFIRMED
Repro steps:
1. Edit any address.
2. Save.
3. Observe the save failure.
```

Bad because it invents broader steps and marks the bug confirmed without observed deterministic reproduction.

### Evidence To Capture

Good:

```markdown
Evidence to attach or capture:
- Screenshot of the failed save state.
- Request ID for the save request.
- Timestamp and account role used in the repro attempt.
- Log excerpt matching that request ID.
```

Bad:

```markdown
Evidence: Logs show errors.
```

Bad because it launders vague evidence as a fact without a log excerpt, timestamp, query, command, or request ID.

### Severity and Priority

Good:

```markdown
Severity: High if confirmed, because the report says billing address changes cannot be saved; reported, not verified.
Priority: not assigned from repro brief.
```

Bad:

```markdown
Priority: High because users are furious.
```

Bad because it assigns scheduling priority from emotional impact language without owner or policy input.

## Persistence Guidance

If the brief should be stored, use the user's existing project or work-management location. Do not prescribe a tracker, folder, file format, or workflow.

If the user is using a work item, place the brief in the work item or link it from the work item. If the user is only debugging locally, keep the brief in the current response unless asked to persist it.

## Anti-Patterns

- Invented repro steps: filling plausible steps not present in evidence and calling them real.
- Diagnosis leak: sliding into "this is probably caused by..." before reproduction evidence.
- Severity inflation: promoting emotional language to high impact without observed effect.
- Priority creep: assigning schedule priority from severity alone.
- Single-source `CONFIRMED`: treating one unverified report as deterministic observation.
- Intermittent dismissal: closing or downgrading an intermittent case after one failed attempt.
- Verdict shopping: choosing `PLAUSIBLE` to unlock fix proposals without doing repro work.
- Chat-history contamination: treating prior session context as source material without re-verification.
- Evidence laundering: treating vague claims like "logs show errors" as facts without source details.

## Compact Worked Examples

### CONFIRMED

Repro confidence: `CONFIRMED`

Source material:
- Screen recording from tester showing complete steps, account role, environment, and visible failed result.

Facts:
- The recording shows the tester editing the billing address and pressing Save; confirmed by supplied evidence.
- The success message does not appear; confirmed by supplied evidence.

Severity: High if billing address updates are blocked for affected users.
Priority: not assigned from repro brief.
Clarifying questions: None.
Next action: Hand to debugging with the recording and listed steps.

### INTERMITTENT

Repro confidence: `INTERMITTENT`

Source material:
- Two support notes with timestamps.
- One screenshot of an error state.

Facts:
- Two users reported the same failed export message; reported, not verified.
- Screenshot shows the message `Export failed`; supplied evidence, not local reproduction.

INTERMITTENT discipline:
- Observations and evidence: two reported observations and one screenshot.
- Constant vs varied conditions: same export screen; different accounts; browser and dataset size unknown.
- Next capture strategy: capture timestamp, account role, dataset size, request ID, and retry result.
- Non-diagnosis boundary: no root cause claimed from intermittent evidence.

Next action: Capture evidence on the next occurrence before debugging.

### PLAUSIBLE

Repro confidence: `PLAUSIBLE`

Source material:
- QA note: "Import fails on CSV with blank optional phone column."

Facts:
- QA reported an import failure with a CSV containing a blank optional phone column; reported, not verified.

Unknowns:
- Sample CSV, import environment, exact error text, and whether the failure repeats.

Next repro attempt:
- Request the sample CSV or create a minimal equivalent only after confirming column names, then run one import attempt and capture error output.

Next action: Attempt reproduction after obtaining the sample or exact schema.

### INSUFFICIENT

Repro confidence: `INSUFFICIENT`

Source material:
- Message: "The dashboard is wrong sometimes."

Facts:
- Reporter said the dashboard is wrong sometimes; reported, not verified.

Unknowns:
- Which dashboard, expected value, actual value, account, timestamp, filters, role, and frequency.

Clarifying questions (repro-blocking only):
- Which dashboard and metric looked wrong?
- What expected value and actual value did you see?
- What account, filters, role, and timestamp were involved?

Next action: Ask reporter for reproduction-blocking details.

## Tune When

Tune this skill when agents:

- Start diagnosing or proposing fixes inside the brief.
- Treat detailed reports as `CONFIRMED` without deterministic evidence.
- Merge severity and priority into one field.
- Omit source material or accept vague evidence as fact.
- State reporter testimony as verified behavior without `reported, not verified`.
- Ask broad curiosity questions instead of reproduction-blocking questions.
- Dismiss intermittent reports after one failed repro attempt.
- Import prior session context as fact without source material or re-verification.
