---
name: optimize-context-docs
description: Optimizes context-heavy repository documents such as agent prompts, system prompts, AGENTS.md, onboarding docs, guidance files, and long how-to docs. Use when the user asks to shorten, condense, deduplicate, or clean up prompt-like files while preserving behavior, constraints, guardrails, formats, and decision-relevant details.
---

# Optimize Context

Reduce token usage in context artifacts without changing meaning, behavior, requirements, or guardrails.

Context artifacts include:

- Agent prompts
- System prompts
- `AGENTS.md`
- `.github/agents/*.agent.md`
- `.github/prompts/*`
- Onboarding and guidance docs
- SKILL.md files
- Long how-to or knowledge docs used as model context

If the user names a file, optimize only that file unless they ask for a broader sweep.

## Core rules

Do not:

- Change technical meaning, requirements, naming conventions, or operational rules
- Remove prohibitions, safety constraints, or must/never rules
- Delete decision-relevant content
- Invent repo structure, policies, tools, or processes
- Silently resolve conflicting instructions
- Split files unless the user asks

Always preserve:

- Required frontmatter
- Required headings and formats
- Commands, paths, thresholds, tool names, and examples needed for correct execution
- Edge cases that prevent likely mistakes
- Standalone usability of the document

If two instructions conflict, stop and surface the conflict.

If importance is unclear, keep the content but compress it.

## Workflow

1. Scan structure
   - Identify sections and their purpose
   - Find repeated constraints, duplicated examples, stale guidance, and meta commentary

2. Classify content
   - Critical: changes behavior, prevents errors, defines rules, or protects safety
   - Important: improves decisions or clarifies edge cases
   - Optional: examples, rationale, historical notes, style preferences
   - Redundant/obsolete: repeated, superseded, stale, or no longer used

3. Compress safely
   - Convert verbose paragraphs to concise bullets
   - Merge overlapping sections
   - Deduplicate repeated rules
   - Replace hedging with direct conditions, e.g. “If X, do Y”
   - Keep concrete specifics intact
   - Remove filler, repeated rationale, and unnecessary explanation

4. Verify
   - Check every critical and important rule still exists
   - Check no guardrail was weakened
   - Check no new ambiguity was introduced
   - Check required formats still work
   - Check the document remains usable standalone

## Output format

Always provide:

~~~markdown
## Optimization analysis

- Size before: [lines/words/chars/tokens, with method]
- Size after: [lines/words/chars/tokens, with method]
- Main reductions:
  - [...]
- Risk notes:
  - [...]

## Optimized content

[Updated file in place when working in a repo, or full optimized content when returning text]

## Implementation summary

- [...]
- [...]
- [...]
~~~

Use a practical size proxy such as lines, words, or characters. If a token counter is available, state which method was used.

## Edge cases

- Already lean: if less than roughly 20% reduction is safe, do minor cleanup only and explain why larger cuts risk meaning loss
- Unclear importance: keep and compress; ask whether it may be removed
- Multiple purposes in one file: suggest splitting, but do not split without instruction
- Contradictions: surface them instead of choosing one silently
- Archival docs: do not optimize archival records unless the user explicitly asks

## Progress updates

For longer tasks, provide short updates at meaningful stages:

- Scanning structure
- Classifying content
- Optimizing
- Verifying

Ask questions only when needed to avoid changing meaning.
