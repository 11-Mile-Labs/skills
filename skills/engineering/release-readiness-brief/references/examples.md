# Examples

Read this file only when a draft misses the output contract: a required section is missing, a verdict is unsupported, or a field was filled in without a source.

Do not read it when the draft already matches the contract.

## Good And Bad Readiness Calls

### Ready

Good:

```markdown
Verdict: READY
Release call:
- Release the copy-only settings label change. Evidence: visual preview confirms the new label and diff shows no behavior change. Rollback: revert the copy commit. Communication: not needed because there is no workflow or policy change.
```

Bad:

```markdown
Verdict: READY
Release call:
- Looks fine to ship.
```

Bad because it has no release intent, scope, evidence, fallback, owner, or communication decision.

### Ready With Watch

Good:

```markdown
Verdict: READY WITH WATCH
Watch plan:
- Signal: import error count and invalid-row reports.
- Owner: release owner.
- Duration: first 60 minutes after release.
- Trigger: more than three import failures or any suspected data corruption.
```

Bad:

```markdown
Verdict: READY WITH WATCH
Watch plan:
- Monitor after deploy.
```

Bad because watch without signal, owner, duration, and trigger cannot support a release decision.

### Hold

Good:

```markdown
Verdict: HOLD
Next action:
- Hold until rollback for the ownership migration is rehearsed or an authorized risk acceptor approves releasing without it.
```

Bad:

```markdown
Verdict: HOLD
Next action:
- This feels risky.
```

Bad because it does not identify the blocker or the condition that would make release possible.

## Anti-Patterns

- **Proof fog:** lists activities instead of evidence and pass conditions.
- **Ready by optimism:** marks ready because the change is wanted or urgent.
- **Watch as a broom:** uses monitoring language to sweep away missing proof.
- **Rollback theater:** says "revert" without considering data, configuration, jobs, or user-visible side effects.
- **Silent scope:** forgets generated files, migrations, configuration, docs, or flags that are part of the release.
- **Decision laundering:** hides unresolved product or operational choices as technical readiness.
- **Risk flattening:** treats all risks as low because no incident has happened yet.

## Compact Worked Examples

### Tiny Copy Change

Verdict: READY

Evidence ledger:
| Claim or risk | Evidence source | Pass condition | Status |
| --- | --- | --- | --- |
| Label text changed only in settings | Diff review | Only intended label changed | proven |
| Text fits in narrow layout | Local preview | No overlap or truncation at target width | proven |

Rollback or fallback: revert the copy change; owner is release owner; trigger is layout or meaning complaint; time sensitivity is low.

Next action: Release.

### Migration Candidate

Verdict: NEEDS EVIDENCE

Evidence ledger:
| Claim or risk | Evidence source | Pass condition | Status |
| --- | --- | --- | --- |
| Existing ownership data preserved | Dry run | Sampled before/after owners match | missing |
| Invalid references handled | Dry run report | Invalid references are reported, not rewritten silently | missing |
| Rollback available | Backup restore note | Pre-migration state can be restored before dependent writes resume | missing |

Next action: Gather evidence before release.

### Reversible Feature Release

Verdict: READY WITH WATCH

Risk and mitigation:
| Risk | Severity | Mitigation | Release impact |
| --- | --- | --- | --- |
| New import flow may reject valid rows | medium | Disable the new import path and restore previous flow | watch |

Watch plan: import error count; release owner; first 60 minutes; trigger is more than three import failures or one suspected data-loss report.

Next action: Release with watch.

## Contrasts: Evidence Standard

Good:

- `account settings save test` exits 0 and includes create, update, and permission-denied cases.
- Manual check in staging: user can enable the preference, reload, and see the saved value.
- Migration dry run reports 1,204 rows scanned, 0 invalid references, and no write actions.
- Error log query for `route=/import` stays at zero matching errors for 30 minutes after release.

Bad:

- "Tests pass." Bad because it does not name the test source, covered behavior, or pass condition.
- "QA looked at it." Bad because it hides what was inspected and what result was acceptable.
- "No errors seen." Bad because it lacks the signal source, threshold, and observation window.
- "Seems low risk." Bad because it is judgment without evidence or a stated release impact.

## Contrasts: Rollback And Fallback

Good:

- Path: disable the new import path with the existing runtime setting; Owner: release owner; Trigger: import error rate above 2% for 10 minutes; Time sensitivity: within 15 minutes.
- Path: restore from the pre-migration backup; Owner: database operator; Trigger: sampled ownership records mismatch; Time sensitivity: before any dependent write job resumes.

Bad:

- "Rollback if needed." Bad because it does not name the action, owner, trigger, or time sensitivity.
- "Revert the code." Bad because it may not reverse data, configuration, or downstream effects.
- "Monitor and fix forward." Bad because it is not a fallback unless the fix-forward action and trigger are named.

## Contrasts: Watch Plan

Good:

- Signal: failed checkout count and payment error rate; Owner: release owner; Duration: first 45 minutes; Trigger: error rate exceeds baseline by 1 percentage point.
- Signal: support inbox tags for import failure; Owner: support lead; Duration: next business day; Trigger: two related reports or one data-loss report.

Bad:

- "Keep an eye on it." Bad because it does not name the signal, owner, duration, or trigger.
- "Watch logs." Bad because logs are a source, not a decision rule.
- "Product will tell us if anything breaks." Bad because it waits for vague escalation instead of defining a signal and trigger.

## Contrasts: Decisions And Communication

Good:

- "Decision needed: release to all users or start with internal accounts; owner: product owner; needed before release because support copy differs."
- "Communication not needed: copy-only admin label change, no user workflow or support implication."

Bad:

- "We can decide later." Bad because the release path depends on the decision now.
- "No comms." Bad because it does not say why notice is unnecessary.
- "Ask stakeholders." Bad because it does not name the decision, owner, or release impact.
