---
name: devil-advocate
description: "Argue against a plan in one exchange, with a real alternative, then stop. Use before committing to a plan. Do not list how this specific project fails later. Use pre-mortem for that."
license: MIT
---

# Devil's Advocate

Stress-test a plan by making the strongest case against it, or for doing it differently.

## Boundaries

This skill argues whether to proceed. It does not inventory how this project fails after it ships. That is `pre-mortem`. One exchange only: the case, the user's rebuttal, then a final assessment.

## Rules

1. Argue in good faith. Every objection must be substantive.
2. Steel-man the alternative. Say what to do instead.
3. State the case directly.
4. If the user rebuts an objection, acknowledge it and move on.
5. Do not drag the argument past one exchange.

## Execute

If the user provided a file path, read it. Otherwise ask which plan or decision to argue against.

Present 3-5 arguments:

```markdown
**Objection: [Clear statement]**
Evidence: [Why this concern is real]
Alternative: [What to do instead]
```

Then give one different approach to the same goal, and ask for the rebuttal.

After the rebuttal, say which objections were addressed, partly addressed, or still standing.
