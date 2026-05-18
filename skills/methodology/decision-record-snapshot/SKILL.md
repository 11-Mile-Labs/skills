---
name: decision-record-snapshot
description: "Use when capturing, summarizing, clarifying, or making transferable a product, design, technical, scope, or work-management decision and its rationale."
argument-hint: "[optional: decision context, tradeoff discussion, or decision request]"
license: MIT
---

# Decision Record Snapshot

Use this skill to capture a lightweight, transferable decision record. The goal is to preserve what was decided, why, where it applies, and when to revisit it without turning the work into a full specification.

A decision snapshot must resist future drift: a later reader should know whether the decision is active, proposed, deferred, superseded, or still missing.

## When To Use

Use this skill when a decision has just been made, when a decision needs to be summarized, when a work item depends on a choice, when someone asks to capture a decision, or when prior tradeoff context needs to be made explicit for future workers.

Use it in two modes:

- **Capture now:** the primary mode, used immediately after a decision is made or proposed.
- **Summarize past context:** used when prior discussion contains decision material. Preserve source, date, and staleness gaps instead of smoothing them over.

Do not use this skill to:

- Write a full PRD, design document, or architecture record.
- Decide whether a work item is claimable.
- Define proof of done or completion evidence.
- Stress-test a decision before it is made.
- Produce a changelog or release note.

If there is only tradeoff discussion and no actual choice, produce a `NEEDS DECISION` snapshot. Do not fabricate a `DECIDED` status from discussion.

## The Snapshot Rule

A decision snapshot is valid only when it preserves the decision status, source, rationale, scope, and revisit trigger without inventing owner, date, options, or finality.

If any of those fields are missing, mark them as unknown or not stated rather than filling them from plausible context.

## Statuses

Use exactly one status.

| Status | Use When |
| --- | --- |
| `DECIDED` | A real choice is asserted or source-backed, including a choice not to do something. |
| `PROPOSED` | A preferred option exists, but it has not been accepted as the decision. |
| `NEEDS DECISION` | Tradeoffs or options are known, but no valid choice exists and work cannot safely proceed without one. |
| `DEFERRED` | There is an explicit choice not to decide or act until a named trigger, date, or condition. |
| `SUPERSEDED` | A prior decision has been replaced by a newer decision. |

Do not add a `REJECTED` status. A decision not to do something is `DECIDED` with a negative decision statement, such as "Do not add offline mode in this release."

Do not mark `DECIDED` merely because the user says to "make it final." A real decision assertion, source, or acceptance signal must exist.

## Source Discipline

Trust current human assertions as source material, but record them honestly.

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

## Stale-Resistant Voice

Write decision statements as self-contained active statements with date or source nearby. Avoid "we decided..." because it goes stale when copied.

Good:

- `Use a relational database for primary storage in the initial release (source: current owner assertion; decided yesterday; exact date unknown).`
- `Do not add offline mode in this release.`
- `Proposed: use server-rendered public pages for launch.`

Bad:

- `We decided to keep it simple.` Bad because it is not self-contained, has no scope, and will read as current even when copied later.

## Scope Discipline

One snapshot should capture one coherent decision.

Always name:

- **Scope in:** where the decision applies.
- **Scope out:** what it does not imply.

If a follow-up prompt broadens the decision beyond the source, do not silently fold it in. Record the broader claim only when the user explicitly says it was part of the original decision and it still remains one coherent choice. Otherwise name it as a separate decision needed.

Good:

- `Scope in: import preview validation messages. Scope out: save behavior and duplicate merge strategy.`

Bad:

- `This means all future import behavior should follow this pattern.` Bad because it expands one decision into an unbounded rule without source.

## Reversibility

Use one reversibility tier:

- `easy to reverse`
- `hard to reverse`
- `one-way door`
- `unknown/not assessed`

If reversibility is missing, record `unknown/not assessed`. Do not block `DECIDED` solely because reversibility is unknown, but add a follow-up when the choice appears high-impact or hard to reverse.

## Decider And Owner

Keep these fields separate:

- **Decider:** who made or approves the choice.
- **Owner:** who is accountable for the affected work going forward.

Both may be unknown. Do not merge them and do not invent either one.

## Output Format

Choose the smallest template that preserves the decision safely.

Use the compact snapshot for tiny, low-risk, current decisions. Use the full snapshot for consequential, cross-team, stale, disputed, hard-to-reverse, or superseding decisions.

### Compact Snapshot

```markdown
## Decision Record Snapshot

Status: DECIDED | PROPOSED | NEEDS DECISION | DEFERRED | SUPERSEDED
Decision: <self-contained active statement, or needed decision>
Source/date: <source and date, or unknown/not stated>
Rationale: <why this choice, or not stated>
Scope in: <where it applies>
Scope out: <what it does not imply>
Revisit trigger: <date, condition, owner request, metric, new information, or not stated>
```

### Full Snapshot

```markdown
## Decision Record Snapshot

Status: DECIDED | PROPOSED | NEEDS DECISION | DEFERRED | SUPERSEDED

Decision:
- <self-contained active statement, or needed decision>

Source/date:
- <source and date, or unknown/not stated>

Context/problem:
- <why a decision was needed>

Options considered:
- <option, or "not stated">

Rejected options:
- <option and reason, or "not stated">

Rationale and tradeoffs:
- <why this choice, what it gives up>

Scope:
- In: <where it applies>
- Out: <what it does not imply>

Decider:
- <person/role, or unknown>

Owner:
- <person/role, or unknown>

Reversibility:
- easy to reverse | hard to reverse | one-way door | unknown/not assessed

Affected work:
- <work items, plans, docs, reviews, implementation areas, or not stated>

Unknowns / follow-ups:
- <decision-blocking or application-blocking unknowns>

Revisit trigger:
- <date, condition, owner request, metric, new information, or not stated>

Supersession:
- Supersedes: <prior decision, or none/not stated>
- Superseded by: <newer decision, or none/not stated>
```

When the status is `NEEDS DECISION`, include the options or tensions surfaced so far and the smallest decision-blocking question. When the status is `SUPERSEDED`, identify the newer decision or mark it unknown.

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

## Tune When

Tune this skill when outputs:

- Mark `DECIDED` without a real source, assertion, or acceptance signal.
- Invent options, owner, decider, date, source, scope, or reversibility.
- Omit scope in/out.
- Use stale "we decided" phrasing without source/date.
- Collapse decider and owner.
- Treat deferred decisions as generic unresolved blockers.
- Fold scope creep into the same snapshot.
- Turn lightweight decisions into full specifications.
- Fail to produce `NEEDS DECISION` when only tradeoff discussion exists.
- Include bad examples without an explicit `Bad because ...` reason.
