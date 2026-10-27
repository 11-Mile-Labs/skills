---
name: verification-contract
description: "Define the evidence required before calling work done. Use when writing or auditing a completion claim. Do not rewrite acceptance criteria or make a release go/no-go call. Use acceptance-criteria-tightener for pass/fail wording and release-readiness-brief for shipping."
license: MIT
---

# Verification Contract

State what must be true, how to know, and which gaps remain accepted. Use it before work starts, or before claiming done.

## Boundaries

Do not run an already-known command just because it exists. Do not rewrite acceptance criteria (`acceptance-criteria-tightener`) or make a release call (`release-readiness-brief`). If a contract already exists for this scope, update it.

## Completion rule

Do not claim completion unless every in-scope behavior has re-checkable evidence with a pass condition, or an accepted gap with all four fields. If evidence is missing and the gap is not accepted, completion is not proven.

## Depth

1. Data loss, permission or security failure, money, irreversible migration, or critical-path breakage: **high-risk**.
2. Rollback hard, slow, or unproven: **high-risk**.
3. Shared behavior, integrations, background jobs, analytics, or user-visible workflows: **standard**, unless a higher rule applies.
4. Reversible in one step, isolated, no shared state, low user-visible risk: **tiny**.

Default to **standard** when the answers are unclear, and list that ambiguity.

Tiny is 3-5 verification rows plus unresolved or accepted gaps. Standard adds behavior, evidence, pass conditions, residual risks, and the completion rule. High-risk adds rollback, monitoring, negative-path, and post-change evidence.

## Evidence and gaps

Evidence is re-checkable from the entry alone, or explicitly labeled as judgment. A usable entry names the source, the pass condition, and enough detail to rerun or challenge it. An activity ("tested it", "QA") is not evidence.

An accepted gap needs all four: acceptor (named person or specific role), reason accepted now, risk if wrong, and reopen trigger. A missing field means the gap is unresolved. Do not invent acceptance. Unknown requirements and undecided choices are unresolved, not accepted.

A pass condition describes the result, not the act of checking.

If the workflow has a durable home, put the contract there. Do not prescribe a project path. When updating a contract, preserve prior decisions unless they are stale, and say what changed.

## Output

```markdown
## Verification Contract

Depth: tiny | standard | high-risk

Outcome to prove:
- ...

In scope:
- ...

Out of scope:
- ...

Verification matrix:
| Behavior or risk | Method | Evidence to collect | Pass condition |
| --- | --- | --- | --- |
| ... | ... | ... | ... |

Manual exploration:
- ...

Accepted gaps:
| Gap | Acceptor | Reason accepted now | Risk if wrong | Reopen trigger |
| --- | --- | --- | --- | --- |
| ... | ... | ... | ... | ... |

Unresolved verification gaps:
- ...

Completion claim rule:
- Completion may be claimed only after ...
```

Omit an empty optional section only when that cannot hide risk.

## When the draft misses the contract

Read [references/examples.md](references/examples.md) and revise once. Read it only when evidence is an activity, a gap is missing a field, or completion is claimed with unresolved gaps. A draft that already matches this contract does not need the file.
