---
name: wp-public-documentation
description: "Use when completed WordPress software needs public-facing, publication-ready documentation. Transforms upstream artifacts (SRS, architecture, developer docs, user docs) into audience-appropriate content suitable for any output format (website, PDF, help center, printed). Aligns to ISO/IEC/IEEE 26514:2022 (Information for Use — Task-based). Use after wp-user-documentation when producing content for external publication."
compatibility: "Targets WordPress 6.9+ (PHP 7.4+). Applies to plugins, themes, blocks, and custom features with stable upstream documentation."
type: skill
tags: [wordpress, documentation, public, publication]
timestamp: 2026-06-27T00:00:00Z
resource: ./references/
---

# WordPress Public Documentation

## When to use

Use this skill after upstream documentation artifacts exist and content needs to be prepared for public consumption:

- Converting SRS, architecture, developer docs, or internal user docs into publication-ready content
- Producing documentation for a public docs site, knowledge base, PDF, or help center
- Features need audience-appropriate framing for external consumers (end users, administrators, AI agents)
- 20+ features need consistent documentation output with editorial oversight
- Content needs editorial metadata (section count, word count, page split, image requirements) for planning

**Timing:** After upstream docs (SRS, dev-doc, user-doc) are stable. Before publication and review.

## Purpose

This skill produces publication-ready documentation content from upstream technical and user-facing artifacts.

**Unlike the SRS:** The SRS specifies *what* must be built. This skill produces content for public consumption.

**Unlike developer documentation:** This is for public readers — end users, site administrators, evaluators — not developers.

**Unlike user documentation:** User documentation targets internal operators with procedural steps. Public documentation targets external readers with audience-appropriate framing, editorial structure, and format-agnostic output.

## Profile Selection

This skill is self-contained and must apply profile behavior without requiring any other workflow skill or repository-level policy file.

- **Default**: produce the core content set with editorial summary and traceability; ready for single-review cycle
- **Team**: add explicit reviewer assignments, rollout context, and collaborative draft workflow
- **Enterprise**: add controlled document metadata, approval gates, audit-ready traceability records, and sign-off requirements

Unless the user or organization asks for a stricter mode, use **Default**.

## Profile Application Rules

### Default

- Produce a single page per feature unless the content exceeds 8 H2 sections or 2,000 words — then split into overview + content pages.
- Include editorial summary after the title using bracket notation: `[Editorial: Feature: ... | Pages: ... | ...]`
- Traceability: link SRS FR-XX references inline.
- No separate approval records unless requested.

### Team

- Add explicit reviewer names and expected review timeline.
- Record audience analysis, onboarding notes, and support impact assessment.
- Include editorial review checklist with pass/fail tracking.
- Document the split decision rationale when multi-page output is chosen.

### Enterprise

- Add controlled document metadata (document ID, version, status, approval date).
- Include formal approval gates with sign-off fields per stakeholder role.
- Add audit-ready traceability records linking each section to its upstream SRS requirement.
- Record change history and release-control expectations explicitly.

## Portability Rule

If this skill is used on its own, it must still select and apply the correct profile using the rules in this file alone.

## What this skill produces

- **Editorial summary line** (right after the title, wrapped in brackets)
  - Feature name, page count, total word count, section counts (H2/H3/H4), screenshot placeholder count
  - Doc category assignment, target audience badge

- **Purpose paragraph** (mandatory, first section)
  - Names the audience, the goal, and the mechanism: "This feature enables [audience] to [accomplish what] by [how it works]."

- **Structured H2 sections**
  - Each H2 is a standalone, scannable TOC entry
  - Minimum 2 sections, maximum 8 per page
  - H3 subsections for detailed content under each H2

- **Image placeholders**
  - `<!-- IMAGE: what this screenshot should show -->` inline at relevant points
  - Counted in the editorial summary

- **In this page table of contents** (right after the overview section, listing all H2-H4 headings as linked anchor list)
  - Block markup: every TOC list item must use `wp:list-item` wrappers (see template Block markup reference)

- **Next steps** (mandatory, final section)
  - Local links to other sections within the same feature
  - Global links to related features in the same doc category

- **Multi-page split** (when content exceeds single-page threshold)
  - Page 1: Title, purpose, table of contents (full H2→H3→H4 tree), next steps
  - Page 2+: Title matching H2, content sections, next steps per page

## Inputs required

- Upstream SRS (strongly preferred) — provides requirement IDs and functional scope
- Developer documentation (strongly preferred) — provides technical accuracy
- User documentation (optional) — provides existing procedural content to adapt
- Architecture description (optional) — provides system context for framing
- Feature slug for artifact naming — normally derived from the project folder or canonical package name (for example `<feature-slug>`)
- Doc category path — for navigation hierarchy (for example `docs/category/ai-features`)
- Admin UI path — for including concrete location references (for example "Settings → AI → Connections")
- Target audience designation — who will read this (end user, administrator, evaluator, AI agent consumer)

## Procedure

### 1. Identify the target audience

Determine who will consume this documentation:

- **End users**: task-focused, minimal configuration, emphasis on "what can I do with this?"
- **Site administrators**: configuration details, prerequisites, permission requirements
- **Evaluators**: emphasis on "why should I use this?", benefits, outcomes
- **AI agents / automation consumers**: only relevant when the feature exposes programmatic contracts; include schema references and discovery endpoints

Keep one document focused on one primary audience. Different audiences get separate documents.

### 2. Extract the purpose statement

Write one sentence that names:
- Audience: "Site administrators"
- Goal: "configure AI data sources"
- Mechanism: "by connecting to PostgreSQL databases and Supabase-style REST APIs"

Template: "This feature enables **[audience]** to **[goal]** by **[mechanism]**."

### 3. Review upstream artifacts

- Locate the SRS FR-XX items that correspond to visible user outcomes
- Locate the developer documentation for technical reference
- Note admin UI paths from the implementation
- Identify prerequisite features or dependencies

### 4. Draft sections using the template

Use `references/public-documentation-template.md`:

- Title followed by editorial summary
- Purpose paragraph
- H2 sections (2–8, each scannable as a TOC entry)
- H3 subsections within H2 sections as needed
- Image placeholders at relevant points
- Next steps section
- In this page table of contents listing all H2-H4 headings with anchor links, placed after the overview section

**Heading discipline:** Every H2 text must be readable as a standalone TOC entry. A reader scanning the "On this page" sidebar should understand the page from headings alone. Use hyphens (-) for list separators in body text. Avoid em dashes (--). Set explicit anchor values on headings to avoid collisions from duplicate names.

### 5. Determine page split

Evaluate whether content needs multiple pages:

- **Single page**: 8 or fewer H2 sections, under 2,000 words, one logical topic
- **Multi-page**: exceeds thresholds, or sections are sufficiently independent that separate pages improve navigation

For multi-page output:
- Page 1: Title, purpose paragraph, full table of contents (all H2→H3→H4), next steps
- Subsequent pages: Title matching the H2 section, content, next steps with links to other pages

Name files using the feature slug:
- Single page: `<feature-slug>-public-documentation.md`
- Multi-page: `<feature-slug>-public-documentation-overview.md`, `<feature-slug>-public-documentation-<section>.md`

### 6. Add editorial summary

Place the editorial summary as an inline line right after the title, wrapped in brackets:

```
[Editorial: Feature: [name] | Pages: [count] | Total words: [count] | Sections: [count] (H2: X, H3: Y, H4: Z) | Screenshots needed: [count] | Doc category: [path] | Audience: [role]]
```

This lets editors assess scope before review. Use pipe separators between fields.

### 7. Add next steps

End every page with a "Next steps" section containing:
- **Local** — links to other sections within the same feature page
- **Global** — links to other features in the same doc category

### 8. Cross-reference requirements

Add inline traceability:

- `[SRS: FR-XX]` at the end of key sections
- If no SRS exists, document assumptions from verified behavior

### 9. Hand off

When complete:
- Headings are scannable as standalone TOC entries
- Purpose paragraph names audience, goal, and mechanism
- Image placeholders are placed at relevant points
- Next steps are present on every page
- Editorial summary is present and accurate
- In this page TOC is present and lists every H2-H4 heading with correct anchor links
- No internal "TODO" or "FIXME" markers remain
- SRS traceability IDs are present where applicable

Profile note:
- **Default** produces core content with editorial summary and traceability.
- **Team** includes reviewer assignments and collaborative notes.
- **Enterprise** includes formal approval fields and audit records.

## Do not use this skill for

- Internal operator or administrator task guides (use `wp-user-documentation`)
- Developer API or code integration documentation (use `wp-developer-documentation`)
- General WordPress Coding Standards (use `wp-coding-standards`)
- Test case specification (use `wp-qa-testing` or `wp-ua-testing`)
- Technical requirements or architecture (use `wp-requirements-specification`)
- Site inventory or structure analysis (use `wp-site-inventory`)

## Standards & Traceability

**Standard:** ISO/IEC/IEEE 26514:2022 — "Design and development of information for users based on common industry specifications and standards"

**Upstream Sources:**
- Software Requirements Specification (SRS) from `wp-requirements-specification` skill as the canonical upstream handoff artifact
- Stakeholder Requirements Specification (StRS) from `wp-requirements-specification` skill when business-context traceability is available
- Operational Concept (OpsCon) from `wp-requirements-specification` skill when workflow-context traceability is available

**Downstream Relationship:**
- `wp-public-documentation` is a downstream consumer of `wp-user-documentation` and `wp-developer-documentation`
- `wp-public-documentation` does not replace `wp-user-documentation` — each targets a different audience
- `wp-public-documentation` is format-agnostic: output can be published as web content, PDF, help center articles, or printed documentation

**Traceability Rule:**
- Every H2 section should reference the SRS Functional Requirement (FR-XX) it operationalizes
- When available, reference the StRS Stakeholder Need (SN-XX) for business context
- Format: `[SRS: FR-XX]`, `[StRS: SN-XX]`, or `[OpsCon: OPS-XX]`
- If no SRS exists, document assumptions in an Assumptions section

**Quality Criteria:**
- Purpose paragraph correctly identifies audience, goal, and mechanism
- H2 headings are scannable as standalone TOC entries
- Editorial summary block is present and accurate
- Image placeholders are placed at relevant points
- Next steps link to both local and global content
- In this page TOC accurately reflects all H2-H4 headings
- Content reads naturally for the target audience
- No internal references (class names, hooks, curl commands) visible to public readers

## Example outputs

See `example/` subdirectory for sample public documentation outputs generated using this skill.
