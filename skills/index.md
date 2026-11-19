# WordPress Agent Skills

A collection of WordPress agent skills for instruction-aware assistants.

The generic documentation-engineering skills (requirements specification, architecture description, QA/UA testing, developer/user/public documentation) have been extracted into a separate framework-agnostic bundle and are no longer in this repository. This bundle keeps only the irreducibly WordPress-specific skills.

- [Coding Standards](./wp-coding-standards/SKILL.md) — WordPress coding standards for PHP, JS, CSS, and HTML
- [Plugin Directory Compliance](./wp-plugin-directory-compliance/SKILL.md) — WordPress.org plugin submission and compliance checks
- [Site Inventory](./wp-site-inventory/SKILL.md) — Site structure analysis and inventory workflow

## Convention

OKF v0.1. Each `SKILL.md` carries YAML frontmatter with `type`, `tags`, `timestamp`, `resource`, and optional `dependencies`.

## Related

- [Open Knowledge Format v0.1](https://cloud.google.com/blog/products/data-analytics/how-the-open-knowledge-format-can-improve-data-sharing)
- [WordPress Agent Skills](https://github.com/WordPress/agent-skills)
