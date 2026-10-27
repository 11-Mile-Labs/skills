---
name: regression-risk-map
description: "Map observable regressions and the checks that would catch them for one specific change. Use when asking what could break before merge. Do not choose test layers, decide ship or no-ship, or find a root cause. Use test-strategy-slicer for proof layers and release-readiness-brief for a ship call."
license: MIT
---

# Regression Risk Map

Map what may regress from one specific change. The result is a risk map tied to that change, not a generic test checklist and not a ship decision.

## Boundaries

Do not find root cause, propose a fix, run checks by default, write an exploratory charter, decide claimability, or emit merge, ship, or no-ship. Hand proof-layer choice to `test-strategy-slicer` and a release call to `release-readiness-brief`. If no specific change is available, ask for one. A speculative map is a miss.

## Mapping rule

Name observable symptoms, not causes. Risk level informs proof depth. It does not decide whether to ship.

## Input confidence

- `HIGH`: concrete diff, changed files, or equivalent source.
- `MEDIUM`: a description that names surfaces, behavior, and workflows.
- `LOW`: a title, vague prose, or an unsupported claim.

`LOW` input cannot emit `LOW` per-surface risk. `MEDIUM` input can emit `LOW` only when the description bounds that surface.

Per-surface risk is `LOW` (isolated, no shared state, contract, permission, or migration), `MEDIUM` (user-visible or shared, bounded callers), `HIGH` (auth, payment, data loss, permissions, migration, broad contract, many consumers), or `UNKNOWN`. `UNKNOWN` must name the context that would resolve it.

Do not collapse surfaces into one overall merge decision.

## Adjacency

Consider callers, consumers, shared state, caches, permissions, UI workflows, data migrations, background jobs, integrations, and config.

Use the full table unless input confidence is `HIGH` and the change is small and isolated. Compact mode still lists every at-risk or unknown category, plus the categories considered and not indicated.

The Risk cell is exactly `LOW`, `MEDIUM`, `HIGH`, `UNKNOWN`, or `not applicable`. Put caveats in Why or Targeted check.

| Status | Allowed risk |
| --- | --- |
| `at risk` | `LOW`, `MEDIUM`, `HIGH`, or `UNKNOWN` |
| `unknown` | `UNKNOWN` |
| `not indicated` | `not applicable` |

`not applicable` requires the source to rule the category out. If the source does not, status is `unknown` and risk is `UNKNOWN`.

Hypotheses name symptoms. A changed boundary may be context. Do not guess an internal cause.

Each targeted check names what to do, the variant, the pass condition, and which surface or hypothesis it covers. An activity such as "test auth" is not a check.

## Skipped checks

Include skipped checks only when the user is deciding whether to merge or ship, a relevant check is intentionally not proposed, or the user asks what risk remains. Four fields: skipped check, reason skipped now, risk if wrong, reopen trigger or owner acceptance. Do not use this table to hide `UNKNOWN` risk.

## Output

```markdown
## Regression Risk Map

Change understanding: HIGH | MEDIUM | LOW
Source: ...

Change summary:
- ...

Adjacency table:
| Surface | Status | Why | Risk | Targeted check |
| --- | --- | --- | --- | --- |
| Callers | at risk / not indicated / unknown | ... | LOW / MEDIUM / HIGH / UNKNOWN / not applicable | ... |

Regression hypotheses:
- Symptom: ...; surface: ...; risk: ...

User workflows at risk:
- ...

Data/state variants at risk:
- ...

Targeted checks:
- ...

Skipped checks:
| Skipped check | Reason skipped now | Risk if wrong | Reopen trigger / owner acceptance |
| --- | --- | --- | --- |

Residual risk (forecast):
- ...

Residual risk (after checks):
- ...

Next action:
- ...
```

Use forecast before checks run, and after-checks only when results exist. Omit skipped checks when no merge or ship decision and no intentional skip exists. Do not prescribe where to store the map.

## When the draft misses the contract

Read [references/examples.md](references/examples.md) and revise once. Read it only when a risk cell is malformed, a symptom names a cause, or a check has no pass condition. A draft that already matches this contract does not need the file.
