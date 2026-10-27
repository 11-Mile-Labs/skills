# Examples

Read this file only when a draft misses the output contract: a required section is missing, a verdict is unsupported, or a field was filled in without a source.

Do not read it when the draft already matches the contract.

## Good And Bad Examples

### Status And Source

Good:

```markdown
Status: DECIDED
Decision: Use server-rendered public pages for launch.
Source/date: current conversation; product owner stated this is the launch decision; exact date is today.
```

Bad:

```markdown
Status: DECIDED
Decision: Use server-rendered public pages forever.
Source/date: approved.
```

Bad because it invents permanence and hides who approved the decision or when.

### Options

Good:

```markdown
Options considered:
- Static pages: rejected because content must reflect account-specific availability.
- Server-rendered pages: chosen because public content must stay current.
```

Bad:

```markdown
Options considered:
- Static pages, server-rendered pages, and a mobile app.
```

Bad because it invents options not stated in the decision source.

### Scope

Good:

```markdown
Scope:
- In: public marketing pages for launch.
- Out: authenticated account dashboard pages.
```

Bad:

```markdown
Scope: all frontend pages.
```

Bad because it broadens a launch-page decision into unrelated product areas.

### Deferral

Good:

```markdown
Status: DEFERRED
Decision: Defer choosing the enterprise export format until two customer examples are collected.
Revisit trigger: two real examples are available.
```

Bad:

```markdown
Status: NEEDS DECISION
Decision: export format later.
```

Bad because it loses the explicit deferral trigger and makes an intentional wait look like an unresolved blocker.

### Empty Context

Good:

```markdown
Status: NEEDS DECISION
Decision: choose whether import preview should validate duplicates before save.
Options considered: validate in preview; validate only on save.
Unknowns / follow-ups: Which user-facing moment should show duplicate warnings?
```

Bad:

```markdown
Status: DECIDED
Decision: validate duplicates in preview.
```

Bad because it fabricates a final choice from unresolved tradeoff discussion.

## Anti-Patterns

- **Finality laundering:** marks a proposal or vague assertion as `DECIDED`.
- **Source invention:** creates owner, decider, date, or approval that was not supplied.
- **Option invention:** lists plausible options that were not actually considered.
- **Scope creep:** expands a local decision into a broad rule.
- **Stale voice:** writes "we decided" without date or source.
- **Owner/decider collapse:** merges the person who decided with the person who owns follow-through.
- **Deferral erasure:** treats an intentional wait as a generic missing decision.
- **Supersession loss:** records an old decision without noting it was replaced.
- **Heavyweight drift:** turns a lightweight snapshot into a full PRD or architecture record.
- **Empty-context fabrication:** converts discussion into a decision when no choice exists.

## Compact Worked Examples

### DECIDED

Status: DECIDED
Decision: Use a single shared import preview component for CSV and spreadsheet imports in the first release.
Source/date: current conversation; owner asserted this as the current decision; exact date unknown.
Rationale: shared validation copy should stay consistent across both import paths.
Scope in: first-release import preview surfaces.
Scope out: save behavior, duplicate merge strategy, and future import formats.
Revisit trigger: a third import format requires different preview behavior.

### DEFERRED

Status: DEFERRED
Decision: Defer the enterprise export format decision until two real customer examples are collected.
Source/date: current conversation.
Rationale: choosing now would guess at field names and file shape.
Scope in: enterprise export format only.
Scope out: standard export behavior.
Revisit trigger: two real examples are available.

### NEEDS DECISION

Status: NEEDS DECISION
Decision: choose where duplicate import warnings appear.
Source/date: current tradeoff discussion; no choice stated.
Rationale: preview warnings help users before save, while save-time warnings may be simpler to implement.
Scope in: duplicate warning timing.
Scope out: duplicate merge policy.
Revisit trigger: decision owner chooses preview-time or save-time warnings.

### SUPERSEDED

Status: SUPERSEDED
Decision: Prior decision to use separate import preview components has been replaced by the shared-component decision.
Source/date: current conversation; prior decision date unknown.
Rationale: shared copy and validation behavior now matter more than component separation.
Scope in: first-release import preview implementation.
Scope out: unrelated import processing internals.
Revisit trigger: none stated.

## Contrasts: Source Discipline

Good:

- `Source/date: current conversation; user asserted this was decided yesterday; exact date unknown.`

Bad:

- `Source/date: approved on 2026-05-17.` Bad because it invents an exact approval date not present in the source.

Never invent:

- Decider
- Owner
- Exact date
- Source
- Options considered
- Rejected options
- Scope
- Reversibility
- Supersession relationship

If options were not stated, write `Options considered: not stated`. Do not force enumeration and do not invent plausible alternatives.

## Contrasts: Stale-Resistant Voice

Good:

- `Use a relational database for primary storage in the initial release (source: current owner assertion; decided yesterday; exact date unknown).`
- `Do not add offline mode in this release.`
- `Proposed: use server-rendered public pages for launch.`

Bad:

- `We decided to keep it simple.` Bad because it is not self-contained, has no scope, and will read as current even when copied later.

## Contrasts: Scope Discipline

Good:

- `Scope in: import preview validation messages. Scope out: save behavior and duplicate merge strategy.`

Bad:

- `This means all future import behavior should follow this pattern.` Bad because it expands one decision into an unbounded rule without source.
