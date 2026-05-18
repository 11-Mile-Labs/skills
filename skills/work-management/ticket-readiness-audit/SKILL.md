---
name: ticket-readiness-audit
description: "Use when checking whether a ticket, issue, backlog item, work item, or implementation request is ready to claim, groom, split, unblock, or send back for clarification."
argument-hint: "[optional: ticket, issue, backlog item, work item, or implementation request]"
license: MIT
---

# Ticket Readiness Audit

A ticket is ready to claim only when a competent implementer can start without guessing, stay inside scope, identify blockers, and know how done will be recognized.

Use this skill to audit claimability before work starts, during grooming, or when deciding whether to pick up a work item mid-stream.

## When To Use

Use this skill when asked whether a ticket, issue, backlog item, work item, or implementation request is ready to claim, ready for development, ready for implementation, blocked, too broad, stale, or missing information.

Do not use this skill to create a ticket from scratch, write a PRD, estimate work, prioritize a backlog, or run a tracker-specific workflow.

This skill checks that a proof path exists. It does not build a full verification matrix. If the user asks to define the evidence contract, use a verification-contract workflow separately.

If a prior readiness audit exists for the same scope, reconcile or update it instead of producing a competing audit.

## The Readiness Rule

Do not claim a ticket unless the Claimability Test passes on all five questions and no higher verdict applies.

Do not fill missing outcome, scope, proof path, or ownership from assumptions and then mark the ticket ready.

## Verdicts And Precedence

Use these exact verdicts in this order:

1. **DO NOT CLAIM:** stale, duplicate, invalid, obsolete, or no longer aligned. Send it to the ticket owner for rewrite, closure, or archive decision.
2. **BLOCKED:** an external dependency, access need, decision, environment, or owner response prevents progress.
3. **SPLIT:** the ticket is too broad, but the intended outcome is clear enough to propose claimable slices.
4. **NEEDS CLARIFICATION:** missing information prevents a safe start or safe split.
5. **READY:** nothing above applies and the Claimability Test passes.

When emitting any verdict below DO NOT CLAIM, include a short `Ruled out:` line naming the higher-precedence verdicts ruled out and why. The line must not mention lower-precedence verdicts by name.

Example: for BLOCKED, rule out only DO NOT CLAIM. Do not also rule out SPLIT, NEEDS CLARIFICATION, or READY.

Choose SPLIT only when claimable slices can be named without inventing product intent. If splitting requires guessing, choose NEEDS CLARIFICATION and ask the question that unlocks splitting.

## Readiness Gates

Audit whether the ticket states:

- **Outcome:** what should be true when the work is complete.
- **Value:** who benefits or why the work matters.
- **Scope:** what is in scope and what is out of scope.
- **Constraints:** limits, compatibility needs, policy, performance, security, data, or UX constraints.
- **Dependencies:** services, people, decisions, data, access, environments, or prior work needed.
- **Proof path or done signal:** at least one observable way to know the work is done, without requiring a full verification matrix.
- **Owner or decision maker:** who answers blocking questions or accepts scope decisions.
- **Blockers:** known blockers, or an explicit statement that none are known.

## Claimability Test

Answer these five questions in order. Any "no" prevents READY:

1. Can a competent implementer start without guessing the desired outcome?
2. Can they stay within scope without inventing details the ticket does not specify?
3. Does the ticket name at least one observable done signal or proof path, without requiring a full verification matrix?
4. Are known dependencies, blockers, access needs, and environment or data needs named or explicitly absent?
5. Can they identify who answers open questions when they arise mid-work?

## Output Format

```markdown
## Ticket Readiness Audit

Verdict: READY | NEEDS CLARIFICATION | SPLIT | BLOCKED | DO NOT CLAIM
Ruled out: ...

Ticket snapshot:
- Outcome:
- In scope:
- Out of scope:
- Dependencies and access needs:
- Proof path or done signal:
- Owner or decision maker:

Claimability Test:
1. ...
2. ...
3. ...
4. ...
5. ...

Missing information:
| Missing item | Impact if claimed now | Question to ask | Who answers |
| --- | --- | --- | --- |
| None found | None | None | None |

Split suggestions:
- Slice: outcome; scope boundary; likely proof path or done signal; remaining question if any.

Readiness summary:
- ...

Next action:
- ...
```

Always include the missing-information table. Omit split suggestions unless the verdict is SPLIT.

## Good And Bad Examples

### Outcomes

Good:

- "Users can reset a forgotten password and sign in with the new password."

Bad:

- "Improve auth." Bad because it names an area, not the desired result.
- "Make settings better." Bad because "better" gives no implementable or reviewable outcome.

### Proof Paths / Done Signals

Good:

- "Done when a user can save the preference, reload the page, and see the saved value."
- "Done when invalid imports show a row-level error and valid rows still import."

Bad:

- "Add tests." Bad because it names an activity, not the observable behavior that proves readiness.
- "QA will check it." Bad because it names a role but not what must be checked or what passing means.

### Blockers And Dependencies

Good:

- "Blocked until test account access is granted by the project owner."
- "Depends on the export format decision from the product owner."

Bad:

- "Need backend stuff." Bad because it does not name the dependency, owner, or needed decision.
- "Probably blocked." Bad because it states uncertainty without the question or owner needed to resolve it.

### Split Suggestions

Good:

- "Slice: save email preference; boundary: account settings only; done signal: save and reload shows the value; remaining question: default value for new users."

Bad:

- "Slice 1: backend. Slice 2: frontend. Slice 3: tests." Bad because the slices are component labels, not claimable outcomes with proof paths.
- "Split into six tiny tasks." Bad because it does not show that each slice is independently claimable.

## Persistence Guidance

If the workflow has a durable system, place the audit where future work will look first: ticket body, ticket comment, PR description, plan, or project documentation.

Do not prescribe a project path or tracker. If no durable system exists, keep the audit inline and make it copy-ready.

When updating a prior audit, preserve decisions that are still true and call out what changed.

## Anti-Patterns

- **Invented readiness:** fills missing outcome, scope, proof path, or ownership from assumption and marks READY.
- **Clarification theater:** asks questions that do not actually block claiming the work.
- **Aggressive splitting:** splits a normal-sized claimable ticket into tiny component chores.
- **Stall via DO NOT CLAIM:** uses DO NOT CLAIM because the work is unfamiliar or undesirable, not because the ticket is stale, duplicate, invalid, obsolete, or unaligned.
- **Verdict shopping:** picks the verdict that requires the least follow-up instead of applying precedence.

## Compact Worked Examples

### READY

Verdict: READY
Ruled out: DO NOT CLAIM because the work is current; BLOCKED because no external dependency is named; SPLIT because the scope is narrow; NEEDS CLARIFICATION because all claimability questions pass.

Ticket snapshot: outcome is "users can export filtered invoices"; scope is one export button and existing filters; dependencies are explicitly absent; done signal is "exported file contains only filtered rows"; owner is product owner.

Missing information: None found.
Next action: claim the ticket.

### NEEDS CLARIFICATION

Verdict: NEEDS CLARIFICATION
Ruled out: DO NOT CLAIM because the work may still be valid; BLOCKED because no external dependency is named; SPLIT because the outcome is too vague to split safely.

Missing information: outcome is "improve reports"; proof path is absent; owner is not named.
Question to ask: "Which report behavior should change, and what observable result proves the improvement?"

Next action: ask the ticket owner before claiming.

### SPLIT

Verdict: SPLIT
Ruled out: DO NOT CLAIM because the request is current; BLOCKED because required access is available.

Ticket: "Add customer import with preview, validation, duplicate detection, and rollback."

Split suggestions:
- Slice: upload and preview file; boundary: parse only, no save; done signal: preview shows parsed rows and parse errors.
- Slice: validate rows; boundary: validation messages only; done signal: invalid rows show specific reasons.
- Slice: save valid rows; boundary: no duplicate merge; done signal: valid rows persist and invalid rows do not.

Next action: rewrite into claimable slices before implementation.

### BLOCKED

Verdict: BLOCKED
Ruled out: DO NOT CLAIM because the work is active and aligned.

Ticket snapshot: outcome is clear, but production-like test data is required and not available; owner is data steward.

Missing information: access grant date and data owner approval.
Next action: request access or a safe fixture before claiming.

## Tune When

Tune this skill when outputs:

- Mark READY while any Claimability Test answer is no.
- Ask non-blocking clarification questions to avoid work.
- Split tickets into component chores instead of claimable slices.
- Use DO NOT CLAIM for unfamiliar but valid work.
- Build a full verification matrix instead of checking that a proof path exists.
- Omit the owner or decision maker.
- Drop the missing-information table on READY tickets.
