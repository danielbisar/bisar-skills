# Best Practices for Skill Creators

Source: https://agentskills.io/skill-creation/best-practices

## Start from real expertise

A common pitfall: asking an LLM to generate a skill without domain-specific context. The result is vague, generic procedures rather than specific patterns, edge cases, and conventions that make a skill valuable.

### Extract from a hands-on task

Complete a real task with an agent, then extract the reusable pattern. Pay attention to:
- Steps that worked — the sequence that led to success
- Corrections you made — where you steered the agent
- Input/output formats
- Context you provided that the agent didn't already know

### Synthesize from existing project artifacts

Good source material:
- Internal docs, runbooks, style guides
- API specs, schemas, config files
- Code review comments and issue trackers
- Version control history (patches and fixes)
- Real-world failure cases and resolutions

## Refine with real execution

Run the skill against real tasks, feed results back. Ask: what triggered false positives? What was missed? What could be cut?

Read agent execution traces, not just final outputs. If the agent wastes time on unproductive steps, common causes:
- Instructions too vague (agent tries several approaches)
- Instructions that don't apply to current task (agent follows them anyway)
- Too many options without a clear default

## Spending context wisely

Once activated, the full SKILL.md loads into context alongside conversation history, system context, and other skills. Every token competes for attention.

### Add what the agent lacks, omit what it knows

Focus on: project-specific conventions, domain-specific procedures, non-obvious edge cases, particular tools/APIs to use.

**Test**: "Would the agent get this wrong without this instruction?" If no, cut it. If unsure, test it. If the agent handles the task well without the skill, the skill may not be adding value.

### Design coherent units

Like deciding what a function should do — encapsulate a coherent unit of work that composes well with other skills.
- Too narrow: forces multiple skills to load, risks overhead and conflicts
- Too broad: hard to activate precisely

### Aim for moderate detail

Overly comprehensive skills can hurt — agent struggles to extract what's relevant and may pursue unproductive paths. Concise, stepwise guidance with a working example outperforms exhaustive documentation. When covering every edge case, consider whether most are better handled by the agent's judgment.

### Structure large skills with progressive disclosure

SKILL.md under 500 lines / 5,000 tokens — core instructions only. Move detail to `references/` or similar.

Key: tell the agent *when* to load each file. "Read `references/api-errors.md` if the API returns a non-200 status code" is better than generic "see references/ for details."

## Calibrating control

Match specificity to the fragility of the task.

### Match specificity to fragility

- **Flexible**: when multiple approaches are valid and task tolerates variation. Explaining *why* is more effective than rigid directives.
- **Prescriptive**: when operations are fragile, consistency matters, or a specific sequence must be followed.

Most skills have a mix. Calibrate each part independently.

### Provide defaults, not menus

Pick a default and mention alternatives briefly rather than presenting equal options.

Bad: "You can use pypdf, pdfplumber, PyMuPDF, or pdf2image..."
Good: "Use pdfplumber for text extraction. For scanned PDFs requiring OCR, use pdf2image with pytesseract instead."

### Favor procedures over declarations

Teach the agent *how to approach a class of problems*, not what to produce for a specific instance. The approach should generalize even when individual details are specific.

## Patterns for effective instructions

Use the ones that fit your task — not every skill needs all of them.

### Gotchas sections

Highest-value content in many skills. Environment-specific facts that defy reasonable assumptions. Not general advice ("handle errors appropriately") but concrete corrections to mistakes the agent will make.

Example:
```
## Gotchas

- The `users` table uses soft deletes. Queries must include
  `WHERE deleted_at IS NULL` or results will include deactivated accounts.
- The user ID is `user_id` in the database, `uid` in the auth service,
  and `accountId` in the billing API. All three refer to the same value.
```

Keep gotchas in SKILL.md where the agent reads them *before* encountering the situation. When an agent makes a mistake you correct, add it to gotchas.

### Templates for output format

More reliable than describing format in prose — agents pattern-match well against concrete structures. Short templates inline in SKILL.md; longer ones in `assets/` referenced from SKILL.md.

### Checklists for multi-step workflows

Explicit checklists help track progress and avoid skipping steps, especially with dependencies or validation gates.

### Validation loops

Pattern: do the work → run validator → fix issues → repeat until passes. The validator can be a script, a reference checklist, or a self-check.

### Plan-validate-execute

For batch or destructive operations: create intermediate plan in structured format → validate against source of truth → execute only after validation passes.

### Bundling reusable scripts

If the agent independently reinvents the same logic each run, write a tested script once and bundle it in `scripts/`.
