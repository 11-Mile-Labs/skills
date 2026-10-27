# AGENTS.md

This repository is a curated, generic skills library for AI coding harnesses.

## Project Purpose

`11-Mile-Labs/skills` publishes reusable engineering, QA, product, and work-management skills that are original, broadly useful, and safe to install outside 11 Mile Labs projects.

## Scope

Included in v1:

- Standard multi-harness skills under `skills/engineering/`, `skills/qa/`, `skills/product/`, and `skills/work-management/`

Excluded from v1:

- Procedural stack skills such as Neon, Drizzle, Stripe, and Ink
- Vercel-authored or Vercel-derived skills
- Claude Code-specific definitions or other harness-specific runtime definitions
- Vertical business content
- Private orchestration or MCP-coupled workflow glue
- Personal paths, secrets, project-specific backlog formats, and hardcoded company infrastructure

## Authoring Rules

Skills must use Agent Skills frontmatter. The reference validator allows `name`, `description`, `license`, `compatibility`, `metadata`, and experimental `allowed-tools`. Anything else, including `argument-hint`, fails validation.

```yaml
---
name: example-skill
description: "What the skill produces. Use when <trigger>. Do not <boundary>."
license: MIT
---
```

The description is the only text every harness loads before choosing a skill. State the job, the trigger, and a boundary when a neighboring skill could match the same request. Name that neighbor.

Keep the body to the rule, the verdicts, and the output contract. Put worked examples in `references/examples.md`, and tell the agent to read that file only when a draft misses the contract.

Do not add harness-only execution fields such as `model`, `effort`, `allowed-tools`, `context`, `user-invocable`, or `disable-model-invocation` to `skills/**/SKILL.md`. `allowed-tools` is in the spec and is experimental; this pack does not use it.

## Layout

- `skills/engineering/<name>/SKILL.md` - engineering workflow and change-risk skills
- `skills/qa/<name>/SKILL.md` - quality, verification, bug, and test strategy skills
- `skills/product/<name>/SKILL.md` - product thinking, decision, criteria, and framing skills
- `skills/work-management/<name>/SKILL.md` - ticket, review, handoff, and work-slicing skills

## Provenance

Every new public item must be checked for:

- Original authorship
- License compatibility
- Source attribution if any upstream material is included
- Absence of private business or project-specific content

When in doubt, exclude it from this repository and link to the upstream source instead.
