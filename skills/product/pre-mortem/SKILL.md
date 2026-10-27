---
name: pre-mortem
description: "Name the likely ways this specific project fails, ranked by likelihood, each with one prevention. Use when reviewing a concrete plan. Do not argue whether to proceed at all. Use devil-advocate for that."
license: MIT
---

# Pre-Mortem

Imagine this project has already failed, then work backward to the specific causes.

## Boundaries

This skill starts from the assumption that the project shipped and failed. Whether to proceed at all is `devil-advocate`. Stay on this plan. Generic risk lists are a miss.

## Rules

1. Be specific. "Communication issues" is not a failure mode.
2. Use the actual plan, code, or context.
3. Rank by likelihood, not by how catastrophic the failure sounds.
4. Each failure mode gets one concrete prevention to take now.

## Execute

If the user provided a file path, read it. Otherwise ask which plan to examine.

Open with: "It's 3 months from now. This project shipped, and it failed. Here's what went wrong:"

Give 5-8 scenarios:

```markdown
**Failure: [What happened]**
Why: [A led to B led to C]
Likelihood: [High / Medium / Low]
Warning sign: [What would show up early]
Prevention: [One action to take now]
```

Rank the top 3 most likely failures and ask which of them to dig into.
