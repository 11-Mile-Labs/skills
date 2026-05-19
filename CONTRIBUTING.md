# Contributing

Thanks for helping keep this library useful and clean.

## Curation Bar

1. **Original 11 Mile Labs work only.** This repo may adapt our own internal tooling, but it must not copy or lightly adapt third-party upstream skills. If someone else already published the skill, link to theirs instead of republishing it here.
2. **Generic only.** Do not add business-specific, client-specific, personal, or project-local workflow content.
3. **Multi-harness first.** Skills should work across skills-compatible harnesses and must not require a specific host or Claude-only runtime feature.
4. **Progressive disclosure.** Keep `SKILL.md` concise. When a skill grows beyond the essential workflow, move long reference material into a `references/` directory inside that skill's own folder.
5. **Self-contained skills.** Each skill carries everything it needs in its own directory. `SKILL.md` and files under `references/` MUST NOT reference paths outside the skill's own folder — no `../` paths, no repo-level shared or common directories. Prose references to sibling skills are fine ("see also: the `aeo` skill in this repository"); file-path references are not. If two skills need the same content, duplicate it deliberately into each skill's `references/` rather than sharing a common file.
6. **Provenance required.** Each new item needs an authorship and license check before merging.

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

## Deferred Packs

Do not add loose procedural skills to v1. Stack-specific content should be designed as coherent packs, such as a future Neon skill set or Drizzle skill set.
