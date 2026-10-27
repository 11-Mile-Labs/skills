# Examples

Read this file only when a draft misses the output contract: a required section is missing, a verdict is unsupported, or a field was filled in without a source.

Do not read it when the draft already matches the contract.

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

## Contrasts: Regression Hypotheses

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

## Contrasts: Targeted Checks

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
