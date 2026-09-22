# wp-agent-skills

**Custom WordPress agent skills for GitHub Copilot and other instruction-aware assistants.**

This repository complements the official WordPress Agent Skills project ([github.com/WordPress/agent-skills](https://github.com/WordPress/agent-skills)) with custom, project-specific workflows and policies. It is independently maintained and intended to extend upstream skills, not replace them.

## Why This Repo?

This repository is the distribution layer for a practical WordPress agent workflow.

It is built around three capabilities:

- Standards and compliance guardrails: enforce WordPress coding and plugin-review expectations (`wp-coding-standards`, `wp-plugin-directory-compliance`).
- Delivery workflow orchestration: run a consistent requirements-to-documentation path with SRS-first handoffs (`wp-requirements-specification`, `wp-architecture-description`, `wp-qa-testing`, `wp-ua-testing`, `wp-developer-documentation`, `wp-user-documentation`, `wp-public-documentation`).
- Implementation acceleration and discovery: support production tasks such as existing-site analysis (`wp-site-inventory`).

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
| `wp-requirements-specification` | SRS-first pre-implementation specification workflow with optional BRS/StRS/OpsCon/SyRS enrichments |
| `wp-architecture-description` | Architecture description workflow with SRS-first traceability and ADR coverage |
| `wp-qa-testing` | QA strategy, planning, procedures, and requirement-linked test coverage |
| `wp-ua-testing` | Stakeholder acceptance testing with SRS-first traceability and release decision evidence |
| `wp-developer-documentation` | Developer-facing API/integration documentation with requirement traceability |
| `wp-user-documentation` | End-user/operator documentation with workflow and requirement mapping |
| `wp-public-documentation` | Public-facing documentation for external publication (web, PDF, help center) |
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
- `wp-requirements-specification`
- `wp-architecture-description`
- `wp-qa-testing`
- `wp-ua-testing`
- `wp-developer-documentation`
- `wp-user-documentation`
- `wp-public-documentation`
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

### Add WordPress Delivery Workflow

```markdown
## WordPress Delivery Workflow

Use these policy rules for WordPress delivery skills:

1. Skills are independently invocable by default.
2. Before using any WordPress delivery workflow skill, select the WordPress delivery profile.
  Use `Default` unless the user or organization explicitly requires `Team` or `Enterprise` controls.
  Apply the selected profile consistently across any workflow skills used in the request.
  Each workflow skill must carry and apply its own profile rules so it remains self-contained when reused independently.
3. Multi-step sequence is optional and user-directed.
  Only run a full requirements -> architecture -> implementation -> documentation/testing flow when the user explicitly asks for end-to-end workflow support.
4. For downstream artifacts, users may provide prior files as context.
  Example: based on an SRS or architecture file and a selected profile, generate QA, UAT, developer docs, or user docs.
5. Use the SRS as the default downstream handoff artifact.
  Treat BRS, StRS, OpsCon, and SyRS as optional enrichments unless a downstream skill explicitly needs them for stronger business or system traceability.
  If UAT runs without stakeholder-level artifacts, record the resulting acceptance assumptions explicitly.
6. Do not generate all delivery artifacts unless explicitly requested by the user.

If the user explicitly asks for a full sequence, use this default order:

1. Requirements first: use `wp-requirements-specification` to define and validate requirement IDs, with SRS as the default downstream handoff artifact.
2. Architecture second: use `wp-architecture-description` for architecture views, ADRs, constraints, risks, and mandatory traceability to requirements.
3. Implementation third: route to implementation skills (`wp-plugin-development`, `wp-rest-api`, `wp-block-development`, `wp-block-themes`, `wp-interactivity-api`) after architecture description is complete.
4. QA planning during implementation planning: use `wp-qa-testing` before or early during development.
5. After implementation reaches stable state, generate downstream artifacts as requested (`wp-developer-documentation`, `wp-user-documentation`, `wp-public-documentation`, `wp-qa-testing` execution evidence updates, `wp-ua-testing`).
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
