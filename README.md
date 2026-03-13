# wp-agent-skills

**Custom WordPress agent skills for GitHub Copilot and other instruction-aware assistants.**

This repository complements the official WordPress Agent Skills project (`https://github.com/WordPress/agent-skills`) with custom, project-specific workflows and policies. It is independently maintained and intended to extend upstream skills, not replace them.

## Why This Repo?

Use this repo to:

- version custom skill policies in Git
- maintain team-specific workflows separately from upstream
- distribute a consistent skill set across projects and environments
- layer custom behavior on top of official WordPress agent skills

## Available Skills

| Skill | What it covers |
|---|---|
| `wp-coding-standards` | WordPress coding standards for PHP, JS, CSS, and HTML |
| `wp-plugin-directory-compliance` | WordPress.org plugin submission and compliance checks |
| `wp-image-to-blocks` | Screenshot/design-to-Gutenberg conversion workflow |
| `wp-site-inventory` | Site inventory and structure analysis workflow |

## Quick Start

### Install skills into your project

Copy skills from this repository's `skills/` directory into your assistant target directory inside your project.

Target directories:

- `.codex/skills/` for OpenAI Codex
- `.github/skills/` for VS Code / GitHub Copilot
- `.claude/skills/` for Claude Code (project-level)
- `.cursor/skills/` for Cursor (project-level)

Copy these skill folders into your selected target directory:

- `wp-coding-standards`
- `wp-plugin-directory-compliance`
- `wp-image-to-blocks`
- `wp-site-inventory`

Updating follows the same process: overwrite or replace those folders in your target directory.

Example (Copilot target only):

```bash
cp -R skills/wp-coding-standards <project>/.github/skills/
```

### Install MCP dependency for image-to-block workflows

`wp-image-to-blocks` depends on `wp-blockmarkup` MCP for high-confidence, production-ready block output.

- Package: https://www.npmjs.com/package/wp-blockmarkup-mcp
- If this MCP is not installed/available, treat generated block markup as draft and validate before publishing.

## Configure Copilot Instructions

Add the policy blocks below to your project's `.github/copilot-instructions.md` so the assistant consistently routes to these skills.

### Add `wp-image-to-blocks` policy

```markdown
### Skill Invocation Reliability
- For screenshot/mockup-to-Gutenberg requests (for example: "convert image/screenshot/design to blocks"), always invoke the `wp-image-to-blocks` skill first.
- Do not answer screenshot-to-block requests from generic memory alone; follow the skill workflow explicitly.
- When `wp-blockmarkup` MCP tools are available, use MCP-first validation for generated markup:
  1. `search_blocks`
  2. `get_block_schema` (and `list_block_attributes` when needed)
  3. Generate markup
  4. `validate_markup`
  5. Iterate until valid
- Keep local delta policies from `wp-image-to-blocks` references (class minimalism, `core/image` wrapper constraints, `core/button` parity, single canonical output).
```

### Add standards/compliance policy

```markdown
## WordPress Standards and Compliance

All code must adhere to official WordPress coding standards and WordPress.org plugin review requirements.

- For general coding conventions and implementation workflows, use the official WordPress skills in `.github/skills/`.
- For all code generation (PHP/JS/CSS/HTML), apply the `wp-coding-standards` skill to enforce WordPress Coding Standards.
- For plugin submission/review compliance checks, use the `wp-plugin-directory-compliance` skill.
- Canonical checklist reference: `.github/skills/wp-plugin-directory-compliance/references/plugin-directory-requirements.md`.
```

## How This Repo Is Organized

Each skill lives under `skills/<skill-name>/` and typically includes:

```text
skills/<skill-name>/
├── SKILL.md
├── references/
└── scripts/ (optional)
```

## Compatibility

- WordPress-focused workflows
- Designed for instruction-aware assistants (Copilot and similar tools)

## Contributing

Use one repository for all skills.

- Do not create nested `.git` folders inside individual skills.
- Keep each skill in `skills/<skill-name>`.

## License

GPL-2.0-or-later
