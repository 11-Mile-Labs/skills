---
name: test-strategy-slicer
description: "Choose the smallest proof layer for a change: none, focused, layered, or exploratory first. Use when deciding which checks to add. Do not list what might regress or write the tests. Use regression-risk-map for blast radius and verification-contract for the done claim."
license: MIT
---

# Test Strategy Slicer

Choose the smallest proof set that protects the behavior at risk. The question is which proof layer earns its keep.

## Boundaries

Do not write the tests, debug a failing test, list blast radius, or replace a completion contract. Blast radius is `regression-risk-map`. The done claim is `verification-contract`. If the changed behavior and risk are unknown, use `NEEDS RISK INPUT`.

## Strategy rule

Every proposed check maps to a changed behavior, a likely regression, or an explicit completion claim. Do not add a layer because it is fashionable. Do not skip proof because the change looks small when the blast radius is real.

## Verdicts

Use exactly one:

1. **NEEDS RISK INPUT.** Changed behavior, risk surface, or completion claim is too vague.
2. **NO NEW TEST.** Existing proof is specific and sufficient, or the change is non-behavioral and reversible. Name that proof or reason.
3. **FOCUSED PROOF.** One or two targeted checks cover the risk.
4. **LAYERED PROOF.** Unit-level confidence cannot prove integration, UI, data, permissions, or runtime behavior.
5. **EXPLORATORY FIRST.** Behavior is ambiguous, visual, workflow-heavy, or poorly specified. Explore before automating.

## Layers

| Layer | Best for | Poor fit |
| --- | --- | --- |
| Unit | Pure logic, parsing, calculation, branching | Cross-component contracts |
| Integration | Modules, persistence, permissions, external boundaries | Pixel layout or a whole journey |
| End-to-end | Critical user workflows | Every small branch |
| Contract | Public input and output shape, compatibility | Internal details |
| Migration/data | Shape, backfills, reversibility | UI-only changes |
| Visual/manual | Layout, copy, interaction | Repeated deterministic logic |
| Snapshot | Stable generated output | Noisy or incidental output |
| No new test | Docs, comments, already-proven behavior, tiny reversible copy | Behavior, data, permissions, or unknown risk |

## Output

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
| ... | ... | ... | ... | ... |

Proof not worth adding:
| Proposed proof | Why not |
| --- | --- |
| ... | ... |

Open risk questions:
- ...

Strategy call:
- Use existing proof | Add focused proof | Add layered proof | Explore first | Clarify risk
```

Omit an empty table only when the omission cannot hide a proof gap.

## When the draft misses the contract

Read [references/examples.md](references/examples.md) and revise once. Read it only when a layer is unjustified, `NO NEW TEST` names no existing proof, or the output starts writing the tests. A draft that already matches this contract does not need the file.
