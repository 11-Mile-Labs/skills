# Examples

Read this file only when a draft misses the output contract: a required section is missing, a verdict is unsupported, or a field was filled in without a source.

Do not read it when the draft already matches the contract.

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
