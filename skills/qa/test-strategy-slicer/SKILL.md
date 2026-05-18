---
name: test-strategy-slicer
description: "Use when choosing what tests, checks, manual proof, or verification layers a change needs, especially when deciding between unit, integration, end-to-end, exploratory, snapshot, or no new test."
argument-hint: "[optional: change summary, risk map, acceptance criteria, or existing test context]"
license: MIT
---

# Test Strategy Slicer

A test strategy slicer chooses the smallest proof set that protects the behavior at risk. It avoids both coverage theater and under-tested changes.

Use it when the question is not "more tests?" but "which proof layer earns its keep?"

## When To Use

Use this skill before adding tests, reviewing a test plan, deciding whether manual proof is enough, splitting verification work, or choosing between unit, integration, end-to-end, snapshot, contract, migration, performance, or exploratory checks.

Do not use this skill to write the tests themselves, debug a failing test, or replace a verification contract for final completion.

If the changed behavior and risk are unknown, use `NEEDS RISK INPUT`.

## Test Strategy Rule

Every proposed test or check must map to a changed behavior, likely regression, or explicit completion claim.

Do not add a test just because a layer is fashionable. Do not skip proof just because the change is small if the blast radius is real.

## Verdicts And Precedence

Use exactly one verdict:

1. **NEEDS RISK INPUT:** changed behavior, risk surface, or completion claim is too vague to choose proof.
2. **NO NEW TEST:** existing proof is specific and sufficient, or the change is truly non-behavioral and reversible.
3. **FOCUSED PROOF:** one or two targeted checks cover the risk.
4. **LAYERED PROOF:** multiple layers are needed because unit-level confidence cannot prove integration, UI, data, permissions, or runtime behavior.
5. **EXPLORATORY FIRST:** behavior is ambiguous, visual, workflow-heavy, emergent, or poorly specified; exploration must define stable checks before automation.

Do not choose `NO NEW TEST` unless you name the existing proof or the non-behavioral reason.

## Proof Layers

| Layer | Best For | Poor Fit |
| --- | --- | --- |
| Unit | Pure logic, parsing, calculation, validation, branching | Cross-component contracts or real integration behavior |
| Integration | Multiple modules, persistence, permissions, external boundaries, generated output | Pixel-level layout or broad user journey confidence |
| End-to-end | Critical user workflows, routing, forms, cross-page behavior | Every small branch or pure function |
| Contract | Public input/output shape, compatibility, consumers, adapters | Internal implementation details |
| Migration/data check | Data shape, backfills, transforms, reversibility | UI-only changes |
| Visual/manual | Layout, copy fit, affordance, interaction feel, exploratory discovery | Repeated deterministic logic |
| Snapshot/golden output | Stable generated output with meaningful diffs | Noisy, incidental, or overbroad output |
| No new test | Docs-only, comments-only, already-proven behavior, tiny reversible copy | Behavior surface, data, permissions, or unknown risk |

## Output Format

```markdown
## Test Strategy Slicer

Verdict: NEEDS RISK INPUT | NO NEW TEST | FOCUSED PROOF | LAYERED PROOF | EXPLORATORY FIRST

Change and claim:
- ...

Risk surface:
- ...

Existing proof:
- ...

Proof slices:
| Slice | Layer | Behavior or risk proven | Pass condition | Why this layer |
| --- | --- | --- | --- | --- |
| ... | unit / integration / end-to-end / contract / migration-data / visual-manual / snapshot / none | ... | ... | ... |

Proof not worth adding:
| Proposed proof | Why not |
| --- | --- |
| ... | ... |

Open risk questions:
- ...

Strategy call:
- Use existing proof | Add focused proof | Add layered proof | Explore first | Clarify risk
```

Omit empty tables only when the omission cannot hide a proof gap.

## Good And Bad Examples

### Mapping Proof To Risk

Good:

- `Unit check: parser rejects blank rows; pass condition is row-level error without rejecting valid rows.`
- `Integration check: saved preference persists after reload; pass condition is value survives round trip.`

Bad:

- "Add unit tests." Bad because it names a layer but not the behavior or risk proven.
- "Add an end-to-end test for confidence." Bad because confidence is not a pass condition.
- "Manual QA." Bad because it hides the workflow and expected result.

### Choosing No New Test

Good:

- `NO NEW TEST: docs-only typo fix; diff changes prose only; proof is review of rendered paragraph.`
- `NO NEW TEST: existing parser test already covers blank row rejection and the change only renames a private helper.`

Bad:

- "No test because small." Bad because size does not prove low blast radius.
- "No test because deadline." Bad because schedule pressure is not proof sufficiency.
- "No test because existing tests probably cover it." Bad because it does not name the existing proof.

### Layered Proof

Good:

- `LAYERED PROOF: unit validation for rule edge cases plus integration save/reload check because persistence is the real user claim.`

Bad:

- "Run everything." Bad because it may waste time without naming the behavior that needs proof.
- "Only test the helper." Bad because helper behavior does not prove the integrated user claim.

### Exploratory First

Good:

- `EXPLORATORY FIRST: gesture behavior is not specified; manually explore expected states, then encode the stable state transitions.`

Bad:

- "Just click around." Bad because exploration must produce findings or stable checks.
- "Automate later." Bad because it gives no condition for when automation is useful.

## Strategy Heuristics

- Prefer the lowest layer that proves the claim.
- Move up a layer when the risk lives in wiring, persistence, permissions, routing, runtime config, or generated output.
- Add manual or visual proof when human perception is the risk.
- Add contract proof when compatibility matters.
- Use exploratory proof before automation when expected behavior is still being discovered.
- Avoid snapshots when diffs are noisy or reviewers cannot tell good from bad.
- Split proof when one test would assert too many unrelated behaviors.

## Anti-Patterns

- **Coverage theater:** adding tests for metrics, not risk.
- **Layer mismatch:** using unit tests to prove workflow behavior or end-to-end tests to prove pure branching.
- **Everything run:** substituting broad command execution for a strategy.
- **Manual fog:** claiming manual proof without steps or pass condition.
- **Snapshot blanket:** blessing huge output diffs no one can review.
- **Deadline skip:** skipping proof because time is short without naming accepted risk.

## Compact Worked Examples

### Focused Proof

Verdict: FOCUSED PROOF

Change and claim: blank import rows show row-level errors.

Proof slices:
| Slice | Layer | Behavior or risk proven | Pass condition | Why this layer |
| --- | --- | --- | --- | --- |
| Blank row validation | Unit | parser classifies blank row correctly | blank row returns row-level error and valid row still passes | pure validation rule |

Strategy call: Add focused proof.

### Layered Proof

Verdict: LAYERED PROOF

Change and claim: users can save notification preference.

Proof slices:
| Slice | Layer | Behavior or risk proven | Pass condition | Why this layer |
| --- | --- | --- | --- | --- |
| Preference validation | Unit | invalid value rejected | invalid value returns validation error | catches edge cases cheaply |
| Save and reload | Integration | persisted preference round trip | saved value appears after reload | proves user-facing claim |

Strategy call: Add layered proof.

### No New Test

Verdict: NO NEW TEST

Change and claim: fix typo in help text.

Existing proof: rendered docs preview shows corrected text.

Strategy call: Use existing proof.

## Tune When

Tune this skill when outputs:

- Recommend tests without mapping to behavior.
- Always choose the same test layer.
- Use broad command execution as a strategy.
- Mark no-new-test without naming existing proof or non-behavior reason.
- Skip manual/visual proof for visual risk.
- Include bad examples without an explicit `Bad because ...` reason.
