# Examples

Read this file only when a draft misses the output contract: a required section is missing, a verdict is unsupported, or a field was filled in without a source.

Do not read it when the draft already matches the contract.

## Good And Bad Examples

### Version Facts

Good:

- `Library: parser-core. Current: 2.4.1. Target: 2.4.3. Movement: patch. Role: runtime parser used by import preview.`

Bad:

- "Update parser-core." Bad because it does not name current version, target version, movement, or role.
- "Bump everything." Bad because grouped upgrades hide individual risk and proof needs.

### Risk Classification

Good:

- `Medium risk: minor runtime SDK update touches upload path; proof needed is upload success, auth failure, and retry behavior.`

Bad:

- "Patch bumps are safe." Bad because patch releases can still alter behavior or generated output.
- "Major version means impossible." Bad because a major upgrade may be safe with a clear migration and proof plan.
- "Automated upgrade is trustworthy." Bad because source of the change does not prove compatibility.

### Proof

Good:

- `Proof: run import parser behavior check for valid row, invalid row, duplicate row, and empty file; pass condition is same accepted/rejected outcomes as before.`

Bad:

- "Run tests." Bad because it does not tie proof to the dependency usage surface.
- "Build passes." Bad because build success does not prove runtime behavior.
- "Smoke test it." Bad because it does not name the flow or pass condition.

### Rollback

Good:

- `Rollback: revert dependency lockfile and package manifest; no data or config migration; owner is change owner.`

Bad:

- "Can revert." Bad because it does not say whether data, generated output, config, or peer requirements also need cleanup.
- "Roll back if broken." Bad because it has no trigger or limit.

## Anti-Patterns

- **Version numerology:** deciding only from patch/minor/major labels.
- **Grouped fog:** reviewing many unrelated upgrades as one risk.
- **Proof mismatch:** proving installation while affected runtime behavior remains untested.
- **Changelog dumping:** pasting release notes without a decision or proof plan.
- **Automated trust:** treating generated upgrade PRs as safer than human changes.
- **Rollback theater:** assuming manifest revert is enough when migrations, generated files, or peer requirements changed.

## Compact Worked Examples

### Low Risk

Verdict: LOW RISK

Upgrade: type-only helper from 1.2.1 to 1.2.2; patch; development-only role.

Usage surface: type declarations only; no runtime import; no generated output.

Proof plan: typecheck affected package; pass condition is no type errors.

Rollback: revert manifest and lockfile.

Upgrade call: Proceed with focused proof.

### Medium Risk

Verdict: MEDIUM RISK

Upgrade: import parser 2.4.1 to 2.5.0; minor; runtime dependency.

Risk classification:
| Risk | Signal | Severity | Proof needed |
| --- | --- | --- | --- |
| Accepted rows may parse differently | minor runtime parser update | medium | valid, invalid, blank, and duplicate row behavior checks |

Upgrade call: Proceed only with focused parser proof.

### Needs Facts

Verdict: NEEDS VERSION FACTS

Open questions:
- What is the current version?
- What is the target version?
- Is this dependency used at runtime or only by local checks?

Upgrade call: Gather facts before risk classification.
