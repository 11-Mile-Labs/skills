# 11 Mile Labs Skills

Curated multi-harness skills from 11 Mile Labs.

This repository collects generic AI coding craft: small reusable skills that work across harnesses. It is intentionally not an installer, not a plugin, and not a vertical business pack.

## Install

```bash
npx skills add 11-Mile-Labs/skills
```

The installer is maintained by the skills ecosystem. This repo only ships content.

## What's Inside

These are standard `SKILL.md` files for harnesses that follow the Agent Skills specification.

## Engineering Skills

| Skill | Purpose |
| --- | --- |
| [`dependency-upgrade-risk-check`](./skills/engineering/dependency-upgrade-risk-check/SKILL.md) | Classify dependency upgrade risk and define proof before updating. |
| [`deprecation-plan-brief`](./skills/engineering/deprecation-plan-brief/SKILL.md) | Plan safe removals with compatibility, notice, proof, and rollback. |
| [`implementation-drift-check`](./skills/engineering/implementation-drift-check/SKILL.md) | Compare stated intent with the actual change set. |
| [`regression-risk-map`](./skills/engineering/regression-risk-map/SKILL.md) | Map observable regressions and the checks that would catch them. |
| [`release-readiness-brief`](./skills/engineering/release-readiness-brief/SKILL.md) | Call a change ready, ready with watch, needing evidence, or hold. |
| [`scope-check`](./skills/engineering/scope-check/SKILL.md) | Cut a plan down to the smallest shippable set. |

## QA Skills

| Skill | Purpose |
| --- | --- |
| [`bug-repro-brief`](./skills/qa/bug-repro-brief/SKILL.md) | Turn a messy bug report into facts, assumptions, and one reproduction confidence. |
| [`test-failure-signal-brief`](./skills/qa/test-failure-signal-brief/SKILL.md) | Classify a failing check before editing code or assertions. |
| [`test-strategy-slicer`](./skills/qa/test-strategy-slicer/SKILL.md) | Choose focused proof layers for a change instead of defaulting to coverage theater. |
| [`verification-contract`](./skills/qa/verification-contract/SKILL.md) | Define the evidence required before claiming a feature, ticket, or plan is done. |

## Product Skills

| Skill | Purpose |
| --- | --- |
| [`acceptance-criteria-tightener`](./skills/product/acceptance-criteria-tightener/SKILL.md) | Rewrite vague acceptance criteria into observable pass/fail statements. |
| [`decision-record-snapshot`](./skills/product/decision-record-snapshot/SKILL.md) | Record one decision with its source, scope, and revisit trigger. |
| [`devil-advocate`](./skills/product/devil-advocate/SKILL.md) | Argue against a plan in one exchange, then stop. |
| [`pre-mortem`](./skills/product/pre-mortem/SKILL.md) | Name how this specific project fails, ranked by likelihood. |

## Work Management Skills

| Skill | Purpose |
| --- | --- |
| [`handoff-readiness-brief`](./skills/work-management/handoff-readiness-brief/SKILL.md) | Make active work state transferable before pausing, resuming, reviewing, or handing off. |
| [`review-comment-action-plan`](./skills/work-management/review-comment-action-plan/SKILL.md) | Turn review comments into an ordered action plan with clarifications, deferrals, and verification. |
| [`ticket-readiness-audit`](./skills/work-management/ticket-readiness-audit/SKILL.md) | Check whether a ticket is ready to claim, clarify, split, block, or send back. |
| [`work-slice-splitter`](./skills/work-management/work-slice-splitter/SKILL.md) | Split broad work into independently claimable outcome slices before execution. |

## Compatibility

| Harness | How these skills load |
| --- | --- |
| Cursor | Discovers `SKILL.md`, shows the description, and lets the agent load the body. |
| Claude Code | Same discovery. An invoked skill stays in the conversation. |
| Codex | Same discovery. The description is what gets matched, and long catalogs are shortened from the description first. |
| Grok Build | Same discovery. Unknown frontmatter keys are ignored. |
| Grok Bot | A different skill model: a saved instruction card in the Bot library, not this folder layout. |

`CLAUDE.md` and `GEMINI.md` point at `AGENTS.md`. Codex and Cursor read `AGENTS.md` directly.

## Deferred Procedural Packs

Stack-specific procedural skills are deliberately held back from v1. Neon, Drizzle, Stripe, Ink, and similar content should become coherent skill sets or packs instead of a loose miscellaneous bucket.

## Related Skill Packs

- [11-Mile-Labs/seo-aeo-skills](https://github.com/11-Mile-Labs/seo-aeo-skills) - SEO and Answer Engine Optimization methodology pack.

## Philosophy

- Originally authored by 11 Mile Labs: adapted from our internal tooling, with no copied third-party upstream content.
- Generic only: no business-specific process, private infrastructure, or project-local pipeline glue.
- Multi-harness: frontmatter stays inside the Agent Skills fields (`name`, `description`, `license`). Descriptions name a boundary when two skills could match. Examples live in `references/` and load only when a draft misses the contract.
- Skills only: published skills must not require a specific harness or Claude-only runtime feature.

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md).

## License

[MIT](./LICENSE)
