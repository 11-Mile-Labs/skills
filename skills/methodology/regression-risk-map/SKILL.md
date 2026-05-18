---
name: regression-risk-map
description: "Use when reviewing a specific code or product change for what could regress, blast radius, adjacent workflows, targeted checks, or evidence needed before merge or ship."
argument-hint: "[change description, diff, file list, or review context]"
license: MIT
---

# Regression Risk Map

## Overview

Use this skill to map what may regress from a specific change before choosing proof, accepting skipped checks, or deciding whether more context is needed.

The goal is a focused risk map tied to the change, not a generic test checklist.

## When To Use

Use this when a specific change exists or is proposed and the user asks what could regress, what blast radius exists, what adjacent workflows matter, what to check before merge or ship, or whether current evidence covers likely regressions.

Primary entry points:

- Engineer: "I changed this; what should I verify before merge?"
- QA reviewer: "This change is ready for test; what could regress?"
- Work owner: "Does this change have enough regression coverage to accept the remaining risk?"

Do not use this to:

- Find root cause.
- Propose or implement a fix.
- Run checks by default.
- Produce broad exploratory test charters.
- Decide claimability of a work item.
- Emit a ship/no-ship verdict.

Hand off to a proof-contract skill when the user needs a formal done-proof plan. Hand off to debugging when the user needs cause. Hand off to work-item readiness review when the question is whether the work is claimable.

If no specific change is available, ask for one or state that the map would be speculative.

## The Mapping Rule

Map what may regress before choosing proof; name observable symptoms, not causes; risk level informs proof depth, not ship decision.

## Input Confidence and Risk Levels

First label change understanding:

- `HIGH`: based on a concrete diff, changed files, or equivalent source-level context.
- `MEDIUM`: based on a detailed change description that names surfaces, behavior, and affected workflows.
- `LOW`: based on vague prose, a title, or an unsupported claim.

Input confidence caps low-risk claims:

- `LOW` input confidence cannot emit `LOW` per-surface risk.
- `MEDIUM` input confidence can emit `LOW` only when the description concretely bounds that surface.
- `HIGH` input confidence can emit `LOW`, `MEDIUM`, `HIGH`, or `UNKNOWN` normally.

Risk labels apply per surface, workflow, or hypothesis, not to the whole change:

| Risk | Use When |
| --- | --- |
| `LOW` | Isolated surface, no shared state, contract, permission, or migration effect; callers known and bounded; straightforward check. |
| `MEDIUM` | User-visible or shared behavior with bounded callers, bounded variants, and clear targeted checks. |
| `HIGH` | Authentication, payment, data loss, permissions, migration, broad contract, shared state, many consumers, or high-impact workflow. |
| `UNKNOWN` | Blast radius cannot be bounded from available context. Must name exact context needed to resolve uncertainty. |

`UNKNOWN` without a context-needed phrase is invalid.

Do not collapse per-surface risks into one overall merge decision. If the user asks for a summary, call it a residual risk summary.

Never write `merge`, `do not merge`, `ship`, or `do not ship` as the skill's decision. Say which risk claim is unsupported, which context is missing, and what owner acceptance would be required.

## Adjacency Enumeration

Always consider these categories:

- Callers
- Consumers
- Shared state
- Caches
- Permissions
- UI workflows
- Data migrations
- Background jobs
- Integrations
- Config

Use a full table by default. Compact mode is allowed only when input confidence is `HIGH` and the change is clearly small and isolated.

Compact mode must include all at-risk and unknown categories plus:

```markdown
Other adjacency categories considered, not indicated: <list>
```

For `MEDIUM` or `LOW` input confidence, use the full table because silent skips are likely.

Each row needs a reason. `UNKNOWN` rows must name the missing context in the reason, such as `UNKNOWN: missing caller list`.

Risk column values are exactly one of `LOW`, `MEDIUM`, `HIGH`, `UNKNOWN`, or `not applicable`. Do not add explanatory text, colons, caveats, or backticked phrases in the Risk column. Put missing context, caveats, and confirmation needs in the `Why` or `Targeted check` columns.

For `not indicated` rows, use `not applicable` only when the source explicitly rules the category out. If the source does not rule it out, use `unknown` status and `UNKNOWN` risk.

If Risk is `UNKNOWN`, Status must be `unknown`. Do not pair `not indicated` with `UNKNOWN`.

Status-risk pairings:

| Status | Allowed Risk |
| --- | --- |
| `at risk` | `LOW`, `MEDIUM`, `HIGH`, or `UNKNOWN` |
| `unknown` | `UNKNOWN` |
| `not indicated` | `not applicable` |

Invalid risk cells:

- `UNKNOWN: caller list not provided`
- `LOW (bounded by description)`
- `confirm no job path`

Valid row:

```markdown
| Callers | unknown | UNKNOWN: caller list not provided | UNKNOWN | Identify callers before bounding risk |
```

## Regression Hypotheses

Regression hypotheses name observable failure symptoms, not root causes.

Good:

```markdown
Symptom: Existing users cannot sign in after the shared form change.
```

Bad:

```markdown
Cause: Token refresh state probably broke.
```

Bad because it guesses root cause instead of naming an observable regression.

It is fine to mention the changed boundary as context: "Because the shared form component changed, signup, login, and reset submit flows may fail." Do not infer internal cause without evidence.

## Targeted Checks

Each targeted check must name:

- What to run or do.
- Data, state, role, or workflow variant.
- Pass condition.
- Which surface, workflow, or hypothesis it covers.

Good:

```markdown
Run password reset with an expired token; pass if the recovery path appears and a subsequent login succeeds. Covers reset flow and auth continuity.
```

Bad:

```markdown
Test auth.
```

Bad because it does not name workflow, variant, pass condition, or covered risk.

Activity is not proof. A check is only useful if another agent can repeat it and know whether it passed.

## Skipped Checks

Include skipped checks only when:

- The user is deciding whether to merge or ship.
- A relevant check is intentionally not proposed or not run.
- The user asks what risk remains.

Use four fields:

| Skipped check | Reason skipped now | Risk if wrong | Reopen trigger / owner acceptance |
| --- | --- | --- | --- |

Do not use skipped checks to launder `UNKNOWN` risk. If risk is `UNKNOWN` because context is missing, name the missing context or the explicit owner acceptance condition.

## Output Format

```markdown
## Regression Risk Map

Change understanding: HIGH | MEDIUM | LOW
Source: <diff, changed files, detailed description, or vague prose>

Change summary:
- <what changed; cite source when possible>

Adjacency table:
| Surface | Status | Why | Risk | Targeted check |
| --- | --- | --- | --- | --- |
| Callers | at risk / not indicated / unknown | ... | LOW/MEDIUM/HIGH/UNKNOWN/not applicable | ... |
| Consumers | ... | ... | ... | ... |
| Shared state | ... | ... | ... | ... |
| Caches | ... | ... | ... | ... |
| Permissions | ... | ... | ... | ... |
| UI workflows | ... | ... | ... | ... |
| Data migrations | ... | ... | ... | ... |
| Background jobs | ... | ... | ... | ... |
| Integrations | ... | ... | ... | ... |
| Config | ... | ... | ... | ... |

Regression hypotheses:
- Symptom: <observable failure>; surface: <surface>; risk: <level>

User workflows at risk:
- <workflow; risk level>

Data/state variants at risk:
- <variant; risk level>

Targeted checks:
- <what to run/do; variant; pass condition; covers: <hypothesis/workflow>>

Skipped checks:
| Skipped check | Reason skipped now | Risk if wrong | Reopen trigger / owner acceptance |
| --- | --- | --- | --- |

Residual risk (forecast):
- <risk remaining if proposed checks pass>

Residual risk (after checks):
- <risk remaining after actual evidence>

Next action:
- <run checks, collect missing context, or get owner acceptance>
```

Use `Residual risk (forecast)` before checks run. Use `Residual risk (after checks)` when actual check results are available.

Omit `Skipped checks` when no merge/ship decision or intentional skip exists. Omit whichever residual-risk mode does not apply.

If the map should be persisted, use the user's existing durable location and link or reference the map from the relevant change record. Do not prescribe a tracker, folder, or file format.

## Good And Bad Examples

### Adjacency Entries

Good:

```markdown
| Permissions | at risk | Shared submit component now gates disabled state for admin and member forms | MEDIUM | Submit as admin and member; pass if both allowed roles can save and disallowed role remains blocked |
```

Bad:

```markdown
| Permissions | not indicated | Seems unrelated | LOW | None |
```

Bad because it dismisses a permission-adjacent surface without a source-backed reason or targeted check.

### Hypotheses

Good:

```markdown
Symptom: Existing users cannot submit login after the shared form change; surface: UI workflows; risk: MEDIUM.
```

Bad:

```markdown
Symptom: Login token reducer probably drops pending refresh.
```

Bad because it names an unverified cause instead of an observable symptom.

### Targeted Checks

Good:

```markdown
Run signup, login, and password reset submit flows with valid inputs; pass if each reaches the expected success state and no validation message is lost.
```

Bad:

```markdown
Run auth tests.
```

Bad because it is activity, not a repeatable check with workflow, variant, pass condition, and covered risk.

### Skipped Checks

Good:

```markdown
| Skipped check | Reason skipped now | Risk if wrong | Reopen trigger / owner acceptance |
| Cross-role member save check | No member test account available today | Member users may be blocked from saving | Owner accepts ship only for admin-only release, or create member account before broad release |
```

Bad:

```markdown
| Skipped check | Reason skipped now | Risk if wrong | Reopen trigger / owner acceptance |
| Contract consumer check | Too slow | Unknown | Accepted |
```

Bad because it hides an `UNKNOWN` consumer risk without naming missing context or real owner acceptance.

## Anti-Patterns

- Silent category skip: omitting an adjacency category with no reason or considered-categories line.
- Invented adjacency: fabricating callers or consumers not visible from source material.
- Confidence laundering: promoting `LOW` input confidence to `LOW` per-surface risk.
- `UNKNOWN` escape hatch: using `UNKNOWN` without context needed to resolve it.
- Hypothesis-as-diagnosis: naming root cause instead of observable symptom.
- Check laundering: listing activity instead of repeatable check, variant, and pass condition.
- Ship/no-ship verdict creep: turning risk levels into a merge decision.
- Over-testing: marking every surface `HIGH` and requiring broad checks for a tiny isolated change.
- Skipped-check laundering: using skipped checks to hide missing context.
- Risk inflation or deflation: choosing levels to look thorough or skip work instead of applying definitions.

## Compact Worked Examples

### LOW Isolated Helper

Change understanding: `HIGH`

Change summary:
- Internal string formatter changed; helper is not exported and callers are known.

Adjacency table:
| Surface | Status | Why | Risk | Targeted check |
| --- | --- | --- | --- | --- |
| Callers | at risk | Two known internal callers format display text | LOW | Run formatter cases for both callers; pass if output matches expected strings |

Other adjacency categories considered, not indicated: consumers, shared state, caches, permissions, UI workflows, migrations, background jobs, integrations, config.

Residual risk (forecast):
- Low if formatter cases pass; risk remains limited to display text from known callers.

### MEDIUM UI-Adjacent Change

Change understanding: `MEDIUM`

Change summary:
- Shared form button behavior changed for account forms.

Adjacency table:
| Surface | Status | Why | Risk | Targeted check |
| --- | --- | --- | --- | --- |
| UI workflows | at risk | Signup, login, reset, and profile save may share the button | MEDIUM | Run each submit flow; pass if valid submit reaches success and invalid submit shows validation |
| Permissions | unknown | Role-specific forms not named in description | UNKNOWN | Identify role-gated forms or confirm none |
| Config | unknown | No config behavior described; source does not rule it out | UNKNOWN | Confirm changed files before bounding config impact |

Targeted checks:
- Run signup, login, reset, and profile save with valid and invalid input; pass if success and validation states remain correct.

### HIGH Shared Schema

Change understanding: `HIGH`

Change summary:
- Shared customer status field changed from free text to enum.

Adjacency table:
| Surface | Status | Why | Risk | Targeted check |
| --- | --- | --- | --- | --- |
| Consumers | at risk | Multiple readers may expect previous values | HIGH | Query current status values and run consumer read paths; pass if old and new records render correctly |
| Data migrations | at risk | Existing rows need valid enum values | HIGH | Run migration on representative sample; pass if all rows map or are reported |
| Background jobs | unknown | Job consumers not listed in source | UNKNOWN | Identify scheduled/status-processing jobs before release |

Skipped checks:
| Skipped check | Reason skipped now | Risk if wrong | Reopen trigger / owner acceptance |
| Background job status consumer check | Job inventory unavailable | Status jobs may fail on enum values | Owner accepts only if release excludes job path, or job inventory is completed |

## Tune When

Tune this skill when agents:

- Produce generic QA checklists instead of change-tied risk maps.
- Skip adjacency categories silently.
- Invent callers, consumers, or workflows not supported by source.
- Treat weak input as enough to claim `LOW` risk.
- Use `UNKNOWN` without naming context needed.
- Turn hypotheses into root-cause guesses.
- Turn risk levels into ship/no-ship decisions.
- Over-test tiny isolated changes.
- Use skipped checks to hide missing context.
