---
name: optimize-context-docs
description: Reduce token usage in documents that serve as LLM context while preserving behavior, constraints, and guardrails. Covers agent prompts, system prompts, skill files, instruction files, onboarding docs, and similar artifacts. Use when the user wants to shorten, compress, deduplicate, or clean up any file consumed as agent or model context, even if they just say "too long" or "make it shorter." Do not use for general prose editing, summarization of end-user docs, or copywriting.
---

# Optimize Context

Reduce token usage in context artifacts without changing meaning, behavior, requirements, or guardrails. If the user names a file, optimize only that file unless they ask for a broader sweep.

## Core rules

Do not:

- Remove prohibitions, safety constraints, or must/never rules
- Delete decision-relevant content
- Invent repo structure, policies, tools, or processes
- Silently resolve conflicting instructions
- Split files unless the user asks

Always preserve:

- Required frontmatter, headings, and formats
- Commands, paths, thresholds, tool names, and examples needed for correct execution
- Edge cases that prevent likely mistakes
- Standalone usability

If two instructions conflict, stop and surface the conflict.

If importance is unclear, keep and compress; ask whether it may be removed.

## Workflow

1. Scan structure — identify section purposes; find repetition, duplication, stale guidance, meta commentary

2. Classify content
   - Critical: changes behavior, prevents errors, defines rules, protects safety
   - Important: improves decisions, clarifies edge cases
   - Optional: examples, rationale, historical notes, style preferences
   - Redundant/obsolete: repeated, superseded, stale, unused

3. Compress safely — merge overlapping sections, deduplicate rules, keep concrete specifics

4. Verify
   - Every critical/important rule still exists
   - No guardrail weakened, no new ambiguity introduced
   - Required formats still work; document remains standalone-usable
   - If any check fails, fix and re-verify

## Output format

After optimizing, provide:

```markdown
## Optimization report

- Before: [size metric]
- After:  [size metric]
- Reduction: [percentage]
- Cuts: [what was removed or merged]
- Risks: [anything that might have changed meaning, or "none"]
```

Then edit the file in place, or output full optimized content if not working in a repo.

Ask questions only when needed to avoid changing meaning.
