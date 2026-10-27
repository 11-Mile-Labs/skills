# Contributing

Thanks for helping keep this library useful and clean.

## Curation Bar

1. **Original 11 Mile Labs work only.** This repo may adapt our own internal tooling, but it must not copy or lightly adapt third-party upstream skills. If someone else already published the skill, link to theirs instead of republishing it here.
2. **Generic only.** Do not add business-specific, client-specific, personal, or project-local workflow content.
3. **Multi-harness first.** Skills should work across skills-compatible harnesses and must not require a specific host or Claude-only runtime feature.
4. **Progressive disclosure.** `SKILL.md` holds the rule, the verdicts, and the output contract. Worked examples live in `references/examples.md` inside that skill. The body tells the agent to read the examples only when a draft misses the contract. Do not put maintainer tuning notes in either file.
5. **Self-contained skills.** Each skill carries everything it needs in its own directory. `SKILL.md` and files under `references/` MUST NOT reference paths outside the skill's own folder — no `../` paths, no repo-level shared or common directories. Prose references to sibling skills are fine ("see also: the `aeo` skill in this repository"); file-path references are not. If two skills need the same content, duplicate it deliberately into each skill's `references/` rather than sharing a common file.
6. **Provenance required.** Each new item needs an authorship and license check before merging.

## Skill Frontmatter

Use:

```yaml
---
name: example-skill
description: "What the skill produces. Use when <trigger>. Do not <boundary>."
license: MIT
---
```

Allowed frontmatter fields are `name`, `description`, `license`, `compatibility`, `metadata`, and experimental `allowed-tools`. Do not add `argument-hint` or harness-only fields such as `model`, `effort`, `context`, or `disable-model-invocation`. This pack does not use `allowed-tools`.

`license` is repository metadata. Harnesses that do not recognize it ignore it.

When two skills could match one request, or the skill must stop before a nearby job, say so in the description. Name the other skill when there is one. Repeat that stop in a short Boundaries section in the body. A skill with neither a neighbor nor a stop does not need that section.

## Deferred Packs

Do not add loose procedural skills to v1. Stack-specific content should be designed as coherent packs, such as a future Neon skill set or Drizzle skill set.
