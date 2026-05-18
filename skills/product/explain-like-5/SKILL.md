---
name: explain-like-5
description: "Force a simple explanation of any concept, architecture, or decision. If you can't explain it simply, you don't understand it well enough."
argument-hint: "[concept, file path, or question to explain simply]"
license: MIT
---

# Explain Like I'm 5 - Simplicity Test

Your job is to explain the given concept, system, or decision in the simplest possible terms. Use analogies, avoid jargon, and test whether the underlying idea is actually well-understood.

## Rules

1. **No jargon.** If a 5-year-old wouldn't know the word, don't use it, or define it immediately with an analogy.
2. **Use analogies.** Connect abstract concepts to physical, everyday things.
3. **Short sentences.** If a sentence has a comma, consider splitting it.
4. **Test understanding, don't just simplify.** After explaining, ask a question that reveals whether the mental model is correct.
5. **Be honest about complexity.** If something is genuinely complex, say "this part is tricky" and explain why. Don't hand-wave.

## Execute Now

### Step 1 - Load Context

If the user provided a file path, read it to understand the system. If they named a concept, use that. If neither, ask: "What should I explain simply?"

### Step 2 - The Simple Version

Explain the concept in 3-5 short paragraphs, max. Use at least one analogy.

### Step 3 - The Check

Ask one question that tests whether the explanation created the right mental model. Something like: "Based on that explanation, what would you expect to happen if [scenario]?"

### Step 4 - Go Deeper If Asked

If the user wants more detail on a specific part, explain that part. Stay simple, but add more precision. Layer complexity gradually; don't dump it.
