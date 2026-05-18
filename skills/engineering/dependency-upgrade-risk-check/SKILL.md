---
name: dependency-upgrade-risk-check
description: "Use when assessing package, library, framework, runtime, SDK, or tool upgrades before updating, merging, releasing, or deciding what proof is required."
argument-hint: "[optional: dependency name, current version, target version, changelog notes, or upgrade diff]"
license: MIT
---

# Dependency Upgrade Risk Check

A dependency upgrade risk check classifies upgrade risk before the change lands. It separates routine patch bumps from compatibility, behavior, security, build, and ecosystem changes that need proof.

Use it to avoid treating every upgrade as either harmless housekeeping or a full migration project.

## When To Use

Use this skill before updating a dependency, reviewing an upgrade pull request, accepting an automated upgrade, changing a runtime version, or deciding how much proof an upgrade needs.

Do not use this skill to:

- Read an entire changelog aloud.
- Replace a migration plan for a known breaking upgrade.
- Recommend a specific vendor or package ecosystem.
- Approve an upgrade without changed-version facts.

If current and target versions are unknown, use `NEEDS VERSION FACTS`.

## Upgrade Rule

Do not classify an upgrade as low risk unless the version movement, runtime surface, direct usage, transitive exposure, and proof path are all known.

Unknown changelog details do not automatically mean high risk, but they do prevent a low-risk verdict.

## Verdicts And Precedence

Use exactly one verdict:

1. **DO NOT UPGRADE YET:** known blocker, incompatible runtime, known breaking change without migration, security concern in target, or rollback impossible for affected usage.
2. **NEEDS VERSION FACTS:** current version, target version, release notes, dependency role, or usage surface is missing.
3. **HIGH RISK:** major version, runtime/toolchain change, persisted data behavior, security/auth/payment/permission path, generated output, plugin API, or many direct call sites are affected.
4. **MEDIUM RISK:** minor version, behavior-affecting patch, important transitive dependency, build/test tooling, or limited direct call sites.
5. **LOW RISK:** patch or metadata-only change with known usage, no behavior surface, no runtime/tooling change, and focused proof is enough.

Do not use `LOW RISK` for a dependency that runs in production paths when usage is unknown.

## Risk Inputs

Capture these facts before deciding:

| Input | Question |
| --- | --- |
| Version movement | Is this patch, minor, major, prerelease, downgrade, or runtime line change? |
| Role | Runtime dependency, build tool, test tool, type package, plugin, SDK, or transitive dependency? |
| Usage surface | Which paths, commands, generated outputs, or workflows use it directly? |
| Change signals | Breaking notes, deprecations, security fixes, defaults, generated output changes, performance changes, or peer requirement changes? |
| Exposure | User-facing, data, permissions, auth, payments, network, filesystem, deployment, or local-only? |
| Rollback | Can the version be reverted without data or config cleanup? |
| Proof | Which focused checks prove the affected surface still works? |

## Output Format

```markdown
## Dependency Upgrade Risk Check

Verdict: DO NOT UPGRADE YET | NEEDS VERSION FACTS | HIGH RISK | MEDIUM RISK | LOW RISK

Upgrade:
- Dependency:
- Current:
- Target:
- Version movement:
- Role:

Usage surface:
- Direct:
- Transitive or generated:
- Unknowns:

Risk classification:
| Risk | Signal | Severity | Proof needed |
| --- | --- | --- | --- |
| ... | ... | low / medium / high | ... |

Proof plan:
- ...

Rollback:
- Path:
- Limit:

Open questions:
- ...

Upgrade call:
- Proceed | Proceed with proof | Plan migration | Gather facts | Do not upgrade yet
```

Keep the brief compact for obvious low-risk upgrades. Do not omit unknowns.

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

## Common Upgrade Shapes

- **Patch runtime dependency:** often low or medium risk, depending on usage and behavior surface.
- **Minor runtime dependency:** usually medium risk unless release notes and usage prove it is isolated.
- **Major dependency:** high risk until migration notes, compatibility plan, and proof are explicit.
- **Build or test tool:** medium or high when it changes generated output, transforms, module resolution, or command behavior.
- **Runtime line change:** high risk because language/runtime behavior and native dependencies may change.
- **Security update:** urgency may be high, but compatibility still needs proof.
- **Grouped upgrade:** split into independently reviewable risk groups unless all changes are metadata-only.

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

## Tune When

Tune this skill when outputs:

- Treat all patch upgrades as low risk.
- Treat all major upgrades as blockers without checking migration facts.
- Hide grouped upgrades instead of splitting risk.
- Use generic proof not tied to usage surface.
- Omit rollback limits.
- Include bad examples without an explicit `Bad because ...` reason.
