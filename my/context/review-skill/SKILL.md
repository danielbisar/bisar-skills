---
name: review-skill
description: Review and improve Agent Skills (SKILL.md files) against the agentskills.io specification and best practices. Use when the user wants to evaluate, audit, lint, or improve a skill file — including its description, body content, structure, and triggering quality. Also use when the user says "review this skill," "is this skill good," or "how can I improve this skill." Do not use for general document optimization or non-skill markdown files.
---

# Review Skill

Evaluate a SKILL.md file against the Agent Skills specification and best practices, then provide actionable findings.

## References

Read these before reviewing:

- Read `references/specification.md` — for checking frontmatter, naming, structure, and progressive disclosure rules
- Read `references/best-practices.md` — for evaluating body content quality, context efficiency, and instruction patterns
- Read `references/optimizing-descriptions.md` — for evaluating description triggering quality

## Review checklist

### 1. Specification compliance

- `name`: lowercase, hyphens only, no consecutive hyphens, matches directory name, ≤64 chars
- `description`: non-empty, ≤1024 chars, describes what the skill does and when to use it
- Body: valid markdown after YAML frontmatter
- Size: body under 500 lines / ~5000 tokens (progressive disclosure)

### 2. Description quality

- Uses imperative framing ("Use when…") not declarative ("This skill does…")
- Focuses on user intent, not implementation details
- Explicitly lists trigger contexts, including non-obvious ones
- Includes a negative boundary (what the skill is NOT for)
- Concise: a few sentences to a short paragraph

### 3. Body content — spending context wisely

- Does NOT teach the agent things it already knows (generic writing advice, common programming concepts)
- Contains knowledge the agent would get wrong without being told
- Gotchas and edge cases that defy reasonable assumptions
- No redundancy between description and body (description triggers activation; body guides execution)

### 4. Body content — calibrating control

- Prescriptive where operations are fragile or consistency matters
- Flexible where multiple approaches are valid
- Provides defaults, not menus of equal options
- Favors procedures over declarations
- Includes a validation loop where appropriate (do → verify → fix → re-verify)

### 5. Patterns present where useful

- Gotchas section for environment-specific facts
- Output format template (if consistent output is needed)
- Checklists for multi-step workflows
- Progressive disclosure: references/ for detail loaded on demand

### 6. Things to flag

- Content the agent already knows (candidate for removal)
- Redundancy between sections or between description and body
- Arbitrary thresholds without justification
- Missing negative boundaries in description
- Instructions that could cause false triggers or wasted steps

## Workflow

1. Read the skill file
2. Run through the review checklist
3. Present numbered findings with severity:
   - **Violation**: breaks spec or will cause incorrect behavior
   - **Improvement**: follows spec but underperforms against best practices
   - **Nitpick**: minor, optional enhancement
4. After presenting findings, offer numbered next steps the user can choose from

## Output format

Present findings as a numbered list:

```
## Findings

1. [Severity] **Short title** — Explanation and recommendation.
2. [Severity] **Short title** — Explanation and recommendation.
...

## Next steps

1. [action]
2. [action]
3. Done for now
```

Always let the user choose next steps by number. Do not apply fixes without selection.
