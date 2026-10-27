# Examples

Read this file only when a draft misses the output contract: a required section is missing, a verdict is unsupported, or a field was filled in without a source.

Do not read it when the draft already matches the contract.

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
