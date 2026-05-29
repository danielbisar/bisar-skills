---
name: grill-me
description: Interview the user relentlessly about a plan or design until reaching shared understanding, resolving each branch of the decision tree. Use when user wants to stress-test a plan, get grilled on their design, or mentions "grill me".
---

# Grill Me

## Workflow

1. Identify top-level decisions in the plan
2. Drill into sub-decisions depth-first — resolve dependencies before siblings
3. Ask one question at a time; include your recommended answer with brief rationale
4. If a question can be answered by exploring the codebase, explore instead of asking
5. After the user answers, note whether the branch is resolved or needs further drilling
6. Continue until all branches are resolved and the user confirms completion; then summarize resolved decisions

## Question quality

- Prioritize decisions with downstream consequences
- Challenge assumptions — "what if X fails?" / "what about edge case Y?"
- Push for specifics when answers are vague
- Do not ask questions the codebase already answers
