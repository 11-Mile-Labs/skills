---
name: review-comment-action-plan
description: "Use when turning code review, design review, QA review, or stakeholder feedback comments into an ordered action plan, response plan, clarification list, or review-resolution checklist."
argument-hint: "[optional: review comments, feedback summary, PR discussion, or change request list]"
license: MIT
---

# Review Comment Action Plan

A review comment action plan turns feedback into a controlled queue: what to change now, what to clarify, what to defer, what to reject with reasons, and how to prove the resolution.

Use it to avoid blindly applying every comment, ignoring hard feedback, or losing review intent across a long thread.

## When To Use

Use this skill when asked to triage review comments, respond to feedback, resolve review threads, plan review fixes, turn comments into tickets, or decide what to do with a list of requested changes.

This skill applies to code review, design review, QA review, product review, documentation review, and implementation handoff feedback.

Do not use this skill to:

- Perform a full code review from scratch.
- Implement the feedback immediately without a plan.
- Write a tracker-specific workflow.
- Treat every comment as mandatory without classification.
- Dismiss review feedback without evidence or a response plan.

If the actual comments are missing, ask for the comment source or summarize the missing source as the first blocker.

## The Review Rule

Every review comment must be accounted for exactly once as an action, clarification, deferral, duplicate, conflict, or no-action decision with a reason.

Do not claim the review is handled while any comment lacks owner, disposition, and next step.

## Verdicts And Precedence

Use exactly one verdict:

1. **NEEDS COMMENTS:** no review comments, feedback list, or usable summary was supplied.
2. **BLOCKED BY CONFLICT:** two or more comments require incompatible outcomes and no decision owner is named.
3. **NEEDS CLARIFICATION:** at least one blocking comment is ambiguous and cannot be safely acted on.
4. **ACTION PLAN READY:** comments can be turned into ordered actions, responses, and proof steps.
5. **NO ACTION REQUIRED:** all comments are informational, already satisfied, duplicate, out of scope with reason, or intentionally deferred with owner and revisit trigger.

If any comment requires both clarification and action, choose `NEEDS CLARIFICATION` only when the ambiguity blocks safe implementation. Otherwise produce `ACTION PLAN READY` and place the clarification in the relevant action.

## Comment Dispositions

Use these dispositions in the comment ledger:

| Disposition | Use When |
| --- | --- |
| `accept` | The feedback is valid and should be implemented in this pass. |
| `accept with adjustment` | The intent is valid, but the exact suggestion should be adapted to fit constraints. |
| `clarify` | The requested outcome, scope, or acceptance condition is not specific enough to act safely. |
| `defer` | The comment is valid but outside this pass; requires owner and revisit trigger. |
| `no action` | The comment is already satisfied, informational, not applicable, or intentionally rejected with evidence. |
| `duplicate` | Another comment covers the same action; link it to the primary action. |
| `conflict` | The comment conflicts with another comment, requirement, or constraint. |

Do not use `no action` as a quiet dismissal. It needs a reason that a reviewer could challenge.

Do not use `defer` without owner, reason, and revisit trigger.

## Action Ordering

Order actions by risk and dependency:

1. Clarifications that block safe changes.
2. Correctness, data, security, permission, compatibility, or user-facing defects.
3. Required test, documentation, or proof updates tied to accepted changes.
4. Low-risk implementation improvements.
5. Style, naming, cleanup, or optional polish.
6. Deferrals and no-action responses.

When a small style fix is in the same file as a high-risk correctness fix, keep it in a separate action unless the style fix is required by the correctness change.

## Output Format

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
1. <batch name>
   - Comments: C...
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
- Review resolution may be claimed only after every comment in the ledger has the named disposition, action or response, and proof when applicable.
```

Always include the comment ledger. Omit empty optional sections only when the omission cannot hide unresolved feedback.

## Comment IDs

Assign stable IDs such as `C1`, `C2`, and `C3` when the source lacks IDs.

Use the same ID in the ledger, action batches, questions, deferrals, and responses. This prevents comment loss during implementation.

For large reviews over 25 comments, group obvious duplicates but keep every unique concern visible. If grouping hides a distinct requested outcome, split it back out.

## Clarifying Questions

Ask clarification only when it blocks safe action. Each question should name the comment, the missing decision, and the consequence of guessing.

Good:

- `C3: Should validation reject empty names or silently trim them? Guessing changes user-visible behavior and test expectations.`
- `C5: Is the request to rename the public option now, or keep the current name and add a compatibility alias? Guessing may break callers.`

Bad:

- "Can you clarify?" Bad because it does not name the missing decision or why guessing is risky.
- "What do you want me to do here?" Bad because it makes the reviewer restate everything instead of narrowing the blocker.
- "Should I fix this?" Bad because it ignores that the comment already requests a fix.

## Deferrals

A deferral is acceptable only when it includes:

1. The deferred comment ID.
2. Reason deferred now.
3. Owner or decision maker.
4. Revisit trigger.
5. Response text explaining the deferral.

Good:

- `C8 defer: valid accessibility polish, but outside the bugfix scope; owner is product owner; revisit when the settings redesign starts; response notes the deferral and trigger.`

Bad:

- "Later." Bad because it has no reason, owner, trigger, or reviewer response.
- "Out of scope." Bad because it does not explain why or where the feedback will go.
- "Nice to have." Bad because it hides whether the feedback is valid, optional, or rejected.

## No-Action Decisions

Use no-action only when the reason is explicit and reviewable.

Good:

- `C4 no action: already satisfied by the existing empty-state test; response points to the test and observed behavior.`
- `C6 no action: suggestion conflicts with the documented compatibility requirement; response names the requirement.`

Bad:

- "Won't do." Bad because it gives no evidence or constraint the reviewer can evaluate.
- "Not needed." Bad because it does not say why the comment is already satisfied, invalid, or out of scope.
- "Ignore old comment." Bad because stale feedback still needs a reason or source of supersession.

## Response Drafts

Every accepted, deferred, clarified, conflict, and no-action comment should have response intent.

Good:

- `C2 response: Accepted. I changed the parser to reject blank rows and added a proof check covering blank, valid, and malformed rows.`
- `C7 response: I handled the intent by keeping the public name stable and adding an internal helper, which avoids a breaking change.`

Bad:

- "Fixed." Bad because it does not say what changed or how it was proven.
- "Done." Bad because it does not map the response back to reviewer intent.
- "Disagree." Bad because it gives no reason, evidence, or alternative.

## Verification

Proof should match the accepted comments, not become a generic test wish list.

Good:

- `C1/C2: parser behavior check covers empty, malformed, and valid input; pass condition is row-level error without rejecting the whole file.`
- `C5: manual narrow-layout check proves the requested label does not overlap the adjacent control.`

Bad:

- "Run tests." Bad because it does not tie proof to the accepted review comments.
- "Manual QA." Bad because it hides the flow, source, and pass condition.
- "Review the diff." Bad because it is not proof that the requested behavior changed.

## Anti-Patterns

- **Comment loss:** summarizes the review but drops individual comments.
- **Blind compliance:** applies every suggestion even when comments conflict or break constraints.
- **Silent rejection:** leaves a comment unresolved without a response plan.
- **Deferral dumping:** moves difficult feedback to later without owner and trigger.
- **Clarification theater:** asks broad questions instead of naming the exact blocker.
- **Style-first ordering:** spends effort on polish while correctness comments remain unresolved.
- **Proof mismatch:** runs generic checks unrelated to the accepted comments.
- **Duplicate churn:** implements duplicate comments twice instead of linking them to one action.

## Compact Worked Examples

### Action Plan Ready

Verdict: ACTION PLAN READY

Comment ledger:
| ID | Comment summary | Disposition | Reason | Owner / response |
| --- | --- | --- | --- | --- |
| C1 | Reject blank import rows | accept | Correctness issue in changed import path | Implement and respond with proof |
| C2 | Rename helper for clarity | accept with adjustment | Improve naming without changing public API | Rename private helper only |
| C3 | Add screenshot for narrow layout | accept | User-facing layout concern | Capture proof after fix |

Action batches:
1. Correct import validation
   - Comments: C1
   - Change to make: reject blank rows with row-level error.
   - Scope boundary: no duplicate detection changes.
   - Proof: import behavior check covers blank and valid rows.
   - Response: accepted with proof result.

2. Low-risk cleanup and proof
   - Comments: C2, C3
   - Change to make: rename private helper; capture narrow-layout proof.
   - Scope boundary: no public API rename.
   - Proof: focused check plus screenshot or manual note.
   - Response: explain adjusted rename and layout evidence.

### Needs Clarification

Verdict: NEEDS CLARIFICATION

Comment ledger:
| ID | Comment summary | Disposition | Reason | Owner / response |
| --- | --- | --- | --- | --- |
| C1 | "Handle old exports too" | clarify | Missing format version and expected behavior | Ask reviewer |

Clarifying questions:
- C1: Which export versions must remain supported, and should unsupported versions show an error or be converted? Guessing may break existing users.

Completion rule: Review resolution cannot be claimed until C1 is answered or explicitly deferred.

### Blocked By Conflict

Verdict: BLOCKED BY CONFLICT

Conflicts:
- C2 asks to remove the compatibility alias; C5 asks to preserve compatibility for existing callers. Decision owner needed before implementation.

Next action: ask the owner to choose compatibility behavior before changing code.

### No Action Required

Verdict: NO ACTION REQUIRED

Comment ledger:
| ID | Comment summary | Disposition | Reason | Owner / response |
| --- | --- | --- | --- | --- |
| C1 | Add test for empty state | no action | Existing test already covers empty state and pass condition is named | Respond with test reference |
| C2 | Consider renaming title | defer | Optional copy polish outside bugfix scope; owner is product owner; revisit during copy pass | Respond with deferral trigger |

Completion rule: resolution may be claimed after the two responses are posted or included in the handoff.

## Tune When

Tune this skill when outputs:

- Drop comments instead of assigning stable IDs.
- Mark no action without a reviewable reason.
- Defer without owner and revisit trigger.
- Ask vague clarification questions.
- Implement style comments before correctness blockers.
- Treat duplicate comments as separate work.
- Use generic tests instead of proof tied to comments.
- Include bad examples without an explicit `Bad because ...` reason.
