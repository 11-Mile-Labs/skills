---
name: scope-check
description: "Cut a plan down to the smallest shippable set. Use when a plan or feature list may be too big and the question is what to keep, defer, or split off. Do not turn the remainder into claimable work slices. Use work-slice-splitter for that."
license: MIT
---

# Scope Check

Find hidden complexity, "while we're at it" additions, and work that belongs in a later version. Bias toward shipping a smaller set.

## Boundaries

Stop after the cut list. Naming a minimum shippable set is this skill. Turning what remains into independently claimable outcomes is `work-slice-splitter`.

## Rules

1. Advocate for shipping. Prefer smaller scope and faster delivery.
2. Find the hidden multiplication. "Add X" often means "add X, Y, and Z."
3. Ask what happens if each item is cut or deferred.
4. Flag concerns, then accept the user's final call.

## Execute

If the user provided a file path, read it. Otherwise ask which plan or feature list to check.

For each major item:

1. Could this ship without it?
2. Does it require other work that is not listed?
3. Is this one thing, or secretly several?
4. Is this solving a stated problem, or a theoretical one?

```markdown
| Item | Verdict | Reason |
| --- | --- | --- |
| Feature A | KEEP | Core to the goal |
| Feature B | DEFER | Nice-to-have, adds hidden work |
| Feature C | SPLIT | Steps 1-2 now, step 3 later |
```

Propose the smallest set that still delivers value, then ask whether that cut removed something essential.

## When the draft misses the contract

This skill has no separate example file. If a row lacks a verdict or a reason, fix that row. Do not expand the audit into a slice plan.
