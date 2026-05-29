# Agent Skills Specification

Source: https://agentskills.io/specification

## Directory structure

```
skill-name/
├── SKILL.md          # Required: metadata + instructions
├── scripts/          # Optional: executable code
├── references/       # Optional: documentation
├── assets/           # Optional: templates, resources
└── ...
```

## SKILL.md format

YAML frontmatter followed by Markdown content.

### Frontmatter fields

| Field | Required | Constraints |
|-------|----------|-------------|
| name | Yes | Max 64 chars. Lowercase letters, numbers, hyphens only. No start/end hyphen. No consecutive hyphens. Must match parent directory name. |
| description | Yes | 1-1024 chars. Non-empty. Describes what the skill does and when to use it. Should include keywords that help agents identify relevant tasks. |
| license | No | License name or reference to a bundled license file. |
| compatibility | No | Max 500 chars. Environment requirements (product, system packages, network access). Most skills don't need this. |
| metadata | No | Map of string keys to string values. Use reasonably unique key names. |
| allowed-tools | No | Space-separated string of pre-approved tools. Experimental. |

### name field rules

- 1-64 characters
- Only lowercase alphanumeric (`a-z`, `0-9`) and hyphens (`-`)
- Must not start or end with a hyphen
- Must not contain consecutive hyphens (`--`)
- Must match the parent directory name

Valid: `pdf-processing`, `data-analysis`, `code-review`
Invalid: `PDF-Processing` (uppercase), `-pdf` (starts with hyphen), `pdf--processing` (consecutive hyphens)

### description field rules

- 1-1024 characters
- Should describe both what the skill does AND when to use it
- Should include specific keywords that help agents identify relevant tasks

Good:
```
Extracts text and tables from PDF files, fills PDF forms, and merges multiple PDFs. Use when working with PDF documents or when the user mentions PDFs, forms, or document extraction.
```

Poor:
```
Helps with PDFs.
```

### Body content

No format restrictions. Write whatever helps agents perform the task effectively.

Recommended sections:
- Step-by-step instructions
- Examples of inputs and outputs
- Common edge cases

The agent loads this entire file once it decides to activate. Consider splitting longer content into referenced files.

## Optional directories

### `scripts/`
Executable code agents can run. Should be self-contained, include helpful error messages, handle edge cases. Common languages: Python, Bash, JavaScript.

### `references/`
Additional documentation loaded on demand. Keep files focused — smaller files mean less context usage.

### `assets/`
Static resources: templates, images, data files, schemas.

## Progressive disclosure

Skills load progressively:
1. **Metadata (~100 tokens)**: `name` and `description` loaded at startup for all skills
2. **Instructions (< 5000 tokens recommended)**: Full `SKILL.md` body loaded when skill activates
3. **Resources (as needed)**: Files in `scripts/`, `references/`, `assets/` loaded only when required

**Keep SKILL.md under 500 lines.** Move detailed reference material to separate files.

## File references

Use relative paths from the skill root. Keep references one level deep. Avoid deeply nested reference chains.

## Validation

```bash
skills-ref validate ./my-skill
```

Checks frontmatter validity and naming conventions.
