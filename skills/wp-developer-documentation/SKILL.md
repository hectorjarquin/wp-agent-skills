---
name: wp-developer-documentation
description: "Use when completed WordPress software needs developer-facing API, integration, and extension documentation. Produces class references, API guides, integration instructions, code examples, and troubleshooting for developers and integrators. Aligns to ISO/IEC/IEEE 26514:2022 (Information for Use — Developer Reference). Use after wp-block-development, wp-plugin-development, or wp-rest-api implementation is complete."
compatibility: "Targets WordPress 6.9+ (PHP 7.4+). Applies to plugins, themes, blocks, and custom classes."
---

# WordPress Developer Documentation

## When to use

Use this skill after software implementation is complete and documented in code:

- Custom class or library needs developer integration guide
- Plugin exposes public API or hooks that developers must understand
- REST API endpoints need integration documentation
- Block or theme extends WordPress in non-obvious ways
- Other developers will maintain, extend, or integrate the code
- Internal patterns or extension points need to be communicated

**Timing:** After implementation is complete and code is stable. Not during active development.

## Purpose

This skill produces developer-facing reference documentation that explains how other developers should understand, use, and extend the completed software.

**Unlike the SRS:** The SRS specifies *what* must be built. This skill documents *how developers use it*.

**Unlike general WordPress documentation:** This is specific to your custom implementation, not WordPress core features.

## Profile Selection

This skill is self-contained and must apply profile behavior without requiring any other workflow skill or repository-level policy file.

- **Default**: use the lightest developer documentation set that still supports production delivery
- **Team**: add explicit collaboration structure, reviewer ownership, and handoff context
- **Enterprise**: add controlled documentation metadata, approval records, and audit-ready detail

Unless the user or organization asks for a stricter mode, use **Default**.

## Profile Application Rules

### Default

- Favor consolidated reference material with concise coverage notes.
- Keep examples, integration guidance, and troubleshooting practical rather than process-heavy.
- Do not require separate approval records unless requested.

### Team

- Add explicit reviewer ownership and handoff context for maintainers or integrators.
- Record rollout or extension concerns that matter to shared ownership.
- Keep documentation structure predictable for collaborative maintenance.

### Enterprise

- Add controlled document metadata, approval status, and audit-ready detail where required.
- Record formal ownership and change-control expectations explicitly.
- Produce any required controlled reference artifacts for regulated or contractual delivery.

## Portability Rule

If this skill is used on its own, it must still select and apply the correct profile using the rules in this file alone.

## What this skill produces

- **Overview/Architecture Summary**
  - What problem does this solve?
  - Intended use cases
  - Design philosophy and assumptions
  - Any breaking changes or version notes

- **API/Class Reference**
  - Public methods and functions
  - Parameters (type, required/optional, default values)
  - Return values (type, structure)
  - Exceptions or errors
  - PHPDoc or JSDoc extracted and curated

- **Integration Guide**
  - How to instantiate or register the class/component
  - Hook names and filter names
  - Configuration options
  - Required dependencies
  - Environment or version requirements

- **Code Examples**
  - Minimal working examples (real code, not pseudocode)
  - Common patterns
  - Anti-patterns to avoid
  - Real-world use cases

- **Extension Points**
  - Hooks and filters provided
  - Filters for extensibility
  - How to subclass or override behavior
  - Plugin integration patterns

- **Troubleshooting**
  - Common errors and how to resolve them
  - Performance considerations
  - Known limitations
  - Debugging strategies

## Inputs required

- Completed, stable codebase (production-ready code or very close)
- Source files with existing PHPDoc/JSDoc comments
- Related SRS (Software Requirements Specification) — optional but strongly preferred
- WordPress project context (theme, plugin, standalone block, etc.)
- Feature slug for artifact naming, normally derived from the project folder or canonical package name (for example `<feature-slug>`)

## Procedure

### 1. Classify the documentation scope

Determine what needs documentation:

- Is this a **class**? (Include full constructor, public methods, properties)
- Is this an **API or set of functions**? (Include all public entry points)
- Is this a **WordPress REST endpoint**? (Include routes, schema, authentication)
- Is this a **Gutenberg block**? (Include attributes, deprecations, extensions)
- Is this a **theme or theme extension**? (Include filters, setup steps, customization points)

Pick one primary scope. Keep this skill focused.

### 2. Extract SRS requirement IDs (if available)

If your code came from an SRS:

- Locate the Software Requirements Specification (SRS)
- Find any requirement IDs (FR-XX namespace) that describe this component
- Note the mapping: "This class implements FR-18 and FR-19"
- Add these IDs to your documentation so readers understand the **what** (SRS) vs. the **how** (this doc)

### 3. Review source code comments

- Extract all existing PHPDoc / JSDoc comments from the source
- Identify gaps: methods without documentation
- Prepare inline examples from actual usage

### 4. Draft using the reference template

Use `references/developer-documentation-template.md`:

- Overview
- Installation/Setup
- API Reference
- Examples
- Extension Points
- Troubleshooting
- Traceability (SRS mappings)

Name the generated document using the package feature slug.

- Default filename pattern: `<feature-slug>-developer-documentation.md`
- Generic examples: `example/feature-slug-developer-documentation-default.md`, `example/feature-slug-developer-documentation-team.md`, `example/feature-slug-developer-documentation-enterprise.md`
- If you split output into multiple developer docs, keep the same slug prefix on every file.

Replace all `[PLACEHOLDER]` sections with your content.

### 5. Validate with real code

Every code example must:

- Be extracted from or validated against actual source code
- Include comments explaining non-obvious lines
- Show the expected output or outcome
- Include any required setup

Do not use hypothetical or pseudocode examples.

### 6. Cross-reference requirements (if available)

For each major section, add inline references:

- "This hook implements FR-18: Yoast detection"
- "This method supports FR-20: Extensibility"

Format: `[SRS: FR-XX]` at the end of the key paragraph.

### 7. Hand off

When complete:
- Artifact is standalone and self-contained
- No internal "TODO" or "FIXME" markers
- All examples have been tested against actual code
- SRS traceability IDs are present (if available)

Ready for developers to use immediately.

Profile note:
- **Default** favors a consolidated implementation reference with verified examples.
- **Team** adds explicit reviewer ownership and handoff notes.
- **Enterprise** may require controlled document metadata and formal approval records.

## Do not use this skill for

- General WordPress Coding Standards (use `wp-coding-standards`)
- Block markup generation or design (use `wp-image-to-blocks`)
- End-user/operator documentation (use `wp-user-documentation`)
- Test case specification (use `wp-qa-testing` or `wp-ua-testing`)
- Architecture decisions that belong in the SRS (use `wp-requirements-specification`)

## Standards & Traceability

**Standard:** ISO/IEC/IEEE 26514:2022 — "Design and development of information for users based on common industry specifications and standards"

**Upstream Source:** Software Requirements Specification (SRS) from `wp-requirements-specification` skill

**Traceability Rule:**
- Every major API or extension point should reference the SRS Functional Requirement (FR-XX) it implements
- Format: `[SRS: FR-18]` or "Implements FR-18 (Yoast detection)"
- If no SRS exists, document assumptions in an Assumptions section

**Quality Criteria:**
- Every code example verified against actual source
- Every public method/function documented
- Extension points are discoverable
- Error handling is explained
- Dependencies are listed

## Example outputs

See `example/` subdirectory for sample developer documentation outputs generated using this skill.
