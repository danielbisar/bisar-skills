# Optimizing Skill Descriptions

Source: https://agentskills.io/skill-creation/optimizing-descriptions

## How skill triggering works

Agents load only `name` and `description` at startup. When a task matches a description, the agent reads the full SKILL.md.

The description carries the entire burden of triggering. If it doesn't convey when the skill is useful, the agent won't reach for it.

Important nuance: agents typically only consult skills for tasks requiring knowledge beyond what they can handle alone. Simple one-step requests may not trigger even with a perfect description match.

## Writing effective descriptions

### Principles

- **Use imperative phrasing**: "Use this skill when…" not "This skill does…" The agent is deciding whether to act — tell it *when* to act.
- **Focus on user intent, not implementation**: Describe what the user is trying to achieve, not the skill's internal mechanics.
- **Err on the side of being pushy**: Explicitly list contexts where the skill applies, including cases where the user doesn't name the domain directly.
- **Keep it concise**: A few sentences to a short paragraph. Long enough to cover scope, short enough to not bloat context across many skills. Hard limit: 1024 characters.

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

Vary along:
- **Phrasing**: formal, casual, typos, abbreviations
- **Explicitness**: some name the domain directly, others describe the need without naming it
- **Detail**: mix terse prompts with context-heavy ones
- **Complexity**: single-step tasks alongside multi-step workflows

Most useful: queries where the skill would help but the connection isn't obvious from the query alone.

### Should-not-trigger queries

Most valuable: **near-misses** — queries sharing keywords/concepts with your skill but needing something different.

Weak negatives: obviously irrelevant queries (test nothing).
Strong negatives: share domain concepts but need a different capability.

### Tips for realism

Include: file paths, personal context ("my manager asked me to..."), specific details, casual language, abbreviations, typos.

## Testing whether a description triggers

Run each query through the agent with the skill installed. The skill triggered if the agent loaded SKILL.md; it didn't trigger if the agent proceeded without it.

A query passes if:
- `should_trigger: true` and skill was invoked, OR
- `should_trigger: false` and skill was NOT invoked

### Running multiple times

Model behavior is nondeterministic. Run each query 3+ times, compute trigger rate. Should-trigger passes if rate > 0.5. Should-not-trigger passes if rate < 0.5.

## Avoiding overfitting

Split query set:
- **Train set (~60%)**: guides improvements
- **Validation set (~40%)**: checks whether improvements generalize

Both sets need proportional mix of positive/negative queries. Keep split fixed across iterations.

## The optimization loop

1. Evaluate current description on train and validation sets
2. Identify train set failures (only use train failures to guide changes)
3. Revise description:
   - Should-trigger failing → description too narrow. Broaden scope.
   - Should-not-trigger failing → description too broad. Add specificity about what skill does NOT do.
   - Don't add specific keywords from failed queries (overfitting). Find the general category instead.
   - If stuck, try structurally different approach rather than incremental tweaks.
   - Check description stays under 1024 chars (they tend to grow).
4. Repeat until train set passes or no meaningful improvement
5. Select best iteration by validation pass rate (may not be the last one)

Five iterations is usually enough. If not improving, issue may be with queries rather than description.

## Applying the result

1. Update `description` field in SKILL.md frontmatter
2. Verify under 1024-character limit
3. Verify triggers as expected — try fresh queries (never part of optimization) for honest generalization check
