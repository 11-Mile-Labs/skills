---
name: review-comment-action-plan
description: "Turn each review comment into one disposition and an ordered action plan. Use when feedback needs a response plan before edits. Do not review the change from scratch and do not implement the comments in the same pass."
license: MIT
---

# Review Comment Action Plan

Turn feedback into a queue: change now, clarify, defer, or decline with a reason, then prove the resolution.

## Boundaries

Do not perform a review from scratch, and do not implement the feedback in this pass. If the comments are missing, the verdict is `NEEDS COMMENTS`. Ask for the source instead of inventing a ledger.

## Review rule

Every comment is accounted for exactly once as an action, clarification, deferral, duplicate, conflict, or no-action decision with a reason. Do not claim the review is handled while any comment lacks a disposition and a next step.

## Verdicts

Use exactly one:

1. **NEEDS COMMENTS.** No comments, feedback list, or usable summary.
2. **BLOCKED BY CONFLICT.** Two or more comments require incompatible outcomes and no decision owner is named.
3. **NEEDS CLARIFICATION.** At least one blocking comment is too ambiguous to act on safely.
4. **ACTION PLAN READY.** Comments can become ordered actions, responses, and proof.
5. **NO ACTION REQUIRED.** Every comment is informational, already satisfied, duplicate, out of scope with a reason, or deferred with owner and revisit trigger.

If a comment needs both clarification and action, use `NEEDS CLARIFICATION` only when the ambiguity blocks safe implementation. Otherwise use `ACTION PLAN READY` and put the question on that action.

## Dispositions

`accept`, `accept with adjustment`, `clarify`, `defer`, `no action`, `duplicate`, or `conflict`.

`no action` needs a reason a reviewer could challenge. `defer` needs the comment id, reason, owner, revisit trigger, and response text. Duplicate links to the primary action.

Assign stable ids (`C1`, `C2`) when the source has none, and use the same id everywhere. Over 25 comments, group obvious duplicates without hiding a distinct requested outcome.

Ask a question only when it blocks safe action. Name the comment, the missing decision, and the consequence of guessing.

Order actions: blocking clarifications, then correctness, data, security, permission, compatibility, or user-facing defects, then required proof, then low-risk improvements, then style, then deferrals and no-action. A style fix in the same file as a correctness fix stays a separate action unless the correctness change requires it.

Every accepted, deferred, clarified, conflict, and no-action comment gets response intent that says what changed or why not, and how it was proven. Proof matches the accepted comments.

## Output

```markdown
## Review Comment Action Plan

Verdict: NEEDS COMMENTS | BLOCKED BY CONFLICT | NEEDS CLARIFICATION | ACTION PLAN READY | NO ACTION REQUIRED

Review source:
- ...

Comment ledger:
| ID | Comment summary | Disposition | Reason | Owner / response |
| --- | --- | --- | --- | --- |
| C1 | ... | accept / accept with adjustment / clarify / defer / no action / duplicate / conflict | ... | ... |

Action batches:
1. ...
   - Comments:
   - Change to make:
   - Scope boundary:
   - Proof:
   - Response:

Clarifying questions:
- ...

Deferrals:
| Comment | Reason deferred | Owner | Revisit trigger |
| --- | --- | --- | --- |
| ... | ... | ... | ... |

Conflicts:
- ...

Verification:
- ...

Completion rule:
- Review resolution may be claimed only after every ledger row has its disposition, action or response, and proof when applicable.
```

Always include the ledger. Omit an empty optional section only when that cannot hide unresolved feedback.

## When the draft misses the contract

Read [references/examples.md](references/examples.md) and revise once. Read it only when a comment has no disposition, a deferral has no trigger, or a response is "fixed" or "won't do." A draft that already matches this contract does not need the file.
