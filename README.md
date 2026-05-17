# 11 Mile Labs Skills

Curated multi-harness skills and Claude Code agents from 11 Mile Labs.

This repository collects generic AI coding-agent craft: small methodology skills that work across harnesses, plus a focused set of Claude Code specialist agents. It is intentionally not an installer, not a plugin, and not a vertical business pack.

## Install

```bash
npx skills add 11-Mile-Labs/skills
```

The installer is maintained by the Agent Skills ecosystem. This repo only ships content.

## What's Inside

### Methodology Skills

These are standard `SKILL.md` files intended to work in Claude Code, OpenAI Codex, Gemini CLI, and other Agent Skills-compatible harnesses.

| Skill | Purpose |
| --- | --- |
| [`devil-advocate`](./skills/methodology/devil-advocate/SKILL.md) | Argue against a plan in good faith before committing. |
| [`explain-like-5`](./skills/methodology/explain-like-5/SKILL.md) | Force a simple explanation of a concept, architecture, or decision. |
| [`pre-mortem`](./skills/methodology/pre-mortem/SKILL.md) | Imagine a project failed and work backward to surface risks. |
| [`scope-check`](./skills/methodology/scope-check/SKILL.md) | Audit scope creep, hidden complexity, and v1/v2 boundaries. |

### Claude Code Agents

These files use Claude Code's `agents/` convention. Other harnesses can ignore them.

| Agent | Specialty |
| --- | --- |
| [`accessibility-tester`](./agents/accessibility-tester.md) | WCAG, screen readers, keyboard navigation, ARIA, and inclusive design. |
| [`debugger`](./agents/debugger.md) | Systematic root-cause debugging and error analysis. |
| [`nextjs-pro`](./agents/nextjs-pro.md) | Next.js App Router, server components, caching, performance, and SEO. |
| [`performance-engineer`](./agents/performance-engineer.md) | Core Web Vitals, profiling, caching, bundles, APIs, and database performance. |
| [`postgres-pro`](./agents/postgres-pro.md) | PostgreSQL query optimization, schema design, indexing, JSONB, and Neon. |
| [`refactoring-specialist`](./agents/refactoring-specialist.md) | Safe behavior-preserving refactoring and code quality improvement. |
| [`security-auditor`](./agents/security-auditor.md) | Read-only adversarial security review and attack-chain analysis. |
| [`typescript-pro`](./agents/typescript-pro.md) | Advanced TypeScript, type-safe contracts, and monorepo type architecture. |

## Compatibility

| Item type | Claude Code | OpenAI Codex | Gemini CLI |
| --- | --- | --- | --- |
| Methodology skills | Yes | Yes | Yes |
| Claude agents | Yes | No native equivalent | No native equivalent |
| Project memory | `CLAUDE.md` points to `AGENTS.md` | `AGENTS.md` | `GEMINI.md` points to `AGENTS.md` |

## Deferred Procedural Packs

Stack-specific procedural skills are deliberately held back from v1. Neon, Drizzle, Stripe, Ink, and similar content should become coherent skill sets or packs instead of a loose miscellaneous bucket.

## Philosophy

- Originally authored by 11 Mile Labs: adapted from our internal tooling, with no copied third-party upstream content.
- Generic only: no business-specific process, private infrastructure, or project-local pipeline glue.
- Multi-harness where possible: skills use standard frontmatter plus the small forward-compatible `argument-hint` field.
- Claude-specific only where honest: agents stay in `agents/` and are documented as Claude Code-only.

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md).

## License

[MIT](./LICENSE)
