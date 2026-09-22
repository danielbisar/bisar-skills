# Using Scripts in Skills

Source: https://agentskills.io/skill-creation/using-scripts

## When scripts help

Move complex commands or repeatedly reinvented logic into tested scripts. Use paths relative to the skill root so the skill remains portable.

## Dependencies and compatibility

- Pin versions in one-off package execution commands.
- Declare required runtimes, tools, system packages, or network access in the `compatibility` frontmatter field.
- Avoid hidden assumptions about the current working directory or globally installed dependencies.

## Interface requirements

- Scripts must not require interactive prompts.
- Provide concise `--help` output.
- Report actionable errors that explain what failed and how to correct it.
- Use meaningful exit codes.
- Write structured result data to stdout and diagnostics to stderr.

## Safety and repeatability

- Prefer idempotent operations.
- Use safe defaults.
- Support dry-run behavior for consequential changes.
- Validate inputs before making changes.

## Context-efficient output

Prevent scripts from flooding the agent context:

- Summarize large results.
- Support filtering or pagination.
- Write large artifacts to an explicit output path and print that path.
