# 11 Mile Labs Skills

Curated multi-harness skills from 11 Mile Labs.

This repository collects generic AI coding craft: small reusable skills that work across harnesses. It is intentionally not an installer, not a plugin, and not a vertical business pack.

## Install

```bash
npx skills add 11-Mile-Labs/skills
```

The installer is maintained by the skills ecosystem. This repo only ships content.

## What's Inside

These are standard `SKILL.md` files intended to work in Claude Code, OpenAI Codex, Gemini CLI, and other skills-compatible harnesses.

## Engineering Skills

| Skill | Purpose |
| --- | --- |
| [`dependency-upgrade-risk-check`](./skills/engineering/dependency-upgrade-risk-check/SKILL.md) | Classify dependency upgrade risk and define proof before updating. |
| [`deprecation-plan-brief`](./skills/engineering/deprecation-plan-brief/SKILL.md) | Plan safe removals with compatibility, notice, proof, and rollback. |
| [`implementation-drift-check`](./skills/engineering/implementation-drift-check/SKILL.md) | Check changed artifacts against stated intent to catch scope drift and unrelated edits. |
| [`regression-risk-map`](./skills/engineering/regression-risk-map/SKILL.md) | Map likely regressions and targeted checks for a specific change. |
| [`release-readiness-brief`](./skills/engineering/release-readiness-brief/SKILL.md) | Decide whether a change is ready to release, needs evidence, or should be held. |
| [`scope-check`](./skills/engineering/scope-check/SKILL.md) | Audit scope creep, hidden complexity, and v1/v2 boundaries. |

## QA Skills

| Skill | Purpose |
| --- | --- |
| [`bug-repro-brief`](./skills/qa/bug-repro-brief/SKILL.md) | Turn messy bug reports into reproducible bug briefs before debugging or ticketing. |
| [`bugfix-retrospective-note`](./skills/qa/bugfix-retrospective-note/SKILL.md) | Capture bugfix cause, proof, prevention, and follow-up without ceremony. |
| [`test-failure-signal-brief`](./skills/qa/test-failure-signal-brief/SKILL.md) | Classify failing test and check output before debugging, fixing, or rewriting assertions. |
| [`test-strategy-slicer`](./skills/qa/test-strategy-slicer/SKILL.md) | Choose focused proof layers for a change instead of defaulting to coverage theater. |
| [`verification-contract`](./skills/qa/verification-contract/SKILL.md) | Define the evidence required before claiming a feature, ticket, or plan is done. |

## Product Skills

| Skill | Purpose |
| --- | --- |
| [`acceptance-criteria-tightener`](./skills/product/acceptance-criteria-tightener/SKILL.md) | Rewrite vague acceptance criteria into observable pass/fail statements. |
| [`decision-record-snapshot`](./skills/product/decision-record-snapshot/SKILL.md) | Capture lightweight decision records with source, rationale, scope, and revisit triggers. |
| [`devil-advocate`](./skills/product/devil-advocate/SKILL.md) | Argue against a plan in good faith before committing. |
| [`explain-like-5`](./skills/product/explain-like-5/SKILL.md) | Force a simple explanation of a concept, architecture, or decision. |
| [`pre-mortem`](./skills/product/pre-mortem/SKILL.md) | Imagine a project failed and work backward to surface risks. |

## Work Management Skills

| Skill | Purpose |
| --- | --- |
| [`handoff-readiness-brief`](./skills/work-management/handoff-readiness-brief/SKILL.md) | Make active work state transferable before pausing, resuming, reviewing, or handing off. |
| [`review-comment-action-plan`](./skills/work-management/review-comment-action-plan/SKILL.md) | Turn review comments into an ordered action plan with clarifications, deferrals, and verification. |
| [`ticket-readiness-audit`](./skills/work-management/ticket-readiness-audit/SKILL.md) | Check whether a ticket is ready to claim, clarify, split, block, or send back. |
| [`work-slice-splitter`](./skills/work-management/work-slice-splitter/SKILL.md) | Split broad work into independently claimable outcome slices before execution. |

## Compatibility

| Item type | Claude Code | OpenAI Codex | Gemini CLI |
| --- | --- | --- | --- |
| Skills | Yes | Yes | Yes |
| Project memory | `CLAUDE.md` points to `AGENTS.md` | `AGENTS.md` | `GEMINI.md` points to `AGENTS.md` |

## Deferred Procedural Packs

Stack-specific procedural skills are deliberately held back from v1. Neon, Drizzle, Stripe, Ink, and similar content should become coherent skill sets or packs instead of a loose miscellaneous bucket.

## Related Skill Packs

- [11-Mile-Labs/seo-aeo-skills](https://github.com/11-Mile-Labs/seo-aeo-skills) - SEO and Answer Engine Optimization methodology pack.

## Philosophy

- Originally authored by 11 Mile Labs: adapted from our internal tooling, with no copied third-party upstream content.
- Generic only: no business-specific process, private infrastructure, or project-local pipeline glue.
- Multi-harness where possible: skills use standard frontmatter plus the small forward-compatible `argument-hint` field.
- Skills only: published skills must not require a specific harness or Claude-only runtime feature.

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md).

## License

[MIT](./LICENSE)
