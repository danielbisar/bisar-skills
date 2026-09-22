# Best Practices for Skill Creators

Source: https://agentskills.io/skill-creation/best-practices

## Start from real expertise

Don't ask an LLM to generate a skill without domain context — the result is vague and generic.

### Extract from a hands-on task

Complete a real task with an agent, then extract the reusable pattern:
- Steps that worked (the successful sequence)
- Corrections you made (where you steered the agent)
- Input/output formats
- Context the agent didn't already know

### Synthesize from project artifacts

Good sources: internal docs, runbooks, style guides, API specs, schemas, code review comments, issue trackers, VCS history, failure cases.

## Refine with real execution

Run the skill against real tasks. Read execution traces, not just final outputs. If the agent wastes time, common causes:
- Instructions too vague (agent tries several approaches)
- Instructions that don't apply to current task (agent follows them anyway)
- Too many options without a clear default

## Spending context wisely

Once activated, full SKILL.md loads into context. Every token competes for attention.

### Add what the agent lacks, omit what it knows

Focus on: project-specific conventions, domain-specific procedures, non-obvious edge cases, particular tools/APIs.

**Test**: "Would the agent get this wrong without this instruction?" If no, cut it.

### Design coherent units

Encapsulate a coherent unit of work that composes well with other skills.
- Too narrow: forces multiple skills to load, risks conflicts
- Too broad: hard to activate precisely

### Aim for moderate detail

Concise, stepwise guidance with a working example outperforms exhaustive documentation. Consider whether edge cases are better handled by the agent's judgment.

### Progressive disclosure

SKILL.md under 500 lines / 5,000 tokens — core instructions only. Move detail to `references/`.

Tell the agent *when* to load each file. "Read `references/api-errors.md` if the API returns a non-200 status code" beats generic "see references/ for details."

## Calibrating control

### Match specificity to fragility

- **Flexible**: multiple approaches valid, task tolerates variation. Explain *why* over rigid directives.
- **Prescriptive**: fragile operations, consistency matters, specific sequence required.

Most skills mix both. Calibrate each part independently.

### Provide defaults, not menus

Bad: "You can use pypdf, pdfplumber, PyMuPDF, or pdf2image..."
Good: "Use pdfplumber for text extraction. For scanned PDFs requiring OCR, use pdf2image with pytesseract instead."

### Favor procedures over declarations

Teach *how to approach a class of problems*, not what to produce for a specific instance.

## Patterns for effective instructions

Use what fits — not every skill needs all of them.

### Gotchas sections

Highest-value content. Environment-specific facts that defy reasonable assumptions — not general advice but concrete corrections to mistakes the agent will make.

Example:
```
## Gotchas

- The `users` table uses soft deletes. Queries must include
  `WHERE deleted_at IS NULL` or results will include deactivated accounts.
- The user ID is `user_id` in the database, `uid` in the auth service,
  and `accountId` in the billing API. All three refer to the same value.
```

Keep gotchas in SKILL.md so the agent reads them *before* encountering the situation.

### Templates for output format

More reliable than prose descriptions — agents pattern-match against concrete structures. Short templates inline; longer ones in `assets/`.

### Checklists for multi-step workflows

Help track progress and avoid skipping steps, especially with dependencies or validation gates.

### Validation loops

Do the work → run validator → fix issues → repeat until passes.

### Plan-validate-execute

For batch/destructive operations: create plan → validate against source of truth → execute only after validation.

### Bundling reusable scripts

If the agent reinvents the same logic each run, write a tested script and bundle it in `scripts/`.

Review bundled scripts using `using-scripts.md`.

## Evaluate outcomes, not only instructions

Trigger accuracy does not prove that a skill improves results. Compare realistic clean-context runs with and without the skill, inspect execution traces, and weigh output gains against added token and time costs.

Use `evaluating-skills.md` for the evaluation workflow.
