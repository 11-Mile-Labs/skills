# 11 Mile Labs Skills

Curated multi-harness skills from 11 Mile Labs.

This repository collects generic AI coding craft: small methodology skills that work across harnesses. It is intentionally not an installer, not a plugin, and not a vertical business pack.

## Install

```bash
npx skills add 11-Mile-Labs/skills
```

The installer is maintained by the skills ecosystem. This repo only ships content.

## What's Inside

## Methodology Skills

These are standard `SKILL.md` files intended to work in Claude Code, OpenAI Codex, Gemini CLI, and other skills-compatible harnesses.

| Skill | Purpose |
| --- | --- |
| [`bug-repro-brief`](./skills/methodology/bug-repro-brief/SKILL.md) | Turn messy bug reports into reproducible bug briefs before debugging or ticketing. |
| [`devil-advocate`](./skills/methodology/devil-advocate/SKILL.md) | Argue against a plan in good faith before committing. |
| [`explain-like-5`](./skills/methodology/explain-like-5/SKILL.md) | Force a simple explanation of a concept, architecture, or decision. |
| [`pre-mortem`](./skills/methodology/pre-mortem/SKILL.md) | Imagine a project failed and work backward to surface risks. |
| [`scope-check`](./skills/methodology/scope-check/SKILL.md) | Audit scope creep, hidden complexity, and v1/v2 boundaries. |

## Compatibility

| Item type | Claude Code | OpenAI Codex | Gemini CLI |
| --- | --- | --- | --- |
| Methodology skills | Yes | Yes | Yes |
| Project memory | `CLAUDE.md` points to `AGENTS.md` | `AGENTS.md` | `GEMINI.md` points to `AGENTS.md` |

## Deferred Procedural Packs

Stack-specific procedural skills are deliberately held back from v1. Neon, Drizzle, Stripe, Ink, and similar content should become coherent skill sets or packs instead of a loose miscellaneous bucket.

## Philosophy

- Originally authored by 11 Mile Labs: adapted from our internal tooling, with no copied third-party upstream content.
- Generic only: no business-specific process, private infrastructure, or project-local pipeline glue.
- Multi-harness where possible: skills use standard frontmatter plus the small forward-compatible `argument-hint` field.
- Skills only: published skills must not require a specific harness or Claude-only runtime feature.

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md).

## License

[MIT](./LICENSE)
