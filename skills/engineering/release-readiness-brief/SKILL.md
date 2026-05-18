---
name: release-readiness-brief
description: "Use when deciding whether a feature, fix, change set, migration, configuration change, or release candidate is ready to release, should be held, needs evidence, or needs a rollback/watch plan."
argument-hint: "[optional: change summary, release candidate, deployment note, migration plan, or release decision]"
license: MIT
---

# Release Readiness Brief

A release readiness brief turns a release decision into a concrete call: ship, ship with watch, gather evidence, decide an open question, or hold.

Use it when the work may leave the local workspace and affect users, operators, data, integrations, or downstream workflows.

## When To Use

Use this skill when asked whether a change is ready to release, ready to ship, ready to deploy, ready to cut over, safe to roll out, ready for a migration, or suitable for a hotfix.

Use it before release notes, deployment approval, production change, staged rollout, migration execution, or announcing that a change is release-ready.

Do not use this skill to:

- Review code quality in depth.
- Build a full project plan.
- Replace the verification contract for the work.
- Create tracker-specific release process.
- Approve a release when the user only asked to summarize changes.

If there is no concrete change or release candidate, ask for the change scope before writing a readiness brief.

## The Release Rule

Do not mark a release `READY` unless the intended outcome, included scope, proof evidence, rollback or fallback path, owner, and watch decision are all explicit.

If any of those are missing, choose the highest-precedence non-ready verdict and name the missing item.

## Verdicts And Precedence

Use exactly one verdict, applying this order:

1. **HOLD:** releasing now creates unacceptable risk, a known blocker exists, rollback is absent for a risky change, or the change is not the intended release candidate.
2. **NEEDS DECISION:** a product, operational, timing, ownership, rollout, compatibility, or customer-impact decision is missing.
3. **NEEDS EVIDENCE:** the release might be reasonable, but proof is missing or too vague.
4. **READY WITH WATCH:** release is acceptable only with named monitoring, owner, duration, and rollback or mitigation trigger.
5. **READY:** release can proceed with named proof and no required watch beyond normal operation.

Do not skip to `READY WITH WATCH` when a blocker or required decision is still open.

Use `READY WITH WATCH` for reversible changes with bounded residual risk, not as a way to hide missing evidence.

## Readiness Gates

Check these gates in order:

| Gate | Ready Means |
| --- | --- |
| Intent | The release outcome and affected users or systems are stated. |
| Scope | Included and excluded changes are named, including migrations, config, generated assets, docs, and flags when relevant. |
| Evidence | Each important behavior or risk has a source, result, and pass condition. |
| Compatibility | Known compatibility, data, permission, privacy, performance, and dependency concerns are addressed or explicitly absent. |
| Rollback or fallback | A concrete restore, revert, disable, mitigation, or customer-safe fallback exists, or the change is low-risk enough to state why none is needed. |
| Watch plan | The post-release signal, owner, duration, and trigger are named when residual risk remains. |
| Communication | Required internal or user-facing notice is named, or explicitly not needed. |
| Owner | One person or role owns the release call and follow-up. |

Unknown is not ready. If a gate is unknown, classify it as missing evidence, missing decision, or hold-level risk.

## Output Format

```markdown
## Release Readiness Brief

Verdict: HOLD | NEEDS DECISION | NEEDS EVIDENCE | READY WITH WATCH | READY

Release intent:
- ...

Included scope:
- ...

Excluded scope:
- ...

Evidence ledger:
| Claim or risk | Evidence source | Pass condition | Status |
| --- | --- | --- | --- |
| ... | ... | ... | proven / missing / not applicable |

Open decisions:
- ...

Risk and mitigation:
| Risk | Severity | Mitigation | Release impact |
| --- | --- | --- | --- |
| ... | low / medium / high | ... | proceed / watch / hold |

Rollback or fallback:
- Path:
- Owner:
- Trigger:
- Time sensitivity:

Watch plan:
- Signal:
- Owner:
- Duration:
- Trigger:

Communication:
- ...

Release call:
- ...

Next action:
- Release | Release with watch | Gather evidence | Make decision | Hold
```

Omit empty optional rows only when the omission cannot hide risk. For tiny reversible changes, keep the brief compact but preserve verdict, evidence, fallback, and next action.

## Evidence Standard

Evidence must name what was checked, where the result came from, and what pass means.

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

## Rollback And Fallback

A rollback or fallback entry must answer four questions:

1. What action reverses, disables, restores, or mitigates the release?
2. Who owns that action?
3. What trigger causes that action?
4. How time-sensitive is the action?

If rollback is impossible or expensive, say so plainly and adjust the verdict. A high-risk irreversible change without an accepted mitigation is `HOLD`.

Good:

- Path: disable the new import path with the existing runtime setting; Owner: release owner; Trigger: import error rate above 2% for 10 minutes; Time sensitivity: within 15 minutes.
- Path: restore from the pre-migration backup; Owner: database operator; Trigger: sampled ownership records mismatch; Time sensitivity: before any dependent write job resumes.

Bad:

- "Rollback if needed." Bad because it does not name the action, owner, trigger, or time sensitivity.
- "Revert the code." Bad because it may not reverse data, configuration, or downstream effects.
- "Monitor and fix forward." Bad because it is not a fallback unless the fix-forward action and trigger are named.

## Watch Plan

Use a watch plan when the release is acceptable but residual risk remains.

A watch plan must include signal, owner, duration, and trigger. If any field is missing, the release is `NEEDS EVIDENCE` or `NEEDS DECISION`, not `READY WITH WATCH`.

Good:

- Signal: failed checkout count and payment error rate; Owner: release owner; Duration: first 45 minutes; Trigger: error rate exceeds baseline by 1 percentage point.
- Signal: support inbox tags for import failure; Owner: support lead; Duration: next business day; Trigger: two related reports or one data-loss report.

Bad:

- "Keep an eye on it." Bad because it does not name the signal, owner, duration, or trigger.
- "Watch logs." Bad because logs are a source, not a decision rule.
- "Product will tell us if anything breaks." Bad because it waits for vague escalation instead of defining a signal and trigger.

## Decisions And Communication

Some release gaps are decisions, not evidence gaps. Treat a missing decision as `NEEDS DECISION` when evidence exists but the release call depends on preference, timing, ownership, customer impact, support readiness, or risk appetite.

Good:

- "Decision needed: release to all users or start with internal accounts; owner: product owner; needed before release because support copy differs."
- "Communication not needed: copy-only admin label change, no user workflow or support implication."

Bad:

- "We can decide later." Bad because the release path depends on the decision now.
- "No comms." Bad because it does not say why notice is unnecessary.
- "Ask stakeholders." Bad because it does not name the decision, owner, or release impact.

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

## Tune When

Tune this skill when outputs:

- Mark `READY` without proof evidence.
- Use `READY WITH WATCH` when decisions or evidence are missing.
- Treat "revert the code" as enough for data or configuration changes.
- Omit owner, trigger, or time sensitivity from rollback.
- Omit signal, owner, duration, or trigger from watch plans.
- Produce a full project plan instead of a release decision.
- Include bad examples without an explicit `Bad because ...` reason.
