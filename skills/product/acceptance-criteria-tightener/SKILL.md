---
name: acceptance-criteria-tightener
description: "Use when writing, reviewing, or tightening acceptance criteria for a feature, ticket, requirement, QA handoff, or product change; when criteria are vague, subjective, implementation-shaped, too broad, or missing pass/fail boundaries."
argument-hint: "[criteria, requirement, ticket, or feature description]"
license: MIT
---

# Acceptance Criteria Tightener

## Overview

Use this skill to turn vague acceptance criteria into product-observable pass/fail statements before implementation, QA, or acceptance review.

The goal is not to write every possible test. The goal is to make acceptance clear enough that an independent reviewer can decide whether the behavior passes without guessing intent.

## When To Use

Use this when the artifact needing work is acceptance criteria or acceptance-like requirements.

Good triggers:

- "Tighten these acceptance criteria."
- "Make this ticket testable."
- "Are these criteria objective?"
- "Rewrite these vague requirements into pass/fail criteria."
- "What acceptance criteria are missing?"

Do not use this to:

- Decide whether a work item is claimable.
- Define proof or evidence for a done claim.
- Decide what should be v1 or v2.
- Map regression surfaces.
- Generate exhaustive QA test scripts.
- Find root cause or implement the feature.

## The Tightening Rule

A criterion is tight when an independent reviewer can decide pass/fail without reading the implementer's mind: actor, condition, action, and observable result are all present.

## Level Discipline

Use exactly one level per criterion:

- `MUST`: required for this acceptance pass. This is the default.
- `SHOULD`: useful but not required for acceptance.
- `OUT OF SCOPE`: explicitly not part of this acceptance pass.

`SHOULD` requires both phrases inside the criterion:

- `Why-not-MUST: <rationale>`
- `Acceptance passes if not met.`

Demoting `MUST` to `SHOULD` is invalid unless the source or owner explicitly changes acceptance scope and the rationale is recorded.

`OUT OF SCOPE` means the behavior is not part of acceptance and should not be implied by implementation.

## Criterion Grammar

Every criterion must include:

- Actor or system subject.
- Condition or context.
- Action, event, or state transition.
- Observable expected result.

Criterion table values:

- Level column values are exactly `MUST`, `SHOULD`, or `OUT OF SCOPE`.
- Type column values are exactly `positive`, `negative`, or `boundary`.

After drafting criteria, self-check each row:

```markdown
- Actor/subject present?
- Condition/context present?
- Action/event/state transition present?
- Observable expected result present?
- Pass/fail decidable without implementation knowledge?
```

## Subjective Language

Suspect words and phrases must be rewritten with an observable signal or marked unresolved:

- works well
- intuitive
- fast
- user-friendly
- good
- smooth
- handles errors
- supports mobile
- scalable
- robust
- easy
- appropriate
- correct

These words are allowed only when paired with an observable signal or threshold supplied by the source or owner. Do not invent thresholds, sample sizes, time limits, viewport widths, or performance numbers. If the threshold is missing and needed for pass/fail, mark it unresolved or ask an acceptance-blocking question.

Good:

```markdown
When a signed-in user saves notification settings and the save fails, the previous settings remain unchanged, an inline error is shown near the save control, and the user can retry without refreshing.
```

Bad:

```markdown
Errors are handled well.
```

Bad because it has no condition, actor, error type, observable result, or pass/fail signal.

## Edge Categories

Consider these categories:

- Empty state
- Invalid input
- Permissions/access
- Existing data/backward compatibility
- Mobile/responsive
- Error recovery
- Accessibility/content clarity
- External/side-effect behavior

Use the full edge table by default. Compact mode is required for clearly tiny or isolated changes, such as copy-only criteria, to avoid edge-case bloat.

Compact mode must not render all categories as rows. It must include only applicable or unknown categories, plus:

```markdown
Other edge categories considered, not applicable: <list with source-backed reasons>
```

Rules:

- `not applicable` requires a source-backed reason, not "seems unrelated."
- `unknown` triggers an acceptance-blocking question only if it prevents writing pass/fail criteria.
- Do not expand every category into criteria. Only applicable or acceptance-blocking unknown categories need criteria or questions.

## Open Questions

Ask acceptance-blocking questions only.

A question is acceptance-blocking only when the answer is required to write or validate a `MUST` criterion.

Bad:

```markdown
Should the design feel polished?
```

Bad because it is broad product curiosity, not a blocker to a pass/fail criterion.

Good:

```markdown
Which roles are allowed to edit notification settings?
```

Good because permission behavior cannot be accepted without knowing the allowed roles.

## Output Format

```markdown
## Acceptance Criteria Tightener

Source: <requirement, ticket, note, or description>

Intended outcome:
- <actor + object + outcome from source>

Edge categories considered:
Use the full table or the compact block, not both.

| Category | Status | Reason | Criteria needed |
| --- | --- | --- | --- |
| Empty state | applicable / not applicable / unknown | <source-backed reason> | yes / no |
| Invalid input | ... | ... | ... |
| Permissions/access | ... | ... | ... |
| Existing data/backward compatibility | ... | ... | ... |
| Mobile/responsive | ... | ... | ... |
| Error recovery | ... | ... | ... |
| Accessibility/content clarity | ... | ... | ... |
| External/side-effect behavior | ... | ... | ... |

Compact edge categories considered:
- Applicable or unknown: <category> because <reason>.
- Other edge categories considered, not applicable: <list with source-backed reasons>.

Tightened criteria:
| ID | Level | Type | Criterion |
| --- | --- | --- | --- |
| AC-1 | MUST | positive | When <actor> <condition>, <action/event>; <observable result>. |
| AC-2 | MUST | negative | When <condition>, <forbidden behavior> does not occur; <observable signal>. |
| AC-3 | SHOULD | positive | <criterion>. Why-not-MUST: <rationale>. Acceptance passes if not met. |

Rewritten or rejected vague criteria:
| Original | Reason | Action |
| --- | --- | --- |
| "<original phrase>" | <why vague/subjective/implementation-shaped> | rewritten as AC-N / moved to notes / OUT OF SCOPE / unresolved |

Acceptance-blocking questions:
- <question required to write a MUST criterion>

Ready-to-use checklist:
- [ ] AC-1 ...
- [ ] AC-2 ...
```

The checklist mirrors `MUST` criteria; do not edit it independently.

If criteria should be persisted, use the user's existing durable location. Do not prescribe a tracker, folder, or file format.

## Good And Bad Examples

### Criterion Grammar

Good:

```markdown
| AC-1 | MUST | positive | When a signed-in user opens notification settings, changes email alerts from off to on, and saves, the setting persists and the page shows a saved confirmation without changing unrelated notification settings. |
```

Bad:

```markdown
| AC-1 | MUST | positive | User can manage settings. |
```

Bad because it lacks condition, specific action, affected setting, and observable result.

### Level Demotion

Good:

```markdown
| AC-4 | SHOULD | positive | When a signed-in user saves settings successfully, the confirmation message disappears after a short delay. Why-not-MUST: success is already accepted by persisted settings and visible confirmation. Acceptance passes if not met. |
```

Bad:

```markdown
| AC-4 | SHOULD | positive | Settings save successfully. |
```

Bad because a real acceptance requirement was demoted to `SHOULD` without a why-not-MUST rationale or acceptance-state note.

### Implementation Leak

Good:

```markdown
| AC-2 | MUST | negative | When the save fails, the user's previous settings remain unchanged and an error message appears near the save control. |
```

Bad:

```markdown
| AC-2 | MUST | positive | The modal calls the save endpoint and writes to the settings table. |
```

Bad because implementation details replaced user-observable acceptance behavior.

### Subjective Rewrite

Good:

```markdown
| AC-3 | MUST | boundary | On the owner-specified minimum supported viewport, the notification settings labels, controls, and save action remain visible without horizontal scrolling. |
```

Bad:

```markdown
| AC-3 | MUST | boundary | The mobile experience is smooth and user-friendly. |
```

Bad because subjective words are not paired with an observable signal or owner-supplied threshold.

## Anti-Patterns

- Invented requirements: filling criteria not supported by the source.
- Subjective laundering: rewriting "works well" as "works correctly" while still subjective.
- Implementation leak: leaving internals such as modal, endpoint, or storage details in acceptance criteria.
- Edge-case bloat: expanding all edge categories on a tiny isolated change.
- `MUST` to `SHOULD` demotion: moving a real requirement to `SHOULD` to look done.
- Hidden `MUST`: framing a real requirement as `SHOULD` without `Why-not-MUST`.
- Curiosity questions: adding non-blocking product questions.
- Premature splitting: invoking split before tightening one item's criteria.
- Subjective survivor: leaving a suspect word without an observable signal.

## Compact Worked Examples

### Tiny Isolated Change

Source:
- Change button copy from "Save" to "Save settings."

Edge categories considered:
- Applicable: accessibility/content clarity, because visible copy changed.
- Other edge categories considered, not applicable: empty, invalid input, permissions, existing data, mobile, error recovery, external side effects; source is copy-only.

Tightened criteria:
| ID | Level | Type | Criterion |
| --- | --- | --- | --- |
| AC-1 | MUST | positive | When a user views the settings form, the primary save action label reads "Save settings." |
| AC-2 | MUST | boundary | When a user navigates by accessible name, the save action is announced as "Save settings." |

### Standard Feature

Source:
- User can manage notification settings, works on mobile, handles errors.

Edge categories considered:
| Category | Status | Reason | Criteria needed |
| --- | --- | --- | --- |
| Empty state | not applicable | Existing settings screen already has settings to edit | no |
| Invalid input | not applicable | Notification toggles do not accept free-form input | no |
| Permissions/access | unknown | Allowed roles not stated | yes |
| Existing data/backward compatibility | applicable | Existing notification settings must not be reset | yes |
| Mobile/responsive | applicable | Source explicitly says mobile | yes |
| Error recovery | applicable | Source says handles errors | yes |
| Accessibility/content clarity | applicable | Settings controls need clear labels | yes |
| External/side-effect behavior | applicable | Notifications may affect outgoing messages | yes |

Tightened criteria:
| ID | Level | Type | Criterion |
| --- | --- | --- | --- |
| AC-1 | MUST | positive | When an allowed signed-in user toggles email notifications and saves, the new value persists after refresh and unrelated notification settings remain unchanged. |
| AC-2 | MUST | negative | When saving fails, the previous settings remain unchanged, an inline error is shown, and the user can retry without refreshing. |
| AC-3 | MUST | boundary | On the owner-specified minimum supported viewport, notification labels, toggles, and save action remain visible without horizontal scrolling. |

Acceptance-blocking questions:
- Which roles are allowed to edit notification settings?
- Which outgoing notification side effects must change when a setting is toggled?
- What is the minimum supported viewport for this acceptance pass?

### Overbroad Criteria

Source:
- Users can manage all account settings, billing, team members, notifications, privacy, and integrations.

Tightened result:
- This is too broad for one acceptance pass. Split by acceptance object before final criteria: account profile, billing, team members, notifications, privacy, integrations.

Acceptance-blocking questions:
- Which one settings area is in this acceptance pass?
- Which actor is allowed to manage that area?
- What is the observable success state for that area?

## Tune When

Tune this skill when agents:

- Leave subjective language in tightened criteria.
- Write criteria without actor, condition, action, or observable result.
- Demote `MUST` to `SHOULD` without rationale and acceptance-state note.
- Leave implementation details in the Criterion column.
- Expand every edge category for tiny changes.
- Ask curiosity questions instead of acceptance-blocking questions.
- Generate test scripts instead of acceptance criteria.
- Invent requirements not supported by the source.
