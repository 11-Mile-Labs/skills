# Examples

Read this file only when a draft misses the output contract: a required section is missing, a verdict is unsupported, or a field was filled in without a source.

Do not read it when the draft already matches the contract.

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

## Contrasts: Clarifying Questions

Good:

- `C3: Should validation reject empty names or silently trim them? Guessing changes user-visible behavior and test expectations.`
- `C5: Is the request to rename the public option now, or keep the current name and add a compatibility alias? Guessing may break callers.`

Bad:

- "Can you clarify?" Bad because it does not name the missing decision or why guessing is risky.
- "What do you want me to do here?" Bad because it makes the reviewer restate everything instead of narrowing the blocker.
- "Should I fix this?" Bad because it ignores that the comment already requests a fix.

## Contrasts: Deferrals

Good:

- `C8 defer: valid accessibility polish, but outside the bugfix scope; owner is product owner; revisit when the settings redesign starts; response notes the deferral and trigger.`

Bad:

- "Later." Bad because it has no reason, owner, trigger, or reviewer response.
- "Out of scope." Bad because it does not explain why or where the feedback will go.
- "Nice to have." Bad because it hides whether the feedback is valid, optional, or rejected.

## Contrasts: No-Action Decisions

Good:

- `C4 no action: already satisfied by the existing empty-state test; response points to the test and observed behavior.`
- `C6 no action: suggestion conflicts with the documented compatibility requirement; response names the requirement.`

Bad:

- "Won't do." Bad because it gives no evidence or constraint the reviewer can evaluate.
- "Not needed." Bad because it does not say why the comment is already satisfied, invalid, or out of scope.
- "Ignore old comment." Bad because stale feedback still needs a reason or source of supersession.

## Contrasts: Response Drafts

Good:

- `C2 response: Accepted. I changed the parser to reject blank rows and added a proof check covering blank, valid, and malformed rows.`
- `C7 response: I handled the intent by keeping the public name stable and adding an internal helper, which avoids a breaking change.`

Bad:

- "Fixed." Bad because it does not say what changed or how it was proven.
- "Done." Bad because it does not map the response back to reviewer intent.
- "Disagree." Bad because it gives no reason, evidence, or alternative.

## Contrasts: Verification

Good:

- `C1/C2: parser behavior check covers empty, malformed, and valid input; pass condition is row-level error without rejecting the whole file.`
- `C5: manual narrow-layout check proves the requested label does not overlap the adjacent control.`

Bad:

- "Run tests." Bad because it does not tie proof to the accepted review comments.
- "Manual QA." Bad because it hides the flow, source, and pass condition.
- "Review the diff." Bad because it is not proof that the requested behavior changed.
