# Evaluating Skill Output Quality

Source: https://agentskills.io/skill-creation/evaluating-skills

## Evaluation cases

Store conventional cases in `evals/evals.json` with:

- A realistic prompt
- The expected output or outcome
- Optional input files

Start with two or three varied cases, including at least one edge case. Expand the set from real failures rather than speculative coverage.

## Comparison method

Run each case in a clean context:

1. With the skill
2. Without the skill, or with the previous skill version

Use identical inputs and compare both output quality and execution behavior.

## Assertions and evidence

- Inspect initial outputs before defining objective assertions.
- Base PASS grades on concrete evidence, not impressions.
- Review execution traces for wasted work, ignored instructions, retries, and accidental dependencies.
- Include human review where subjective quality matters.

## Cost and iteration

Compare quality gains against token use and execution time. Revise the skill, rerun the cases, and continue until feedback is consistently empty or improvement plateaus.
