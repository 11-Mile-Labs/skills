---
name: acceptance-criteria-tightener
description: "Rewrite acceptance criteria into observable pass/fail statements. Use when criteria are vague, subjective, or missing a boundary. Do not define the evidence log, decide whether a ticket is claimable, or make a release call. Use verification-contract for proof of done."
license: MIT
---

# Acceptance Criteria Tightener

Turn vague criteria into statements an independent reviewer can pass or fail without guessing the implementer's intent.

## Boundaries

Do not decide claimability, define proof of done (`verification-contract`), choose v1 versus v2 (`scope-check`), map regressions, write a test script, or implement the feature.

## Tightening rule

A criterion is tight when it has an actor or system, a condition, an action or state change, and an observable result.

## Levels and grammar

- `MUST`: required for this acceptance pass. This is the default.
- `SHOULD`: not required. The criterion itself must include `Why-not-MUST:` and `Acceptance passes if not met.`
- `OUT OF SCOPE`: not part of this acceptance pass.

Do not demote `MUST` to `SHOULD` unless the source or owner changes the acceptance scope and the reason is recorded.

Type is exactly `positive`, `negative`, or `boundary`.

Words such as works well, intuitive, fast, user-friendly, smooth, robust, easy, and handles errors must be rewritten with an observable signal or marked unresolved. Do not invent thresholds, sample sizes, time limits, or viewport widths.

## Edges

Consider empty state, invalid input, permissions, existing data, mobile, error recovery, accessibility, and external side effects.

Use the full table unless the change is clearly tiny, such as copy-only criteria. Compact mode lists only applicable or unknown categories, plus the categories considered and not applicable, each with a source-backed reason. `not applicable` is not "seems unrelated." Ask a question only when the answer is required to write or validate a `MUST` criterion.

## Output

```markdown
## Acceptance Criteria Tightener

Source: ...

Intended outcome:
- ...

Edge categories considered:
| Category | Status | Reason | Criteria needed |
| --- | --- | --- | --- |
| Empty state | applicable / not applicable / unknown | ... | yes / no |
| Invalid input | ... | ... | ... |
| Permissions/access | ... | ... | ... |
| Existing data/backward compatibility | ... | ... | ... |
| Mobile/responsive | ... | ... | ... |
| Error recovery | ... | ... | ... |
| Accessibility/content clarity | ... | ... | ... |
| External/side-effect behavior | ... | ... | ... |

Tightened criteria:
| ID | Level | Type | Criterion |
| --- | --- | --- | --- |
| AC-1 | MUST | positive | When <actor> <condition>, <action>; <observable result>. |

Rewritten or rejected vague criteria:
| Original | Reason | Action |
| --- | --- | --- |
| ... | ... | rewritten as AC-N / unresolved / OUT OF SCOPE |

Acceptance-blocking questions:
- ...
```

Use the full edge table or the compact block, not both. The checklist of `MUST` rows, if included, mirrors the table. Do not prescribe where to store the criteria.

## When the draft misses the contract

Read [references/examples.md](references/examples.md) and revise once. Read it only when a criterion is still subjective, a threshold was invented, or a `SHOULD` lacks its two required phrases. A draft that already matches this contract does not need the file.
