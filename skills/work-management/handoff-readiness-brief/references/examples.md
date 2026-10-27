# Examples

Read this file only when a draft misses the output contract: a required section is missing, a verdict is unsupported, or a field was filled in without a source.

Do not read it when the draft already matches the contract.

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

## Contrasts: Suspect Phrases

Good:

- "`project test` exited 0 at 2026-05-18 14:20 local time; the auth and billing tests passed."

Bad:

- "Tests are all green." Bad because it uses a suspect phrase without the command, result, time, or source needed to re-check the claim.

Tiny handoffs should stay compact. A one-file docs pause may need four short bullets, not a full ceremony.

Bad:

- "For a typo fix, render all audience blocks, risk sections, and a long decision history." Bad because overbloat hides the continuation signal for a tiny reversible change.
