---
name: implementation-drift-check
description: "Use when checking scope drift, intent vs diff alignment, PR drift, changed artifacts, unrelated edits, missing promised work, or whether a change set matches stated intent."
argument-hint: "[optional: intent source and changed-item summary]"
license: MIT
---

# Implementation Drift Check

Use this skill to compare stated intent against actual changed artifacts before commit, review, or handoff. It checks alignment to scope; it is not a deep code correctness review.

The goal is to surface unrelated edits, missing promised work, hidden generated artifacts, and suspected behavior changes before they become part of the change.

## When To Use

Use this when reviewing a diff, change set, branch, review draft, or work in progress against stated intent.

Do not use this to:

- Review code quality, style, or correctness in depth.
- Build a regression-risk map.
- Define a verification contract.
- Explain a diff without checking scope alignment.

If no usable intent source exists, do not infer one from the diff.

## The Drift Rule

Alignment can be claimed only when the intent can be restated specifically and every changed item is intended, required supporting work, expected generated output, or explicitly out of scope with action named.

Generated, derived, untracked, formatting-only, rename, move, and deletion changes must remain visible in the breakdown.

## Verdicts And Precedence

Use exactly one primary verdict.

1. **NEEDS INTENT:** no usable intent source exists, or the intent cannot be restated specifically enough to map changes.
2. **DRIFT FOUND:** any changed item is unrelated, suspicious, unexpectedly generated, out of scope, or any promised artifact/work is missing.
3. **MISSING INTENT:** some intent exists, but it is too vague to classify one or more changed items without guessing.
4. **ALIGNED:** all changed items map to stated intent, required support, or expected generated output, and no promised work is missing.

Show aligned items inside a `DRIFT FOUND` result when some items are fine and others are not.

Never emit `ALIGNED` if the changed-item breakdown contains `suspicious`, `unrelated`, or `missing expected`. If a suspicious item is discovered while filling the table, change the verdict to `DRIFT FOUND`.

## Intent Sources

Prefer intent sources in this order:

1. Explicit current user or scope instruction.
2. Plan, specification, or work item summary.
3. Prior handoff or decision record.
4. Draft commit message, branch description, or review description.

A draft commit message is weak intent. If it merely summarizes the diff after the fact, treat it as circular and use `NEEDS INTENT` unless another source exists.

Always restate intent before classifying:

- One sentence naming what changes and where.
- Optional second sentence naming explicit promises such as tests, docs, migration, proof, compatibility, or untouched areas.

If you cannot write that from the source, use `NEEDS INTENT`.

## Changed Item Source

Use the user-supplied changed-item list when provided.

When inspecting a local pre-commit change, include staged, unstaged, and untracked items. When inspecting a branch or review, use the aggregate base-to-head diff plus local working tree and untracked items when available.

Untracked files must appear in the breakdown or be explicitly noted as none seen.

For large changes over 30 changed items or 1000 changed lines, group by directory or module with representative paths, then list highest-risk suspicious, unrelated, or missing items. If intent is too broad to group honestly, use `NEEDS INTENT`.

## Classifications

Use these changed-item classifications:

| Classification | Use When |
| --- | --- |
| `intended` | Directly implements the stated intent. |
| `supporting` | Required for the intended change to work, compile, remain consistent, or be proven. Must include `required because ...`. |
| `generated/derived` | Generated, lock, snapshot, compiled, or derived artifact. Explain whether intended, supporting, or suspicious. |
| `suspicious` | May be related, but lacks a required-because reason or touches behavior surface outside intent. |
| `unrelated` | Does not map to the stated intent. |
| `missing expected` | Promised artifact or work is absent. |

Tests are `intended` only when the intent explicitly includes tests. Tests for the same behavior are usually `supporting`. Tests touching unrelated behavior are `suspicious`.

Snapshot or generated updates are supporting only when tied to a named intended behavior change. Otherwise classify them as suspicious generated/derived until reviewed.

Do not assume a generated or derived artifact is tied to intent merely because it changed in the same change set. The source, path, or supplied context must connect it to the intended behavior.

## Behavior Surface

A changed item touches behavior surface when it changes runtime control flow, public or exported signatures, persisted-data shape, validation or authorization rules, runtime-driving config, build or deploy output, or generated artifacts consumed at runtime.

Pure comments, docs prose, private-symbol renames with no call-site behavior change, and import-order-only changes do not touch behavior surface.

Treat accidental behavior change as suspicion, not fact. Say `suspected behavior change` only when the changed item touches behavior surface outside intent.

## Missing Expected Work

Missing expected work comes from explicit promises or exact nouns in the intent, such as "add a test," "add a migration," "update docs," or "change the settings page."

Do not infer missing work from best-practice assumptions. Name the exact path only when supplied or obvious from convention; otherwise name the missing category and source phrase.

## Output Format

Use the compact path only for changes with three or fewer changed items, all obviously mapped to intent, no generated/untracked/deletion/rename/move items, no behavior surface beyond intent, and no missing expected work.

```markdown
## Implementation Drift Check

Verdict: NEEDS INTENT | DRIFT FOUND | MISSING INTENT | ALIGNED

Intent restatement:
- <what changes and where; explicit promises if any>

Changed-item breakdown:
| Item | Classification | Why |
| --- | --- | --- |
| ... | intended / supporting / generated/derived / suspicious / unrelated / missing expected | ... |

Missing expected:
- <item/category/source phrase, or None>

Suspected behavior changes:
- <item and suspicion, or None>

Untracked / generated / derived visibility:
- <items or None seen>

Next action:
- Proceed to commit/review | Narrow scope | Clarify intent | Add missing artifact | Add proof for behavior surface | Justify suspicious item with required-because
```

## Good And Bad Examples

### Intent Source

Good:

```markdown
Verdict: NEEDS INTENT
Intent restatement:
- Cannot restate intent from the supplied diff summary.
Next action:
- Clarify intent: provide the requested outcome and explicit scope.
```

Bad:

```markdown
Verdict: ALIGNED
Intent restatement:
- Clean up the code.
```

Bad because vague cleanup language is not specific enough to map changed items.

### Supporting Work

Good:

```markdown
| parser test | supporting | required because the parser behavior changed and this test covers the changed behavior |
```

Bad:

```markdown
| formatting changes | supporting | general cleanup |
```

Bad because cleanup is not supporting without a specific required-because reason.

### Generated Files

Good:

```markdown
| snapshot output | generated/derived suspicious | generated file changed, but no named behavior change explains the snapshot update |
```

Bad:

```markdown
Generated files ignored.
```

Bad because generated and derived files must stay visible in the breakdown.

### Scope Filtering

Good:

```markdown
| docs/guide.md | unrelated | user asked to check source changes only, but changed items outside the scope must still be visible |
```

Bad:

```markdown
Omitted docs because user said ignore docs.
```

Bad because user-supplied scope narrows intent but does not hide changed items.

### Deletions

Good:

```markdown
| remove legacy config | suspicious | deletion is not named or implied by the intent |
```

Bad:

```markdown
Deleted old config as cleanup.
```

Bad because deletions are suspicious unless named or clearly implied by intent.

## Anti-Patterns

- Deep-reviewing code quality instead of checking scope alignment.
- Accepting cleanup or drive-by refactors as supporting without a required-because reason.
- Hiding generated, derived, untracked, formatting-only, rename, move, or deletion changes.
- Claiming `ALIGNED` when intent is vague.
- Letting scope filters omit changed items from the breakdown.
- Treating suspected behavior change as proven fact.
- Building a regression plan or verification contract.
- Using a circular commit message as the only intent source.
- Inferring missing work from best practices instead of exact intent.
- Quietly accepting deletions as cleanup.

## Compact Worked Examples

### ALIGNED

Verdict: ALIGNED

Intent restatement:
- Fix the typo in the help text.

Changed-item breakdown:
| Item | Classification | Why |
| --- | --- | --- |
| help text file | intended | directly changes the typo named in intent |

Missing expected: None.
Suspected behavior changes: None.
Untracked / generated / derived visibility: None seen.
Next action: Proceed to commit/review.

### DRIFT FOUND

Verdict: DRIFT FOUND

Intent restatement:
- Add row-level validation messages to import preview. Intent promises a focused validation test.

Changed-item breakdown:
| Item | Classification | Why |
| --- | --- | --- |
| import preview view | intended | implements row-level validation messages |
| account settings cleanup | unrelated | no required-because reason connects it to import preview |
| validation test | missing expected | intent explicitly promised a focused validation test |

Missing expected:
- Focused validation test from intent phrase "promises a focused validation test."

Suspected behavior changes:
- account settings cleanup touches behavior surface outside intent.

Next action:
- Narrow scope: remove account settings cleanup.
- Add missing artifact: focused validation test.

### NEEDS INTENT

Verdict: NEEDS INTENT

Intent restatement:
- Cannot restate what should change and where from "polish the branch."

Changed-item breakdown:
| Item | Classification | Why |
| --- | --- | --- |
| source files | suspicious | no usable intent source maps these changes |

Next action:
- Clarify intent: provide the requested outcome, scope in, and scope out.

## Tune When

Tune this skill when outputs:

- Claim `ALIGNED` without restating specific intent.
- Hide untracked, generated, derived, formatting, rename, move, or deletion items.
- Treat cleanup as supporting without `required because`.
- Omit missing promised artifacts.
- Deep-review correctness instead of intent alignment.
- Build a regression plan or verification contract.
- Let user filters hide changed items.
- Treat suspected behavior change as fact.
- Produce huge per-file tables for large diffs instead of grouping.
- Include bad examples without an explicit `Bad because ...` reason.
