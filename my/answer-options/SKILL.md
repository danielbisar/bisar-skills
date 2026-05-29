---
name: answer-options
description: Use this skill when asking a question where concise numbered options would reduce typing or speed the reply. Use for suggested answers and similar cases where the user can reply with a number. Do not use when a small option set would distort the user's real answer.
---

# Answer Options

Ask one question at a time with numbered options so the user can reply by number.

## Decision rule

Use when a small, meaningful set of options will speed the reply.

Do not use when options would distort the user's real answer; ask open-ended instead.

## Rules

- Prefer 3-5 concise, distinct options that cover likely answers
- Optimize for reply by number alone
- Treat a bare number reply as selecting that option
- Include `Proposed answer` by default; omit it if it would bias or prematurely narrow the answer
- Omit `Rationale` unless decision-useful
- If no good small option set exists, ask open-ended instead of forcing weak options

## Template

```markdown
**Question**

1. [option]
2. [option]
3. [option]

Proposed answer: [number]
Rationale: [only if useful]

Reply with the number, or your own answer.
```
