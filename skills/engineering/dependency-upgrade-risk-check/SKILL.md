---
name: dependency-upgrade-risk-check
description: "Classify one dependency upgrade and name the proof it needs. Use before updating, merging, or accepting an automated bump. Do not plan a known breaking migration, and do not call an upgrade low risk when the version movement or usage is unknown."
license: MIT
---

# Dependency Upgrade Risk Check

Classify upgrade risk before the change lands. Separate a routine bump from a compatibility, behavior, security, build, or ecosystem change that needs proof.

## Boundaries

Stop at the risk call and the proof plan. A known breaking upgrade that already needs a migration plan is out of scope. Unknown current or target versions are `NEEDS VERSION FACTS`, not a guessed risk level.

## Upgrade rule

Do not classify an upgrade as low risk unless the version movement, runtime surface, direct usage, transitive exposure, and proof path are all known.

Unknown changelog details do not mean high risk. They do block a low-risk verdict.

Do not use `LOW RISK` for a dependency that runs in production paths when usage is unknown.

## Verdicts

Use exactly one, in this order:

1. **DO NOT UPGRADE YET.** Known blocker, incompatible runtime, breaking change without a migration, security concern in the target, or rollback is impossible for the affected usage.
2. **NEEDS VERSION FACTS.** Current version, target version, release notes, role, or usage surface is missing.
3. **HIGH RISK.** Major version, runtime or toolchain change, persisted data, security, auth, payment, permission path, generated output, plugin API, or many direct call sites.
4. **MEDIUM RISK.** Minor version, behavior-affecting patch, important transitive dependency, build or test tooling, or limited direct call sites.
5. **LOW RISK.** Patch or metadata-only change with known usage, no behavior surface, no runtime or tooling change, and a focused proof path.

## Facts to capture

Version movement, role (runtime, build, test, types, plugin, SDK, or transitive), usage surface, change signals, exposure, rollback, and the proof that matches that surface.

Use the project's package manager for version and dependency facts. Do not guess them.

## Shapes

- Patch runtime dependency: low or medium, depending on usage and behavior.
- Minor runtime dependency: medium unless notes and usage show it is isolated.
- Major dependency: high until migration notes, compatibility, and proof are explicit.
- Build or test tool: medium or high when it changes generated output, transforms, resolution, or command behavior.
- Runtime line change: high.
- Security update: urgency does not replace compatibility proof.
- Grouped upgrade: split into reviewable groups unless every change is metadata-only.

## Output

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

Keep an obvious low-risk upgrade compact. Do not omit unknowns.

## When the draft misses the contract

Read [references/examples.md](references/examples.md) and revise once. Read it only when a required field is missing, the verdict is unsupported, or proof is generic. A draft that already matches this contract does not need the file.
