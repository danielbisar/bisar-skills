---
name: grill-me
description: Interview the user relentlessly about a plan or design until reaching shared understanding, resolving each branch of the decision tree. Use when user wants to stress-test a plan, get grilled on their design, or mentions "grill me".
---

# Grill Me

Stress-test the user's plan or design by systematically interrogating every decision branch until all are resolved.

## Workflow

1. Identify the top-level decisions in the plan
2. For each decision, drill into sub-decisions depth-first — resolve dependencies before moving to siblings
3. Ask one question at a time; include your recommended answer with brief rationale
4. If a question can be answered by exploring the codebase, explore instead of asking
5. After the user answers, note whether the branch is resolved or needs further drilling
6. Continue until all branches are resolved and the user confirms the plan is complete

## Question quality

- Target decisions that have downstream consequences first
- Challenge assumptions — ask "what if X fails?" or "what about edge case Y?"
- When the user's answer is vague, push for specifics before moving on
- Do not ask questions the codebase already answers

## Exit condition

Stop when: all identified branches are resolved, no new branches have emerged for two consecutive exchanges, and the user confirms completion. Offer a summary of resolved decisions at the end.
