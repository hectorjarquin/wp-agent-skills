# Change Log

## 2026-11-19

- Extracted the seven documentation-engineering skills to a separate framework-agnostic bundle: `wp-requirements-specification`, `wp-architecture-description`, `wp-qa-testing`, `wp-ua-testing`, `wp-developer-documentation`, `wp-user-documentation`, `wp-public-documentation`
- Kept `wp-coding-standards`, `wp-plugin-directory-compliance`, `wp-site-inventory` (irreducibly WordPress-specific)
- Updated `index.md` and root `README.md` to reflect the reduced bundle

## 2026-10-13

- Added `target`/`rel` link guidance to `wp-coding-standards/references/html.md` (safelist + `esc_attr()`, optional `rel="noopener"`)

## 2026-09-22

- Removed the `wp-image-to-blocks` skill and its `references/`
- Dropped the `wp-blockmarkup` MCP dependency and related README guidance
- De-referenced `wp-image-to-blocks` from `index.md` and cross-skill "Do not use" lists

## 2026-06-27

- Published initial bundle with 11 WordPress agent skills
- Enriched all concept files with OKF v0.1 frontmatter (`type`, `tags`, `timestamp`, `resource`, `dependencies`)
- Added `index.md` for progressive disclosure
- Added Compend MCP integration for agent-side discovery
