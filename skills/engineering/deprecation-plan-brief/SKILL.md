---
name: deprecation-plan-brief
description: "Decide whether to remove a surface, deprecate it first, or gather consumer facts. Use when removing, renaming, or sunsetting behavior, APIs, fields, settings, or compatibility paths. Do not use it for private dead code with no callers, persisted data, docs, or compatibility promise."
license: MIT
---

# Deprecation Plan Brief

Name what is changing, who still depends on it, how they are warned, how compatibility is preserved, and when removal is allowed.

## Boundaries

Private dead code with no callers, persisted data, user path, docs, or compatibility promise can be removed with the evidence stated. Do not run a deprecation plan for that case.

Absence of complaints is not evidence of absent consumers.

## Deprecation rule

Do not remove a used or possibly used surface until consumers, replacement, notice, compatibility window, proof, and rollback are known, or explicitly accepted as unnecessary with evidence.

## Verdicts

Use exactly one, in this order:

1. **DO NOT REMOVE YET.** Active consumers would break, no replacement exists for required behavior, removal is irreversible, or a support or contract obligation forbids it.
2. **NEEDS CONSUMER FACTS.** Usage, consumers, owners, compatibility promises, or persisted-data impact are unknown.
3. **DEPRECATE FIRST.** Removal is not safe yet, but warning, replacement, compatibility, or migration can start now.
4. **READY TO REMOVE WITH GUARDS.** Consumers are handled, proof exists, and rollback or mitigation is named.
5. **SAFE TO REMOVE.** No consumer, persisted state, documented promise, or compatibility impact, and rollback is simple.

Do not use `SAFE TO REMOVE` for a public, documented, persisted, exported, or user-visible surface unless consumer evidence is explicit.

## Facts to capture

Surface, consumers, replacement, compatibility (alias, adapter, redirect, flag, dual-write, or read fallback), notice (audience, channel, message, window), proof, rollback, and the removal trigger.

A changelog mention is not notice by itself. Documentation is not a compatibility path by itself.

## Output

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

Keep a tiny private removal compact, and still say why the surface is private and unused.

## When the draft misses the contract

Read [references/examples.md](references/examples.md) and revise once. Read it only when a required field is missing, the verdict is unsupported, or consumer evidence was assumed. A draft that already matches this contract does not need the file.
