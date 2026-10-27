---
name: implementation-drift-check
description: "Compare stated intent with the actual change set and classify each change. Use before commit, review, or handoff when the question is whether the diff matches the intent. Do not review code quality, map regressions, or define done. Use regression-risk-map for what could break."
license: MIT
---

# Implementation Drift Check

Compare stated intent with the changed artifacts. This checks alignment. It is not a correctness review.

## Boundaries

Do not review code quality, build a regression map, or define a verification contract. If there is no usable intent source, do not infer one from the diff. A draft commit message that only restates the diff is circular. Use `NEEDS INTENT` unless another source exists.

## Drift rule

Alignment can be claimed only when the intent can be restated specifically and every changed item is intended, required supporting work, expected generated output, or explicitly out of scope with an action named.

Generated, derived, untracked, formatting-only, rename, move, and deletion changes stay visible.

## Verdicts

Use exactly one:

1. **NEEDS INTENT.** No usable intent source, or the intent cannot be restated specifically enough to map changes.
2. **DRIFT FOUND.** Any changed item is unrelated, suspicious, unexpectedly generated, or out of scope, or any promised work is missing.
3. **MISSING INTENT.** Some intent exists, but it is too vague to classify one or more items without guessing.
4. **ALIGNED.** Every item maps to stated intent, required support, or expected generated output, and no promised work is missing.

Show aligned items inside a `DRIFT FOUND` result. Never emit `ALIGNED` if any item is `suspicious`, `unrelated`, or `missing expected`.

## Intent sources

Prefer, in order: the current user or scope instruction, a plan or work item, a prior handoff or decision record, then a draft commit or review description.

Restate intent in one sentence naming what changes and where. Add a second sentence for explicit promises such as tests, docs, migration, or untouched areas. If that cannot be written from the source, use `NEEDS INTENT`.

## Changed items

Use the supplied list when present. For a local change, include staged, unstaged, and untracked items. For a branch or review, use the base-to-head diff plus local untracked items when available.

Over 30 items or 1000 lines, group by directory or module and list the highest-risk suspicious, unrelated, or missing items. If the intent is too broad to group honestly, use `NEEDS INTENT`.

## Classifications

| Classification | Use when |
| --- | --- |
| `intended` | Directly implements the stated intent. |
| `supporting` | Required for the intended change to work, compile, stay consistent, or be proven. Include `required because ...`. |
| `generated/derived` | Generated, lock, snapshot, compiled, or derived. Say whether it is intended, supporting, or suspicious. |
| `suspicious` | May be related, but lacks a required-because reason or touches behavior outside intent. |
| `unrelated` | Does not map to the stated intent. |
| `missing expected` | Promised artifact or work is absent. |

Tests are `intended` only when the intent includes tests. Tests for the same behavior are usually `supporting`. Tests for unrelated behavior are `suspicious`.

A generated update is supporting only when tied to a named intended behavior change. Do not treat "it changed in the same set" as that tie.

Behavior surface means runtime control flow, public or exported signatures, persisted-data shape, validation or authorization, runtime-driving config, build or deploy output, or generated artifacts consumed at runtime. Comments, docs prose, private renames with no call-site behavior change, and import-order-only changes do not.

Missing work comes from explicit promises or exact nouns in the intent. Do not invent missing work from general practice.

## Output

Use the compact path only for three or fewer items, all obviously mapped, with no generated, untracked, deletion, rename, or move items, no behavior beyond intent, and no missing expected work.

```markdown
## Implementation Drift Check

Verdict: NEEDS INTENT | DRIFT FOUND | MISSING INTENT | ALIGNED

Intent restatement:
- ...

Changed-item breakdown:
| Item | Classification | Why |
| --- | --- | --- |
| ... | intended / supporting / generated/derived / suspicious / unrelated / missing expected | ... |

Missing expected:
- ...

Suspected behavior changes:
- ...

Untracked / generated / derived visibility:
- ...

Next action:
- Proceed to commit/review | Narrow scope | Clarify intent | Add missing artifact | Add proof for behavior surface | Justify suspicious item with required-because
```

## When the draft misses the contract

Read [references/examples.md](references/examples.md) and revise once. Read it only when a required field is missing, the verdict contradicts the table, or intent was inferred from the diff. A draft that already matches this contract does not need the file.
