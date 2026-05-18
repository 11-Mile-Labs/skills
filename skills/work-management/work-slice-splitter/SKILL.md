---
name: work-slice-splitter
description: "Use when a request, ticket, feature idea, epic, or implementation plan is too broad and needs outcome-based slices before execution or claiming."
argument-hint: "[optional: broad request, ticket, epic, feature idea, or plan]"
license: MIT
---

# Work Slice Splitter

Use this skill to turn broad work into independently claimable outcome slices. It creates a slice plan; it does not decide whether each slice is ready to claim.

The goal is to avoid component chores, invented product intent, and tiny busywork while preserving sequence, blockers, and lightweight done signals.

## When To Use

Use this when a request, ticket, epic, feature idea, or implementation plan is too broad and needs smaller slices before execution.

Do not use this when:

- The work is already one claimable outcome.
- Slicing would orphan in-flight changes; make a handoff or drift check first.
- The request is a research spike with no defined outcome.
- An urgent hotfix has one safe outcome and slicing would delay delivery.

If a prior readiness audit suggested slices, reconcile and refine those suggestions instead of silently replacing them.

## The Slice Rule

Each slice must have an independently observable user, business, or enabling outcome, scope in/out, dependencies or blockers, a lightweight done signal, and a sequence label.

Do not split below an observable outcome.

## Verdicts

Use exactly one verdict.

| Verdict | Use When |
| --- | --- |
| `SLICE` | At least one independent outcome can be sliced safely. Clear slices may be emitted while unclear remainder is deferred. |
| `CLARIFY BEFORE SLICING` | Unknowns affect every possible slice or product intent is too vague to form safe outcome slices. |
| `DO NOT SPLIT` | The work has one claimable outcome, is tiny, is an urgent single-outcome hotfix, or splitting would create busywork. |

For partial clarity, use `SLICE` for source-backed slices and add a `Deferred / needs clarification` section for the unclear remainder.

## Slice Shape

Use this strict template for every slice:

```markdown
Slice: <name>
Outcome: <user, business, or enabling outcome>
Scope in: <included behavior or work>
Scope out: <excluded behavior or work>
Dependencies/blockers: <none, or named dependency/blocker plus unlock condition>
Done signal: <one observable result, not a full verification contract>
Sequence: first safe | parallelizable | blocked | later | deferred-needs-clarification
```

The done signal can be user-observable or developer/system-observable. It must be tied to the slice outcome and observable by a reviewer.

Good done signal:

- `Preview shows parsed rows and row-level errors without saving.`
- `Existing sign-in behavior reads from the new schema with old behavior preserved.`

Bad:

- `Write tests.` Bad because it names an activity, not the observable result of the slice.

## Slicing Rules

Prefer vertical outcome slices over component or team chores.

Allowed slice types:

- User-facing outcome slices.
- Business outcome slices.
- Enabling slices that preserve or unlock observable behavior.
- Risk-reducing prerequisite slices that make later work safer.

Not allowed:

- Backend/frontend/tests as separate slices when they cannot be claimed as outcomes.
- Tiny activity chores.
- Slices that share the same done signal.
- Slices that cannot be reviewed or stopped after independently.

Target 2-5 meaningful slices for broad work. More than 5 requires a clear reason such as independent workflows or blockers.

Merge slices that share a done signal, cannot ship independently, are merely component steps, or create coordination overhead without reducing risk.

## First Safe Slice

Always name the first safe slice.

Choose in this order:

1. Prerequisite or blocker-reducing slice.
2. Lowest blast radius or most reversible slice.
3. Earliest user or business value.

If multiple slices qualify, name the criterion used.

## Clarify Before Slicing

For `CLARIFY BEFORE SLICING`, emit:

- Reason slicing is unsafe.
- Known source facts.
- Missing information table capped at three slice-blocking questions.
- What would unlock slicing.

Do not invent tentative slices. You may list source-backed candidate areas, but not as claimable slices.

## Prior Slice Reconciliation

If prior slice suggestions exist, include:

```markdown
Prior slice reconciliation:
| Prior suggestion | Action | Reason |
| --- | --- | --- |
| ... | kept / refined / dropped / added | ... |
```

Omit this section when there are no prior suggestions.

## Output Format

```markdown
## Work Slice Splitter

Verdict: SLICE | CLARIFY BEFORE SLICING | DO NOT SPLIT

Source facts:
- <facts from request/context that support slicing>

Slice plan:
1. <slice template>

Deferred / needs clarification:
- <unclear remainder, or None>

First safe slice:
- <slice name and criterion used>

Parallelizable slices:
- <slice names, or None>

Blocked slices:
- <slice name, blocker, unlock condition, or None>

Next step:
- Run a ticket-readiness-audit on each slice before claiming or assigning it.
```

For `DO NOT SPLIT`, replace the slice plan with the reason not to split and the single done signal if known.

## Good And Bad Examples

### Vertical Slice

Good:

```markdown
Slice: Upload and preview import file
Outcome: User can upload a file and see parsed rows before anything is saved.
Scope in: file selection, parsing, preview rows, parse errors.
Scope out: saving records, duplicate merge, rollback.
Dependencies/blockers: none known.
Done signal: preview displays parsed rows and row-level parse errors without saving.
Sequence: first safe
```

Bad:

```markdown
Slice: Backend parser
Outcome: Build parsing backend.
```

Bad because it is a component chore, not an independently claimable outcome.

### Busywork

Good:

```markdown
Verdict: DO NOT SPLIT
Reason: copy fix has one changed sentence and one done signal.
Done signal: target copy appears in the intended location.
```

Bad:

```markdown
Slice 1: edit copy. Slice 2: review copy. Slice 3: commit copy.
```

Bad because the slices share one done signal and create busywork below an observable outcome.

### Enabling Slice

Good:

```markdown
Slice: Read accounts from new schema with old behavior preserved
Outcome: Existing account lookup behavior works through the new schema.
Scope in: read path only.
Scope out: writes, migration cleanup, new fields.
Dependencies/blockers: schema draft accepted.
Done signal: existing account lookup returns the same shape from the new schema.
Sequence: first safe, chosen because it reduces migration risk.
```

Bad:

```markdown
Slice: Database work.
```

Bad because it has no observable preserved or enabled behavior.

### First Safe Slice

Good:

```markdown
First safe slice:
- Upload and preview import file, chosen because it is reversible and does not save data.
```

Bad:

```markdown
First safe slice:
- Save imported records.
```

Bad because it chooses a mutating slice before the safer prerequisite preview slice.

### Invented Intent

Good:

```markdown
Verdict: CLARIFY BEFORE SLICING
Reason: "improve onboarding" does not name the user outcome or affected flow.
Missing information: Which onboarding behavior should change?
```

Bad:

```markdown
Slice 1: signup form. Slice 2: welcome email. Slice 3: product tour.
```

Bad because it invents onboarding outcomes from a generic product model rather than source facts.

## Anti-Patterns

- Component slicing: backend/frontend/tests without independent outcomes.
- Activity slicing: design, build, test, review as separate slices without slice outcomes.
- Invented intent: guessing user flows from vague requests.
- Busywork slicing: splitting below a done signal.
- Hidden blockers: marking blocked slices without blocker and unlock condition.
- No first safe slice: listing slices without sequence.
- Proof inflation: turning done signals into a full verification contract.
- Readiness duplication: replacing ticket-readiness-audit with this skill.
- Partial clarity laundering: hiding unclear remainder instead of deferring it.
- Orphaning in-flight work: slicing mid-execution before preserving current state.

## Compact Worked Examples

### SLICE

Verdict: SLICE

Source facts:
- Request asks for import upload, preview validation, duplicate handling, and save.

Slice plan:
1. Slice: upload and preview file; Outcome: user sees parsed rows before save; Scope in: upload, parse, preview; Scope out: duplicate handling and save; Dependencies/blockers: none known; Done signal: parsed rows and parse errors appear without saving; Sequence: first safe.
2. Slice: validate duplicate warnings; Outcome: user sees duplicate warnings before save; Scope in: warning display; Scope out: merge policy; Dependencies/blockers: duplicate rule needed; Done signal: duplicate rows show warning state; Sequence: blocked.
3. Slice: save valid rows; Outcome: valid rows persist after confirmation; Scope in: save valid rows; Scope out: rollback; Dependencies/blockers: preview and validation slices complete; Done signal: valid rows persist and invalid rows do not; Sequence: later.

First safe slice: upload and preview file, chosen because it is reversible and does not mutate data.
Blocked slices: validate duplicate warnings, blocked by duplicate rule decision.
Next step: run a ticket-readiness-audit on each slice before claiming or assigning it.

### CLARIFY BEFORE SLICING

Verdict: CLARIFY BEFORE SLICING

Reason: "make reports better" does not name the report, user, behavior, or outcome.

Known source facts:
- Reports area is in scope.

Missing information:
| Missing item | Question | Unlocks |
| --- | --- | --- |
| Target report | Which report should change? | Slice boundary |
| Desired outcome | What should users be able to do or see? | Slice outcome |
| Done signal | What observable result proves improvement? | Claimable slices |

What would unlock slicing: target report plus desired observable outcome.

### DO NOT SPLIT

Verdict: DO NOT SPLIT

Reason: the change has one claimable outcome and splitting would create activity chores.
Single outcome: update the settings label copy.
Done signal: new copy appears in the settings label.
Next step: keep as one work item.

## Tune When

Tune this skill when outputs:

- Split by component, team, or activity instead of outcome.
- Invent product intent from vague requests.
- Create more than five slices without a clear reason.
- Create slices below an observable done signal.
- Omit first safe slice.
- Mark slices blocked without blocker and unlock condition.
- Turn done signals into a verification contract.
- Duplicate ticket-readiness-audit instead of recommending it as next step.
- Hide unclear remainder in partial-slice scenarios.
- Include bad examples without an explicit `Bad because ...` reason.
