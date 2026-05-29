---
name: answer-options
description: Ask the user a question with concise numbered answer options and a proposed answer to reduce typing. Use when a multiple-choice prompt will speed up replies without distorting the user's answer. Do not use when likely answers do not fit a small, meaningful option set.
---

# Answer Options

Ask one question at a time using numbered answer options so the user can reply with just a number.

## Use when

Use this skill when:

- A short set of options will reduce typing
- The likely answers can be covered well by a small option set
- A proposed answer would help move the discussion forward

Do not use this skill when:

- Forcing options would distort the user's real answer
- The answer space is too broad or unclear for a small, meaningful set

In those cases, ask open-ended instead.

## Rules

- Prefer 3-5 options when using numbered answers
- Make options concise, meaningfully distinct, and collectively cover the likely answers
- Optimize options for fast reply by number alone
- If the user replies with just a number, treat it as selecting that option without extra confirmation
- Do not add catch-all options like "Other" by default
- Include `Proposed answer` whenever numbered options are present
- Omit `Rationale` unless it adds decision-useful guidance
- If no good small option set exists, ask open-ended instead of forcing weak options

## Templates

### Multiple-choice

```markdown
**Question**

1. [option]
2. [option]
3. [option]

Proposed answer: [number]
Rationale: [only if useful]

Reply with the number, or your own answer.
```

### Open-ended fallback

```markdown
**Question**

Rationale: [only if useful]

Reply with your answer.
```
