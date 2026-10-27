# Examples

Read this file only when a draft misses the output contract: a required section is missing, a verdict is unsupported, or a field was filled in without a source.

Do not read it when the draft already matches the contract.

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
