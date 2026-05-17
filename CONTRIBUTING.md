# Contributing

Thanks for helping keep this library useful and clean.

## Curation Bar

1. **Original 11 Mile Labs work only.** This repo may adapt our own internal tooling, but it must not copy or lightly adapt third-party upstream skills. If someone else already published the skill, link to theirs instead of republishing it here.
2. **Generic only.** Do not add business-specific, client-specific, personal, or project-local workflow content.
3. **Multi-harness first.** Skills should work across Agent Skills-compatible harnesses. Claude-only behavior belongs in `agents/`, not in skill frontmatter.
4. **Progressive disclosure.** Keep `SKILL.md` concise. Move long reference material into a clearly named `references/` directory when a skill grows beyond the essential workflow.
5. **Provenance required.** Each new item needs an authorship and license check before merging.

## Skill Frontmatter

Use:

```yaml
---
name: example-skill
description: "Clear trigger-oriented description."
argument-hint: "[optional-input]"
license: MIT
---
```

`argument-hint` is allowed because it is small, useful invocation metadata and likely to become part of the broader skill ecosystem. Omit it when the skill does not take a meaningful argument.

`license` is included as repository metadata. Harnesses that do not recognize it should ignore it.

Do not use Claude-only execution fields in `skills/**/SKILL.md`.

## Agent Frontmatter

Claude Code agents may keep:

```yaml
---
name: example-agent
description: "Clear trigger-oriented description."
tools: Read, Grep, Glob
model: sonnet
---
```

Strip runtime/session fields before publishing.

## Deferred Packs

Do not add loose procedural skills to v1. Stack-specific content should be designed as coherent packs, such as a future Neon skill set or Drizzle skill set.
