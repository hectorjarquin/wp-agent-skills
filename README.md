# wp-agent-skills

**Custom WordPress agent skills for GitHub Copilot and other instruction-aware assistants.**

This repository complements the official WordPress Agent Skills project ([github.com/WordPress/agent-skills](https://github.com/WordPress/agent-skills)) with custom, project-specific workflows and policies. It is independently maintained and intended to extend upstream skills, not replace them.

## Why This Repo?

This repository is the distribution layer for a practical WordPress agent workflow.

It is built around two capabilities:

- Standards and compliance guardrails: enforce WordPress coding and plugin-review expectations (`wp-coding-standards`, `wp-plugin-directory-compliance`).
- Implementation acceleration and discovery: support production tasks such as existing-site analysis (`wp-site-inventory`).

The documentation-engineering skills (requirements specification, architecture description, QA/UA testing, developer/user/public documentation) were extracted into a separate framework-agnostic bundle (2026-11-19) and are no longer distributed here.

Use this repo to:

- version custom skill policies in Git
- keep team-specific workflow decisions independent from upstream defaults
- distribute a repeatable skill set across projects and environments
- extend official WordPress agent skills with local operational conventions

## OKF v0.1 Format

All skills use [Open Knowledge Format v0.1](https://cloud.google.com/blog/products/data-analytics/how-the-open-knowledge-format-can-improve-data-sharing) frontmatter for machine discovery. OKF fields are additive alongside standard agent frontmatter (`name`, `description`, `compatibility`):

```yaml
type: skill
tags: [wordpress, domain, keywords]
timestamp: 2026-06-27T00:00:00Z
resource: ./references/
```

## Available Skills

| Skill | What it covers |
|---|---|
| `wp-coding-standards` | WordPress coding standards for PHP, JS, CSS, and HTML |
| `wp-plugin-directory-compliance` | WordPress.org plugin submission and compliance checks |
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
- `wp-site-inventory`

Updating follows the same process: overwrite or replace those folders in your target directory.

Example (Copilot target only):

```bash
cp -R skills/wp-coding-standards <project>/.github/skills/
```

## Configure Copilot Instructions

Add the policy blocks below to your project's `.github/copilot-instructions.md` so the assistant consistently routes to these skills.

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
