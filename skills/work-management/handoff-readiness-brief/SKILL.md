---
name: handoff-readiness-brief
description: "Use when handing off work, pausing a task, summarizing current state for someone to resume, preparing continuation after context loss, or making active work transferable."
argument-hint: "[optional: work state, pause note, transfer request, or continuation context]"
license: MIT
---

# Handoff Readiness Brief

Use this skill to make active work transferable. A handoff brief is not a status update; it is a continuation aid for the next worker.

The goal is to capture current state, resumption surface, evidence state, and the safest next action without hiding failures or inventing certainty.

## When To Use

Use this skill when asked to hand off work, pause a task, summarize current work state for someone else to resume, prepare continuation after context loss, transfer work to a reviewer or implementer, or make an in-progress task safe to pick up later.

Do not use this skill to:

- Decide whether a ticket or work item is claimable before work starts. Use a ticket-readiness-audit workflow for that.
- Define the evidence required before claiming done. Use a verification-contract workflow for that.
- Map likely regression surfaces. Use a regression-risk-map workflow for that.
- Produce a changelog, release note, or generic progress update.

This skill teaches continuation. If the output would not help a competent next worker resume safely, it is not a handoff brief.

If a prior handoff exists for the same work, update or reconcile it instead of producing a competing brief.

## The Handoff Rule

A handoff is READY only when a competent next worker can resume without guessing current state, overwriting protected work, trusting false evidence, or wondering what to do first.

If any of those four risks remain, do not mark the handoff READY.

## Verdicts And Precedence

Use exactly one verdict, applying this precedence:

1. **INCOMPLETE:** the work is not at a coherent pause point. The state is mid-change, broken, divergent, or insufficiently described, so even the handoff cannot be trusted yet.
2. **BLOCKED:** the state is coherent, but an external dependency, access need, decision, or owner response prevents continuation.
3. **NEEDS REFRESH:** the state is coherent and not externally blocked, but freshness or verification is stale, expired, not current, or not proven against the latest known state.
4. **READY:** no higher verdict applies and all four gates pass.

For every verdict below INCOMPLETE, include a `Ruled out:` line that names only higher-precedence verdicts and why they do not apply.

Do not include a `Ruled out:` line for INCOMPLETE.

Never mark READY while known failures, blockers, hidden skipped verification, or protected work risks remain unresolved.

## The Four Gates

Every handoff brief must satisfy these four gates.

| Gate | Required discipline |
| --- | --- |
| Current state | State what is true now. Any decaying state requires `Verified at: <timestamp/source>` or `Verify before acting: <specific recheck>`. Non-decaying state may include `Stable because: <reason>` when useful. |
| Resumption surface | Name changed files, artifacts, or work surfaces. Protected entries must name what is protected, why it is protected, and how to verify it is still present. A bare "do not touch this path" is not enough. |
| Evidence state | Name the command or method, result, and source or excerpt. "Tested it" and "all green" are invalid without source-backed evidence. |
| Next action | Name the smallest state-preserving first step. Default to refresh or verification before mutation. If the first step is mutating or irreversible, label it and justify why it is safe or necessary. |

If information is missing, mark it as an assumption or unknown. Do not fill gaps with plausible narrative and call the handoff READY.

## Suspect Phrases

These phrases often hide missing evidence or stale state:

- should be fine
- mostly working
- wrapping up
- looks good
- tested it
- all green
- ready to ship
- just need to
- nothing major left

Use them only after converting them into source-backed current state, evidence, or next action. Otherwise rewrite them or mark the underlying point unresolved.

Good:

- "`project test` exited 0 at 2026-05-18 14:20 local time; the auth and billing tests passed."

Bad:

- "Tests are all green." Bad because it uses a suspect phrase without the command, result, time, or source needed to re-check the claim.

Tiny handoffs should stay compact. A one-file docs pause may need four short bullets, not a full ceremony.

Bad:

- "For a typo fix, render all audience blocks, risk sections, and a long decision history." Bad because overbloat hides the continuation signal for a tiny reversible change.

## Audience

Declare exactly one audience:

- `same-agent continuation`: use when the same worker or a replacement with the same task context will resume. Emphasize exact commands, transient state, what was tried, and what was rejected.
- `human handoff`: use when another person or general implementer will pick up the work. Emphasize decisions, rationale, blockers, owners if supplied, and acceptance focus.
- `reviewer handoff`: use when the next worker is reviewing or approving. Emphasize changed artifacts, verification state, residual risks, and review focus.

If the audience is not stated, default to `human handoff (defaulted)`. Do not block the handoff to ask which audience applies.

Render only the audience-specific block matching the declared audience. Do not render all three.

## Output Format

Produce the brief inline unless the surrounding workflow already has a durable home such as a work item, review description, plan, release checklist, or project document.

```markdown
## Handoff Readiness Brief

Verdict: INCOMPLETE | BLOCKED | NEEDS REFRESH | READY
Ruled out: <higher-precedence verdicts ruled out and why; omit when verdict is INCOMPLETE>

Audience: same-agent continuation | human handoff [(defaulted if inferred)] | reviewer handoff

Goal:
- <work in flight>

Current state:
- Status: <one-line current state>
- Branch / artifact: <if relevant>
- Verified at: <timestamp, source, commit, or other stable reference>
- Verify before acting: <specific recheck for decaying state>
- Stable because: <optional reason for non-decaying state>

Resumption surface:
- Changed files / artifacts:
  - <path, object, or artifact>
- Protected (do not overwrite):
  - <path or area> - protected because <reason>; verify with <check>

Evidence state:
| Check | Command or method | Result | Source / excerpt |
| --- | --- | --- | --- |
| <check> | <command or method> | passed / failed / not run | <output, error excerpt, file:line, or n/a> |

Known failures or blockers:
- <failure or blocker, with source or "None known from supplied state">

Open assumptions / unknowns:
- <assumption with rationale, or unknown that affects continuation>

Audience-specific:
- Already tried: <same-agent continuation only>
- Decision rationale: <human handoff only>
- Review focus: <reviewer handoff only>

Non-omission note:
- <only when the request asks to hide failures, blockers, skipped verification, or protected work>

Next action:
- First action: <smallest state-preserving step; if mutating or irreversible, label and justify>
- Stale-state refresh needed before acting: <yes/no plus what>

What not to do:
- <specific pitfalls, paths, or actions>
```

The evidence table is always present. If no checks ran, use this row:

```markdown
| No checks run | not run | verification skipped because <reason> | n/a |
```

Known failures or blockers is always present. If none are known, write `None known from supplied state`.

Render exactly one audience-specific line. Remove the other two. Include the non-omission note only when the request asks to hide failures, blockers, skipped verification, or protected work.

If the brief should persist, place it where the next worker will look first in the user's existing workflow. Do not prescribe a tool, tracker, path, or storage system. If there is no durable home, keep it inline and copy-ready.

## Good And Bad Examples

### Current State

Good:

```markdown
Current state:
- Status: parser accepts quoted values, but escape handling is not finished.
- Verified at: local test run at 2026-05-18 13:40; commit abc1234.
- Verify before acting: rerun the parser tests after syncing because upstream may have moved.
```

Bad:

```markdown
Current state:
- Mostly working.
```

Bad because it uses a suspect phrase and gives no source, freshness marker, or unfinished boundary.

### Resumption Surface

Good:

```markdown
Resumption surface:
- Changed files / artifacts:
  - src/import/parser.ts
  - tests/import/parser.test.ts
- Protected (do not overwrite):
  - config/local-settings.json - protected because it contains uncommitted user edits; verify with git status before rewriting config files.
```

Bad:

```markdown
Protected:
- config/local-settings.json
```

Bad because it names a path but not why it is protected or how the next worker should verify the risk.

### Evidence State

Good:

```markdown
| Check | Command or method | Result | Source / excerpt |
| --- | --- | --- | --- |
| Parser unit tests | project test tests/import/parser.test.ts | failed | "expected escaped quote to survive parse" |
```

Bad:

```markdown
Evidence: tested it.
```

Bad because it states an activity without command, result, source, or pass/fail detail.

### Next Action

Good:

```markdown
Next action:
- First action: rerun `project test tests/import/parser.test.ts` to confirm the escaped-quote failure still reproduces.
- Stale-state refresh needed before acting: yes, sync branch and re-run the focused test before editing.
```

Bad:

```markdown
Next action:
- Keep going on the feature.
```

Bad because it does not name a specific first step, preserve state, or protect against stale evidence.

### Concealment

Good:

```markdown
Non-omission note:
- I cannot produce a READY handoff while hiding a failed check; including it as a known failure.
```

Bad:

```markdown
Verdict: READY
Known failures or blockers:
- None.
```

Bad because it hides a requested-to-be-omitted failure and creates a false READY claim.

## Anti-Patterns

- **False-ready claim:** marks READY while hiding failures, blockers, or skipped verification.
- **Concealment compliance:** obeys a request to omit known failures, protected work, skipped checks, or blockers.
- **Stale-state assumption:** treats last-known-good state as current without a freshness mark or recheck.
- **Protected-area omission:** fails to flag uncommitted local work, or flags it without what, why, and verify-with.
- **Vague next action:** says "continue the feature" instead of naming the first command, check, or state-preserving step.
- **Decision laundering:** gives a human handoff outcome without the rationale needed to continue safely.
- **Compaction loss:** drops failing output, blocker text, or exact errors because the handoff is replacing longer context.
- **Status-update drift:** becomes a progress summary with no continuation signal and no four-gate structure.
- **Overbloat:** gives tiny reversible work full ceremony or renders all audience-specific blocks.
- **Audience mismatch:** declares one audience but renders the wrong audience-specific block.

## Compact Worked Examples

### READY Same-Agent Continuation

Verdict: READY
Ruled out: INCOMPLETE because the change is at a coherent pause point; BLOCKED because no external dependency is known; NEEDS REFRESH because checks ran on the current state.

Audience: same-agent continuation

Current state: settings save fix implemented on the current branch; verified at local test run 2026-05-18 11:10.

Resumption surface: changed `settings-form` and focused tests. Protected: local sample data file - protected because it has user edits; verify with status before touching data fixtures.

Evidence state:
| Check | Command or method | Result | Source / excerpt |
| --- | --- | --- | --- |
| Focused save tests | project test settings-save | passed | exit 0 |
| Formatting | project format-check | passed | exit 0 |

Known failures or blockers: None known from supplied state.
Audience-specific: Already tried the broader form refactor and rejected it because it touched unrelated validation.
Next action: run the full relevant test group before final claim.

### NEEDS REFRESH Human Handoff

Verdict: NEEDS REFRESH
Ruled out: INCOMPLETE because the branch is at a coherent pause point; BLOCKED because no external dependency is known.

Audience: human handoff

Current state: import preview change appears complete, but verification is from yesterday. Verify before acting: sync current branch and rerun focused import checks.

Resumption surface: changed import preview view and parser tests. Protected: local fixture file - protected because it contains exploratory user edits; verify with status before rewriting.

Evidence state:
| Check | Command or method | Result | Source / excerpt |
| --- | --- | --- | --- |
| Import preview tests | project test import-preview | passed | exit 0 yesterday |
| Current verification | not run | not run | needs refresh on current state |

Known failures or blockers: None known from supplied state.
Audience-specific: Decision rationale is to keep validation messages in preview only; save behavior is out of scope.
Next action: refresh branch, rerun focused checks, then update verdict.

### BLOCKED

Verdict: BLOCKED
Ruled out: INCOMPLETE because the current state is coherent.

Audience: human handoff (defaulted)

Current state: export mapping code is ready to connect, but the required field mapping decision is missing. Verified at: design note reviewed 2026-05-18.

Resumption surface: changed export mapping draft and tests. Protected: generated sample export - protected because it is comparison evidence; verify with status before regenerating.

Evidence state:
| Check | Command or method | Result | Source / excerpt |
| --- | --- | --- | --- |
| Export mapping tests | project test export-mapping | not run | blocked by missing field decision |

Known failures or blockers: field mapping owner decision required before continuing.
Audience-specific: Decision rationale is to avoid inventing column semantics.
Next action: ask for the field mapping decision before editing.

### INCOMPLETE

Verdict: INCOMPLETE

Audience: reviewer handoff

Current state: navigation refactor is mid-change; two files are partially updated and the route table no longer compiles. Verify before acting: inspect current diff and identify the intended pause point.

Resumption surface: changed navigation files and route tests. Protected: local layout experiment - protected because it contains user edits; verify with status before overwriting.

Evidence state:
| Check | Command or method | Result | Source / excerpt |
| --- | --- | --- | --- |
| Type check | project typecheck | failed | "RouteConfig is missing required field" |

Known failures or blockers: compile failure above; pause point not reached.
Audience-specific: Review focus is whether the refactor should be completed or rolled back to a coherent slice.
Next action: reach a coherent pause point before assessing BLOCKED, NEEDS REFRESH, or READY.

## Tune When

Tune this skill when outputs:

- Mark READY while known failures, blockers, or skipped checks remain.
- Omit the audience line or fail to default to `human handoff (defaulted)`.
- Render more than one audience-specific block.
- Add a `Ruled out:` line to INCOMPLETE.
- Treat stale or decaying state as current without `Verified at` or `Verify before acting`.
- Use protected paths without what, why, and verify-with.
- Say "tested it," "all green," or similar without source-backed evidence.
- Become generic status updates instead of continuation briefs.
- Produce long ceremony for tiny reversible work.
- Drift into ticket claimability, proof-contract, regression-map, changelog, or release-note work.
