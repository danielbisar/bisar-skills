# Optimizing Skill Descriptions

Source: https://agentskills.io/skill-creation/optimizing-descriptions

## How skill triggering works

Agents load only `name` and `description` at startup. When a task matches, the agent reads full SKILL.md.

The description carries the entire triggering burden. Agents typically only consult skills for tasks requiring knowledge beyond what they can handle alone.

## Writing effective descriptions

- **Imperative phrasing**: "Use this skill when…" not "This skill does…" — tell the agent *when* to act.
- **Focus on user intent**: Describe what the user is trying to achieve, not internal mechanics.
- **Be pushy**: Explicitly list contexts where the skill applies, including indirect references.
- **Stay concise**: Few sentences to a short paragraph. Hard limit: 1024 characters.

## Designing trigger eval queries

Create ~20 queries: 8-10 should-trigger, 8-10 should-not-trigger.

Format:
```json
[
  { "query": "...", "should_trigger": true },
  { "query": "...", "should_trigger": false }
]
```

### Should-trigger queries

Vary along: phrasing (formal/casual/typos), explicitness (naming domain vs describing need), detail level, complexity (single-step vs multi-step).

Most useful: queries where the skill would help but the connection isn't obvious.

### Should-not-trigger queries

Most valuable: **near-misses** — queries sharing keywords/concepts but needing something different.

Weak negatives (obviously irrelevant) test nothing. Strong negatives share domain concepts but need a different capability.

### Realism tips

Include: file paths, personal context, specific details, casual language, abbreviations, typos.

## Testing triggers

Run each query through the agent with the skill installed. Skill triggered = agent loaded SKILL.md.

Model behavior is nondeterministic — run each query 3+ times. Should-trigger passes if rate > 0.5. Should-not-trigger passes if rate < 0.5.

## Avoiding overfitting

Split queries into train (~60%) and validation (~40%) sets. Both need proportional positive/negative mix. Only use train failures to guide changes.

## The optimization loop

1. Evaluate current description on train and validation sets
2. Identify train set failures
3. Revise description:
   - Should-trigger failing → too narrow, broaden scope
   - Should-not-trigger failing → too broad, add specificity about what skill does NOT do
   - Don't add specific keywords from failed queries (overfitting) — find the general category
   - If stuck, try structurally different approach
   - Check stays under 1024 chars
4. Repeat until train set passes (~5 iterations usually enough)
5. Select best iteration by validation pass rate

## Applying the result

1. Update `description` in SKILL.md frontmatter
2. Verify under 1024-character limit
3. Verify with fresh queries (never part of optimization) for honest generalization check
