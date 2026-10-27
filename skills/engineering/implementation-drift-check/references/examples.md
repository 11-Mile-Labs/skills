# Examples

Read this file only when a draft misses the output contract: a required section is missing, a verdict is unsupported, or a field was filled in without a source.

Do not read it when the draft already matches the contract.

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
