---
name: wp-plugin-directory-compliance
description: "Use when reviewing WordPress plugins for WordPress.org directory submission readiness, including plugin-check blocking items, disclosure/readme requirements, licensing, and submission-specific security/compliance rules."
compatibility: "Targets WordPress plugin repos intended for WordPress.org distribution."
---

# WP Plugin Directory Compliance

## When to use

Use this skill when:

- preparing a plugin for WordPress.org submission
- running a release gate before packaging a public plugin ZIP
- auditing an existing plugin against directory-specific requirements
- validating readme/disclosure/licensing compliance beyond generic coding standards

Do **not** use this skill for:

- general plugin architecture/refactoring work (use `wp-plugin-development`)
- Gutenberg block implementation details (use `wp-block-development`)
- broad coding standards only (use existing WordPress coding skills)

## Inputs required

- Plugin slug and main plugin file path (if known)
- Plugin readme path (`readme.txt`)
- Intended external services/APIs used by the plugin (if any)

## Procedure

1. Load the authoritative checklist:
   - `./references/plugin-directory-requirements.md`
2. Run a compliance gate with severity buckets:
   - `blocking issues` (must fix before submission)
   - `warnings` (fix or explicitly justify)
   - `advisories` (best-practice improvements)
3. Capture evidence per finding:
   - file path + line reference
   - short reason tied to requirement
   - concrete fix recommendation
4. Confirm release-facing artifacts:
   - plugin header fields
   - `readme.txt` validator readiness
   - licensing and third-party compatibility notes

## Verification

Before marking compliant:

- No unresolved blocking issues remain.
- Every warning is either fixed or justified with a clear rationale.
- External service disclosure text is present when applicable.
- Text domain and slug alignment are verified.
- Required direct-file-access guards are present in PHP entry files.

## Output format

Return a concise report with:

1. **Blocking issues**
2. **Warnings**
3. **Advisories**
4. **Pass/Fail summary**
5. **Next fixes to apply first**

## Reference

- `./references/plugin-directory-requirements.md`
