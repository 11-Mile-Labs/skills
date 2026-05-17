# AGENTS.md

This repository is a curated, generic skills and agents library for AI coding harnesses.

## Project Purpose

`11-Mile-Labs/skills` publishes reusable agent methodology and specialist-agent definitions that are original, broadly useful, and safe to install outside 11 Mile Labs projects.

## Scope

Included in v1:

- Standard methodology skills under `skills/methodology/`
- Claude Code specialist agents under `agents/`

Excluded from v1:

- Procedural stack skills such as Neon, Drizzle, Stripe, and Ink
- Vercel-authored or Vercel-derived skills
- Vertical business content
- Private orchestration or MCP-coupled workflow glue
- Personal paths, secrets, project-specific backlog formats, and hardcoded company infrastructure

## Authoring Rules

Skills must use the Agent Skills standard with minimal frontmatter:

```yaml
---
name: example-skill
description: "Clear trigger-oriented description."
argument-hint: "[optional-input]"
license: MIT
---
```

Use `argument-hint` only when a skill benefits from a user-supplied target. Do not add Claude-only execution fields such as `model`, `effort`, `allowed-tools`, `context`, or `user-invocable` to `skills/**/SKILL.md`.

Claude agents may use Claude agent frontmatter:

```yaml
---
name: example-agent
description: "Clear trigger-oriented description."
tools: Read, Grep, Glob
model: sonnet
---
```

Do not ship runtime/session fields such as `memory`, `background`, `initialPrompt`, `permissionMode`, or `isolation`.

## Layout

- `skills/methodology/<name>/SKILL.md` - multi-harness methodology skills
- `agents/<name>.md` - Claude Code-only specialist agents

## Provenance

Every new public item must be checked for:

- Original authorship
- License compatibility
- Source attribution if any upstream material is included
- Absence of private business or project-specific content

When in doubt, exclude it from this repository and link to the upstream source instead.
