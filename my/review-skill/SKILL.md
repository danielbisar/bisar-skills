---
name: review-skill
description: Review and improve Agent Skills (SKILL.md files) against the agentskills.io specification and best practices. Use when the user wants to evaluate, audit, lint, or improve a skill file — including its description, body content, structure, and triggering quality. Also use when the user says "review this skill," "is this skill good," or "how can I improve this skill." Do not use for general document optimization or non-skill markdown files.
---

# Review Skill

Evaluate a SKILL.md file against the Agent Skills specification and best practices, then provide actionable findings.

## References

Read before reviewing:

- `references/specification.md` — frontmatter, naming, structure, progressive disclosure rules
- `references/best-practices.md` — body content quality, context efficiency, instruction patterns
- `references/optimizing-descriptions.md` — description triggering quality

## Review dimensions

1. **Spec compliance** — frontmatter fields, naming rules, size limits
2. **Description quality** — imperative framing, trigger coverage, negative boundary, conciseness
3. **Context efficiency** — only teaches what the agent would get wrong without it; no redundancy with description
4. **Control calibration** — prescriptive where fragile, flexible where multiple approaches work; defaults not menus
5. **Useful patterns** — gotchas, output templates, checklists, validation loops, progressive disclosure

## Workflow

1. Run through the review dimensions
2. Present numbered findings with severity:
   - **Violation**: breaks spec or causes incorrect behavior
   - **Improvement**: follows spec but underperforms against best practices
   - **Nitpick**: minor, optional enhancement
3. Offer numbered next steps the user can choose from

## Output format

```
## Findings

1. [Severity] **Short title** — Explanation and recommendation.
2. ...

## Next steps

1. [action]
2. [action]
3. Done for now
```

Do not apply fixes without user selection.
