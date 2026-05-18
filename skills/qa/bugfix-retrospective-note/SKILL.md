---
name: bugfix-retrospective-note
description: "Use when summarizing a completed or nearly completed bugfix, especially to capture cause, missed signal, proof, prevention, follow-up, or whether a defect should generate another work item."
argument-hint: "[optional: bug report, fix summary, failing test, incident note, or PR diff]"
license: MIT
---

# Bugfix Retrospective Note

A bugfix retrospective note captures what failed, why the fix is credible, and what should prevent recurrence. It is deliberately lighter than an incident postmortem.

Use it when a bugfix taught something worth preserving but does not deserve a heavy ceremony.

## When To Use

Use this skill after fixing a bug, before closing a bug ticket, when writing a bugfix PR summary, when deciding whether follow-up work is needed, or when a defect exposes a test, monitoring, documentation, or process gap.

Do not use this skill for a full incident review, blame assignment, or generic release notes.

If the fix or proof is not known yet, use `NEEDS FIX FACTS`.

## Retrospective Rule

Do not call a bugfix understood unless the symptom, cause, fix, proof, missed signal, prevention, and follow-up decision are all addressed.

Unknown cause is allowed, but it must be named as unknown with the next investigation step.

## Verdicts And Precedence

Use exactly one verdict:

1. **NEEDS FIX FACTS:** symptom, fix, proof, or affected surface is missing.
2. **CAUSE UNKNOWN:** the symptom is fixed or mitigated, but root cause is not known enough to prevent recurrence.
3. **FOLLOW-UP REQUIRED:** the fix works, but a prevention, cleanup, monitoring, documentation, data repair, or broader regression task remains.
4. **NOTE COMPLETE:** cause, fix, proof, missed signal, prevention, and follow-up decision are captured.

Do not use `NOTE COMPLETE` when proof is only "seems fixed" or prevention is absent.

## Required Fields

| Field | Meaning |
| --- | --- |
| Symptom | What failed from the user's, operator's, or system's point of view. |
| Impact | Who or what was affected, and how bad it was. |
| Cause | The mechanism that made the bug happen, or what is still unknown. |
| Fix | What changed to stop the bug. |
| Proof | Evidence that the fix addresses the symptom and does not obviously regress nearby behavior. |
| Missed signal | Why the defect was not caught earlier, or what signal was absent. |
| Prevention | Test, check, guardrail, design constraint, monitoring, or documentation that reduces recurrence. |
| Follow-up | Required, optional, or none, with owner/trigger when not immediate. |

## Output Format

```markdown
## Bugfix Retrospective Note

Verdict: NEEDS FIX FACTS | CAUSE UNKNOWN | FOLLOW-UP REQUIRED | NOTE COMPLETE

Bug:
- Symptom:
- Impact:
- Affected surface:

Cause:
- ...

Fix:
- ...

Proof:
- ...

Missed signal:
- ...

Prevention:
- ...

Follow-up:
| Item | Required? | Owner | Trigger or due point |
| --- | --- | --- | --- |
| ... | yes / no | ... | ... |

Closeout:
- ...
```

Keep the note short. The value is precision, not length.

## Good And Bad Examples

### Cause

Good:

- `Cause: empty import rows skipped validation because the parser filtered blank strings before row-level errors were attached.`

Bad:

- "Bug in parser." Bad because it names an area, not the mechanism.
- "Race condition maybe." Bad because uncertainty is not separated from known cause or next investigation.
- "User error." Bad because it blames the reporter without identifying the system behavior that allowed failure.

### Proof

Good:

- `Proof: blank, malformed, and valid row checks pass; manual import preview shows row-level error without dropping valid rows.`

Bad:

- "Fixed locally." Bad because it does not name evidence or pass condition.
- "Tests pass." Bad because it does not identify the test or bug behavior covered.
- "Looks good now." Bad because visual confidence is not reproducible proof.

### Missed Signal

Good:

- `Missed signal: existing parser tests covered malformed cells but not fully blank rows.`
- `Missed signal: support report described the symptom, but ticket lacked a minimal reproduction.`

Bad:

- "We should have caught it." Bad because it does not identify the missing signal.
- "No one noticed." Bad because it does not say what should have noticed.
- "Testing gap." Bad because it does not name the missing case.

### Follow-up

Good:

- `Required follow-up: backfill affected records; owner is data operator; trigger is before closing the customer report.`

Bad:

- "Maybe add monitoring later." Bad because it has no owner, trigger, or decision.
- "Follow up in v2." Bad because it does not say what work remains or why it can wait.
- "No follow-up." Bad because it is unsupported without prevention and proof context.

## Follow-Up Decisions

Use `FOLLOW-UP REQUIRED` when:

- Data repair is needed.
- A broader regression risk is discovered.
- The fix is a mitigation, not the underlying cause.
- Prevention requires a separate change.
- Documentation, support copy, or operator guidance must change.
- Monitoring or alerting should be added but is not part of the fix.

Use `NOTE COMPLETE` only when remaining work is either unnecessary or intentionally deferred with owner and trigger.

## Prevention Types

- **Regression proof:** a focused test or manual check for the exact missed case.
- **Input guard:** validation, constraint, or invariant that rejects bad state earlier.
- **Observability:** signal that makes recurrence visible.
- **Documentation:** clarified expected behavior or support steps.
- **Design constraint:** simplification that removes the bug class.
- **Ticket improvement:** reproduction requirements for similar reports.

## Anti-Patterns

- **Blame note:** focuses on who made the mistake instead of the failure mechanism.
- **Victory lap:** says fixed without cause or proof.
- **Vague prevention:** says "add tests" without naming the missed case.
- **Follow-up fog:** leaves "later" work without owner or trigger.
- **Root-cause theater:** invents a cause because unknown feels uncomfortable.
- **Postmortem inflation:** turns a small bugfix into a heavyweight incident process.

## Compact Worked Examples

### Note Complete

Verdict: NOTE COMPLETE

Bug:
- Symptom: blank import rows disappeared instead of showing row-level errors.
- Impact: users could not tell why row counts changed.
- Affected surface: import preview validation.

Cause:
- Parser filtered blank strings before attaching row errors.

Fix:
- Blank rows now produce row-level errors while valid rows continue through preview.

Proof:
- Blank, malformed, and valid row checks pass with expected accepted/rejected outcomes.

Missed signal:
- Existing parser checks covered malformed cells but not fully blank rows.

Prevention:
- Added blank-row regression proof to parser checks.

Follow-up:
| Item | Required? | Owner | Trigger or due point |
| --- | --- | --- | --- |
| None | no | change owner | no known affected saved data |

Closeout:
- Close bug after proof is attached to the review.

### Cause Unknown

Verdict: CAUSE UNKNOWN

Bug: intermittent save failure.

Fix: retry reduced frequency but did not identify why the original save fails.

Proof: retry behavior works in focused check.

Missed signal: no error classification exists for save failures.

Follow-up: investigate original failure class before claiming recurrence prevention.

### Follow-Up Required

Verdict: FOLLOW-UP REQUIRED

Cause: migration skipped records with missing owner IDs.

Fix: migration now reports skipped records.

Proof: dry run reports skipped records.

Follow-up:
| Item | Required? | Owner | Trigger or due point |
| --- | --- | --- | --- |
| Repair existing skipped records | yes | data owner | before customer closeout |

## Tune When

Tune this skill when outputs:

- Claim a bugfix is understood without cause or proof.
- Invent root cause.
- Say "add tests" without naming the missed case.
- Omit missed signal.
- Hide required follow-up.
- Turn small bugfixes into heavyweight incident reviews.
- Include bad examples without an explicit `Bad because ...` reason.
