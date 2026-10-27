# Examples

Read this file only when a draft misses the output contract: a required section is missing, a verdict is unsupported, or a field was filled in without a source.

Do not read it when the draft already matches the contract.

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

## Contrasts: Subjective Language

Good:

```markdown
When a signed-in user saves notification settings and the save fails, the previous settings remain unchanged, an inline error is shown near the save control, and the user can retry without refreshing.
```

Bad:

```markdown
Errors are handled well.
```

Bad because it has no condition, actor, error type, observable result, or pass/fail signal.

## Contrasts: Open Questions

Good:

```markdown
Which roles are allowed to edit notification settings?
```

Good because permission behavior cannot be accepted without knowing the allowed roles.
