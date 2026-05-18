---
name: verification-contract
description: "Use when defining done, acceptance criteria, proof of completion, ticket sign-off, PRD verification, or the evidence needed before claiming work is complete."
argument-hint: "[optional: ticket, plan, PRD, change description, or completion claim]"
license: MIT
---

# Verification Contract

A verification contract is the evidence standard for a future completion claim: what must be true, how you will know, and what gaps remain accepted.

Use it to stop completion language from drifting away from proof.

## When To Use

Use this skill in two modes:

- **Before work starts:** define the evidence required to prove a feature, bugfix, ticket, PRD, release slice, or plan is done.
- **Before claiming done:** audit an existing completion claim against the required evidence.

Do not use this skill when the user only asks you to run an already-known command or test suite. If a verification contract already exists for the same scope, reconcile or update it instead of creating a competing contract.

## The Completion Claim Rule

Do not claim completion unless every in-scope behavior has either re-checkable evidence with a pass condition, or an accepted gap meeting the four-field discipline.

If evidence is missing and the gap is not accepted, say that completion is not yet proven.

## Depth Triage

Choose the smallest depth that protects the completion claim:

1. Can this change cause data loss, permission or security failure, money or billing impact, irreversible migration or state change, or critical-path customer breakage? If yes, use **high-risk**.
2. Is rollback hard, slow, or unproven? If yes, use **high-risk**.
3. Does it affect shared behavior, integrations, background jobs, analytics, metrics, or user-visible workflows? If yes, use **standard** unless the previous questions made it high-risk.
4. Is it reversible in one step, isolated, free of shared state, and low user-visible risk? If yes, use **tiny**.

Default to **standard** when the answers are unclear, and list the ambiguity as a residual risk.

- **Tiny:** 3-5 verification rows, plus any unresolved or accepted gaps.
- **Standard:** full contract with behavior, evidence, pass conditions, residual risks, and completion rule.
- **High-risk:** standard contract plus rollback, monitoring, negative-path, and post-change evidence.

## Output Format

Produce the verification contract inline unless the surrounding workflow already has a durable home such as a ticket, PR description, plan, release checklist, or project doc.

```markdown
## Verification Contract

Depth: tiny | standard | high-risk

Outcome to prove:
- ...

In scope:
- ...

Out of scope:
- ...

Verification matrix:
| Behavior or risk | Method | Evidence to collect | Pass condition |
| --- | --- | --- | --- |
| ... | ... | ... | ... |

Manual exploration:
- ...

Accepted gaps:
| Gap | Acceptor | Reason accepted now | Risk if wrong | Reopen trigger |
| --- | --- | --- | --- | --- |
| ... | ... | ... | ... | ... |

Unresolved verification gaps:
- ...

Completion claim rule:
- Completion may be claimed only after ...
```

Omit empty optional sections only when doing so cannot hide risk. Keep tiny contracts compact.

## Evidence Bar

Evidence must be re-checkable by a third party from the entry alone, or explicitly labeled as non-recheckable judgment evidence.

A usable evidence entry names the source, states the pass condition, and is specific enough to rerun, inspect, or challenge.

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

## Accepted Gaps

An accepted gap is allowed only when all four fields are present:

1. **Acceptor:** named person if known, otherwise a specific role.
2. **Reason accepted now:** why it is acceptable not to prove this in the current pass.
3. **Risk if wrong:** what could happen if the gap hides a defect.
4. **Reopen trigger:** event, date, release phase, metric, user report, or scope change that makes the gap invalid.

If any field is missing, the gap is unresolved, not accepted.

Do not invent acceptance. If the user or source material has not accepted the gap, do not put it in the accepted gaps table. List it under unresolved verification gaps with the required acceptor role and ask for acceptance.

Unknown requirements, undecided product choices, and missing defaults are unresolved verification gaps. Do not record them as accepted gaps.

Good:

- Gap: no tablet visual check. Acceptor: product owner. Reason: layout change only targets desktop nav. Risk: tablet nav may wrap awkwardly. Reopen trigger: any tablet complaint or future nav layout change.

Bad:

- "Skipping Safari because low risk." Bad because it has no acceptor, no concrete risk, and no reopen trigger.
- "We accept missing load testing." Bad because "we" is not a specific acceptor and the risk is unstated.
- "Mobile can wait." Bad because it does not explain why waiting is acceptable or when the gap reopens.
- "Default preference value not yet determined." Bad because an undecided requirement is unresolved, not accepted.

## Pass Conditions

A pass condition is the observable threshold that makes evidence meaningful. It must describe the result, not just the check.

Good:

- Password reset link cannot be reused after one successful reset.
- Empty-state message appears when the list has zero items and disappears when one item is added.
- Error rate remains below the agreed threshold for the first 30 minutes after release.

Bad:

- "Run reset tests." Bad because running a command is an activity, not a passing result.
- "Check empty state." Bad because it does not say what the user should see.
- "Monitor errors." Bad because it has no threshold, duration, or decision rule.

## Persistence Guidance

If the workflow has a durable system, put the contract where future work will look first: ticket body or comment, PR description, plan, release checklist, or project documentation.

Do not prescribe a project path. If no durable system exists, keep the contract inline and make it copy-ready.

When updating an existing contract, preserve prior decisions unless they are stale. Call out what changed and why.

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

## Tune When

Tune this skill when outputs:

- Become generic checklists instead of evidence contracts.
- Skip pass conditions.
- Treat vague phrases like "tested it" as evidence.
- Put uncomfortable work into accepted gaps without the four required fields.
- Trigger for simple "run the tests" requests.
- Produce long ceremony for tiny reversible changes.
- Claim completion while unresolved verification gaps remain.
