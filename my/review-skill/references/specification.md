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
| name | Yes | 1-64 chars. Lowercase `a-z`, `0-9`, hyphens only. No start/end hyphen. No consecutive hyphens. Must match parent directory name. |
| description | Yes | 1-1024 chars. What the skill does and when to use it. Include keywords for agent task matching. |
| license | No | License name or reference to bundled license file. |
| compatibility | No | Max 500 chars. Environment requirements. |
| metadata | No | Map of string keys to string values. |
| allowed-tools | No | Space-separated pre-approved tools. Experimental. |

### name examples

Valid: `pdf-processing`, `data-analysis`, `code-review`
Invalid: `PDF-Processing` (uppercase), `-pdf` (starts with hyphen), `pdf--processing` (consecutive hyphens)

### description guidelines

Good: "Extracts text and tables from PDF files, fills PDF forms, and merges multiple PDFs. Use when working with PDF documents or when the user mentions PDFs, forms, or document extraction."

Poor: "Helps with PDFs."

### Body content

No format restrictions. Write whatever helps agents perform the task. Recommended: step-by-step instructions, I/O examples, common edge cases.

Split longer content into referenced files.

## Optional directories

- **`scripts/`** — Self-contained executable code with helpful error messages
- **`references/`** — Additional docs loaded on demand; keep files focused
- **`assets/`** — Static resources: templates, images, data files, schemas

## Progressive disclosure

1. **Metadata (~100 tokens)**: `name` + `description` loaded at startup for all skills
2. **Instructions (< 5000 tokens recommended)**: Full SKILL.md body on activation
3. **Resources (as needed)**: `scripts/`, `references/`, `assets/` loaded only when required

**Keep SKILL.md under 500 lines.** Move detailed reference material to separate files.

## File references

Use relative paths from skill root. Keep one level deep. Avoid nested reference chains.
