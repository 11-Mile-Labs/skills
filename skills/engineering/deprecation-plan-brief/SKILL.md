---
name: deprecation-plan-brief
description: "Use when removing, renaming, replacing, sunsetting, disabling, or deprecating behavior, APIs, fields, settings, workflows, docs, flags, or compatibility paths."
argument-hint: "[optional: thing to deprecate, replacement, consumers, timeline, or removal request]"
license: MIT
---

# Deprecation Plan Brief

A deprecation plan brief makes removal safe. It names what is changing, who or what still depends on it, how users are warned, how compatibility is preserved, and when removal is allowed.

Use it to prevent "cleanup" from becoming an unannounced breaking change.

## When To Use

Use this skill when a change removes, renames, disables, hides, replaces, sunsets, or stops supporting an existing behavior, public contract, data field, setting, workflow, output, or documented path.

Do not use this skill for private dead code that has no callers, no persisted data, no user path, no docs, and no compatibility promise. For that case, record the evidence briefly and remove it.

If consumers or replacement path are unknown, use `NEEDS CONSUMER FACTS`.

## Deprecation Rule

Do not remove a used or possibly used surface until consumers, replacement, notice, compatibility window, proof, and rollback are known or explicitly accepted as unnecessary with evidence.

Absence of complaints is not evidence of absence of consumers.

## Verdicts And Precedence

Use exactly one verdict:

1. **DO NOT REMOVE YET:** active consumers would break, no replacement exists for required behavior, removal is irreversible, or support/contract obligations forbid removal.
2. **NEEDS CONSUMER FACTS:** usage, consumers, owners, compatibility promises, or persisted-data impact are unknown.
3. **DEPRECATE FIRST:** removal is not safe yet, but warning, replacement, compatibility, or migration can start now.
4. **READY TO REMOVE WITH GUARDS:** consumers are handled, proof exists, rollback or mitigation is named, and removal can proceed with checks.
5. **SAFE TO REMOVE:** evidence shows no consumer, no persisted state, no documented promise, no compatibility impact, and rollback is simple.

Do not use `SAFE TO REMOVE` for public, documented, persisted, exported, or user-visible surfaces unless consumer evidence is explicit.

## Deprecation Inputs

| Input | Question |
| --- | --- |
| Surface | What behavior, field, setting, command, output, workflow, or documentation is changing? |
| Consumers | Who or what uses it directly, indirectly, or through stored data? |
| Replacement | What should consumers use instead? |
| Compatibility | Is an alias, adapter, redirect, feature flag, dual-write, or read fallback needed? |
| Notice | Who needs warning, where, and how long before removal? |
| Proof | What shows consumers are migrated or absent? |
| Rollback | How will removal be reversed or mitigated if a consumer appears? |
| Removal trigger | What event, date, usage threshold, or owner approval allows final removal? |

## Output Format

```markdown
## Deprecation Plan Brief

Verdict: DO NOT REMOVE YET | NEEDS CONSUMER FACTS | DEPRECATE FIRST | READY TO REMOVE WITH GUARDS | SAFE TO REMOVE

Surface:
- ...

Replacement:
- ...

Consumer map:
| Consumer or source | Evidence | Status | Action |
| --- | --- | --- | --- |
| ... | ... | active / unknown / migrated / absent | ... |

Compatibility plan:
- ...

Notice plan:
- Audience:
- Channel or location:
- Message:
- Window:

Proof plan:
- ...

Rollback or mitigation:
- Path:
- Trigger:
- Owner:

Removal trigger:
- ...

Deprecation call:
- Remove | Remove with guards | Deprecate first | Gather facts | Do not remove yet
```

Keep tiny private removals compact, but still state why the surface is private and unused.

## Good And Bad Examples

### Consumer Evidence

Good:

- `Search of runtime imports, documentation links, and saved configuration keys found no references to oldExportMode.`
- `Usage metric for legacy endpoint stayed at zero for 30 days after warning banner shipped.`

Bad:

- "Nobody should use this." Bad because expectation is not consumer evidence.
- "No one complained." Bad because silent consumers may not report until removal breaks them.
- "Looks unused." Bad because it does not name the sources checked.

### Replacement

Good:

- `Replacement: use importMode=preview; oldExportMode remains as an alias for one release window.`

Bad:

- "Use the new way." Bad because it does not name the replacement or migration path.
- "Docs will explain it." Bad because documentation is not a compatibility path by itself.

### Notice

Good:

- `Notice: add warning to old setting docs and runtime warning for administrators for two release windows.`

Bad:

- "Tell users." Bad because it does not name audience, location, message, or window.
- "Mention in changelog." Bad because a changelog may not reach active consumers or give them time to migrate.

### Removal

Good:

- `READY TO REMOVE WITH GUARDS: usage is zero for 30 days, replacement shipped, alias rollback is one-line restore, and release owner watches unknown-key errors for one week.`

Bad:

- "Delete it as cleanup." Bad because removal can break consumers even when the code looks messy.
- "Remove and see what happens." Bad because production discovery is not a deprecation plan.

## Compatibility Patterns

- **Alias:** keep old name forwarding to new name while consumers migrate.
- **Adapter:** translate old input/output shape into the new shape.
- **Redirect:** route old location to new location.
- **Dual support:** accept both old and new values during the migration window.
- **Read fallback:** continue reading old stored data while writing the new shape.
- **Feature flag or setting:** allow quick disablement of the new removal behavior.

Choose the smallest compatibility pattern that protects real consumers.

## Anti-Patterns

- **Cleanup camouflage:** presenting a breaking removal as harmless cleanup.
- **Consumer guessing:** assuming no consumers because none are visible in the current file.
- **Notice theater:** posting a warning where consumers will not see it.
- **Forever deprecation:** adding warnings without a removal trigger.
- **Compatibility drift:** keeping aliases without proof they still work.
- **Rollback fantasy:** deleting persisted-data support without a restore path.

## Compact Worked Examples

### Deprecate First

Verdict: DEPRECATE FIRST

Surface: old import option `legacyMode`.

Replacement: `importMode=preview`.

Consumer map:
| Consumer or source | Evidence | Status | Action |
| --- | --- | --- | --- |
| Saved customer settings | key exists in stored config | active | keep read fallback and show warning |

Compatibility plan: accept both values, write only the replacement value after save.

Removal trigger: zero legacy reads for two release windows.

Deprecation call: Deprecate first.

### Safe To Remove

Verdict: SAFE TO REMOVE

Surface: private helper only referenced by deleted test fixture.

Consumer map: source search found no imports, docs links, runtime config, or persisted keys.

Rollback: restore helper from previous commit if needed.

Deprecation call: Remove.

### Needs Consumer Facts

Verdict: NEEDS CONSUMER FACTS

Open facts:
- Is the field stored in existing records?
- Is the output documented?
- Do any integrations parse it?

Deprecation call: Gather facts before removal.

## Tune When

Tune this skill when outputs:

- Say "safe to remove" without consumer evidence.
- Skip replacement path.
- Treat changelog mention as enough notice for active consumers.
- Deprecate forever without a removal trigger.
- Ignore persisted data, config, generated output, or documentation promises.
- Include bad examples without an explicit `Bad because ...` reason.
