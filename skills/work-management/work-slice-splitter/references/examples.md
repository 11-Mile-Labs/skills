# Examples

Read this file only when a draft misses the output contract: a required section is missing, a verdict is unsupported, or a field was filled in without a source.

Do not read it when the draft already matches the contract.

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
