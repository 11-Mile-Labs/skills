# Examples

Read this file only when a draft misses the output contract: a required section is missing, a verdict is unsupported, or a field was filled in without a source.

Do not read it when the draft already matches the contract.

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
