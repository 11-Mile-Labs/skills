# Examples

Read this file only when a draft misses the output contract: a required section is missing, a verdict is unsupported, or a field was filled in without a source.

Do not read it when the draft already matches the contract.

## Anti-Patterns

- **Acceptance criteria without proof:** describes desired behavior but not how completion will be proven.
- **Activity as evidence:** says tests, QA, or review happened without naming the result.
- **Gap dumping:** moves hard-to-prove behavior into accepted gaps without the four required fields.
- **Invented acceptance:** puts a gap in the accepted table when the person or role has not actually accepted it.
- **Tiny-change ritual:** turns an isolated reversible change into a release ceremony.
- **False completion:** claims done while unresolved verification gaps remain.
- **Test-plan drift:** renames the contract into a generic test plan and loses the completion claim rule.

## Compact Worked Examples

### Tiny

Change: copy update on a settings label.

| Behavior or risk | Method | Evidence to collect | Pass condition |
| --- | --- | --- | --- |
| Label text changed in the settings view | Inspect changed view | Screenshot or local preview note | New copy appears in the intended location |
| No unrelated settings copy changed | Diff review | File diff or review note | Only targeted label text changed |
| Mobile wrapping remains acceptable | Narrow preview | Screenshot or manual note | Label does not overlap adjacent controls |

Accepted gaps: none. Completion may be claimed when the three rows have evidence.

### Standard

Change: add an account email preference.

| Behavior or risk | Method | Evidence to collect | Pass condition |
| --- | --- | --- | --- |
| User can enable the preference | Automated or manual flow | Test output or recorded manual steps | Preference persists after save and reload |
| User can disable the preference | Automated or manual flow | Test output or recorded manual steps | Preference persists as disabled after save and reload |
| Existing preferences are unchanged | Regression check | Test output or diff-backed review | Existing preference values continue to load and save |
| Validation handles missing account | Negative-path check | Test output or manual note | Missing account returns a clear failure without saving |

Unresolved verification gaps must be listed before any completion claim.

### High-Risk

Change: migrate account ownership records.

| Behavior or risk | Method | Evidence to collect | Pass condition |
| --- | --- | --- | --- |
| Existing owners are preserved | Dry run or migration check | Before/after counts and sampled records | Every sampled account keeps the expected owner |
| Invalid owner references are handled | Negative-path check | Report of invalid references | Invalid references are reported and not silently reassigned |
| Rollback is possible | Rollback rehearsal or documented restore path | Rollback command, backup reference, or restore note | Previous ownership state can be restored within the agreed window |
| Production health remains stable | Monitoring query | Error and latency checks for agreed window | No ownership-related error spike after release |

Completion may be claimed only after migration evidence, rollback evidence, and monitoring evidence are named, or any missing item is recorded as an accepted gap with all four fields.

## Contrasts: Evidence Bar

Good:

- `project test auth-reset` exits 0 and includes expired-token, reused-token, and unknown-email cases.
- Screenshot saved to `/tmp/settings-mobile-save.png` shows the save button remains visible at 390px width.
- Query `route=/checkout status>=500` stays at 0 matching errors for 30 minutes after release.
- Manual check: user without `billing:write` cannot view or submit the billing form; observed forbidden response in the network panel.

Bad:

- "Tested it." Bad because it names an activity but no source, scope, or pass condition.
- "Looks good." Bad because it is not re-checkable and does not say what was inspected.
- "Manual QA." Bad because it hides the flow, environment, and expected result.
- "Should be covered by existing tests." Bad because it asserts coverage without naming the test or expected behavior.

## Contrasts: Accepted Gaps

Good:

- Gap: no tablet visual check. Acceptor: product owner. Reason: layout change only targets desktop nav. Risk: tablet nav may wrap awkwardly. Reopen trigger: any tablet complaint or future nav layout change.

Bad:

- "Skipping Safari because low risk." Bad because it has no acceptor, no concrete risk, and no reopen trigger.
- "We accept missing load testing." Bad because "we" is not a specific acceptor and the risk is unstated.
- "Mobile can wait." Bad because it does not explain why waiting is acceptable or when the gap reopens.
- "Default preference value not yet determined." Bad because an undecided requirement is unresolved, not accepted.

## Contrasts: Pass Conditions

Good:

- Password reset link cannot be reused after one successful reset.
- Empty-state message appears when the list has zero items and disappears when one item is added.
- Error rate remains below the agreed threshold for the first 30 minutes after release.

Bad:

- "Run reset tests." Bad because running a command is an activity, not a passing result.
- "Check empty state." Bad because it does not say what the user should see.
- "Monitor errors." Bad because it has no threshold, duration, or decision rule.
