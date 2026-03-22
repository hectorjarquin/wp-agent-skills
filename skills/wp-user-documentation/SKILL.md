---
name: wp-user-documentation
description: "Use when completed WordPress software needs end-user, operator, or administrator documentation. Produces task-based guides, workflows, procedures, FAQs, and troubleshooting for non-technical stakeholders. Aligns to ISO/IEC/IEEE 26514:2022 (Information for Use — Task-based). Use after wp-block-development, wp-plugin-development, or wp-block-themes implementation achieves stable UI."
compatibility: "Targets WordPress 6.9+ (PHP 7.4+). Applies to plugins, themes, blocks, and custom features with user-facing UI."
---

# WordPress User Documentation

## When to use

Use this skill after software implementation achieves stable user interface and workflows:

- Feature needs to be explained to non-technical editors or administrators
- Content managers need to understand how to use a custom block or setting
- Site operators need workflow guidance
- Before training, launch, or production deployment
- When common user questions or support requests need answers
- Admin/settings screens require explanation

**Timing:** After UI is stable and no major workflows are expected to change. Not during active UI development.

## Purpose

This skill produces end-user-facing reference documentation that explains how to *operate* and *use* the completed software from a non-technical perspective.

**Unlike the SRS:** The SRS specifies *what* must be built. This skill shows *how users do their job with it*.

**Unlike developer documentation:** This is for content creators, editors, administrators — not for other developers.

## Profile Selection

This skill is self-contained and must apply profile behavior without requiring any other workflow skill or repository-level policy file.

- **Default**: use the lightest user documentation set that still supports production delivery
- **Team**: add explicit collaboration structure, reviewer ownership, and rollout context
- **Enterprise**: add controlled user documentation metadata, approval records, and audit-ready traceability notes

Unless the user or organization asks for a stricter mode, use **Default**.

## Profile Application Rules

### Default

- Favor concise task-based guidance, workflows, and troubleshooting that users can follow directly.
- Keep coverage notes lightweight and embedded in the main document set.
- Do not require separate approval records unless requested.

### Team

- Add explicit reviewer ownership and rollout or handoff context.
- Record audience, support, and training considerations that affect shared operations.
- Keep the documentation package structured for collaborative maintenance and review.

### Enterprise

- Add controlled document metadata, approval status, and audit-ready traceability notes where required.
- Record formal ownership and release-control expectations explicitly.
- Produce any required controlled user-facing artifacts for regulated or contractual delivery.

## Portability Rule

If this skill is used on its own, it must still select and apply the correct profile using the rules in this file alone.

## What this skill produces

- **User Roles & Prerequisites**
  - Who should use this? (editors, admins, etc.)
  - What permissions are needed?
  - Any training or background knowledge required?
  - Browser/device compatibility

- **Task-Based Procedures**
  - Step-by-step "How do I...?" guides
  - Screenshots (with annotations when complex)
  - Expected outcomes after each step
  - When to do what (decision trees for branching workflows)

- **Workflows & Scenarios**
  - Real-world use cases
  - Multi-step processes (end-to-end)
  - Typical work patterns
  - Role-based workflows (different paths for different users)

- **Common Errors & Troubleshooting**
  - "Why did this happen?"
  - How to fix common problems
  - When to escalate to admin
  - Recovery steps

- **Frequently Asked Questions**
  - What users actually ask (collect from support/feedback)
  - Short answers with links to detailed guides
  - Clarifications on confusing features

- **Tips & Best Practices**
  - Efficiency tips for power users
  - Common mistakes to avoid
  - Performance or quality considerations

- **Glossary** (if needed)
  - Technical terms explained in plain language
  - References to each major concept

## Inputs required

- Completed, stable UI (production-ready or very close)
- Ability to interact with the software (live instance or detailed screenshots)
- Related SRS (Software Requirements Specification) — strongly preferred as the canonical upstream handoff artifact
- Related StRS (Stakeholder Requirements Specification) — optional, enriches business context
- OpsCon (Operational Concepts) — optional, clarifies workflows
- WordPress project context (theme, plugin, block, etc.)
- Feature slug for artifact naming, normally derived from the project folder or canonical package name (for example `<feature-slug>`)

## Procedure

### 1. Identify the user audience

Who will read this documentation?

- **Content Editors:** Focus on how to create/manage content with your feature
- **Site Administrators:** Focus on configuration, settings, permissions, maintenance
- **Marketing Managers:** Focus on business outcomes and what the feature enables
- **General End-Users:** Focus on day-to-day operational tasks

Keep this documentation focused on one primary audience per document. Different roles may need separate guides.

### 2. Collect real workflows

Before writing, observe or interview actual users:

- What tasks do they need to accomplish?
- What confuses them about the current interface?
- What are the common mistakes?
- What questions do they ask repeatedly?
- What outcomes do they care about (not technical details)?

Write down 3–5 typical workflows.

### 3. Review upstream requirements (if available)

If your feature came from an SRS package:

- Locate the Software Requirements Specification (SRS)
- Find the FR-XX items that correspond to visible user behavior, prerequisites, or outcomes
- Locate the Stakeholder Requirements Specification (StRS)
- Locate the Operational Concepts (OpsCon)
- Find stakeholder needs (SN-XX) that this feature addresses
- Find operational scenarios (OPS-XX) that show workflows
- Note these IDs so readers understand the **what** (SRS), the **why** (business need), and the **how** (this guide)

### 4. Draft using the reference template

Use `references/user-documentation-template.md`:

- Overview (what is this feature, why do I care?)
- User Roles (who uses this?)
- Prerequisites (what do I need to know/do first?)
- Task-based procedures (How do I...?)
- Workflows (real multi-step scenarios)
- Common issues (what goes wrong?)
- FAQs (quick answers)
- Traceability (SRS mappings, plus StRS/OpsCon mappings when available)

Name the generated document using the package feature slug.

- Default filename pattern: `<feature-slug>-user-documentation.md`
- Generic examples: `example/feature-slug-user-documentation-default.md`, `example/feature-slug-user-documentation-team.md`, `example/feature-slug-user-documentation-enterprise.md`
- If you split output into multiple user guides, keep the same slug prefix on every file.

Replace all `[PLACEHOLDER]` sections with your content.

### 5. Write for non-technical readers

- Avoid jargon. If you must use technical terms, explain them.
- Use active voice: "Click the button" not "The button may be clicked."
- Use imperative mood: "Create a new page" not "You might want to consider creating a page."
- Short sentences and paragraphs
- One idea per sentence
- Numbered steps for procedures, bullet points for lists

### 6. Include screenshots or visuals (recommended)

- Annotate with text pointers to important elements
- Show the step and the result
- Use tools like Loom, Screenshot annotation, or WordPress Playground
- Include captions explaining what users should see

### 7. Cross-reference requirements (if available)

For workflows and key features, add inline references:

- "This procedure implements SRS FR-18: FAQ block selection and rendering"
- "This procedure supports stakeholder need SN-05: Yoast compatibility"
- "This workflow aligns with operational scenario OPS-06: Yoast-active FAQs"

Format: `[SRS: FR-XX]`, `[StRS: SN-XX]`, or `[OpsCon: OPS-XX]` at the end of key sections.

### 8. Hand off

When complete:
- Documentation reads naturally for non-technical users
- All procedures are tested and verified (you walked through them)
- Screenshots match current UI
- No internal "TODO" or "FIXME" markers
- SRS, StRS, and OpsCon traceability IDs are present (if available)

Ready for users and support teams to use immediately.

Profile note:
- **Default** keeps a single practical guide with verified procedures.
- **Team** adds explicit reviewer and rollout ownership notes.
- **Enterprise** may require controlled document metadata and approval records.

## Do not use this skill for

- Developer API or code integration documentation (use `wp-developer-documentation`)
- General WordPress Coding Standards (use `wp-coding-standards`)
- Block markup generation or design (use `wp-image-to-blocks`)
- Test case specification (use `wp-qa-testing` or `wp-ua-testing`)
- Technical requirements or architecture (use `wp-requirements-specification`)

## Standards & Traceability

**Standard:** ISO/IEC/IEEE 26514:2022 — "Design and development of information for users based on common industry specifications and standards"

**Upstream Sources:**
- Software Requirements Specification (SRS) from `wp-requirements-specification` skill as the canonical upstream handoff artifact
- Stakeholder Requirements Specification (StRS) from `wp-requirements-specification` skill when business-context traceability is available
- Operational Concept (OpsCon) from `wp-requirements-specification` skill when workflow-context traceability is available

**Traceability Rule:**
- Every major workflow should reference the SRS Functional Requirement (FR-XX) it operationalizes
- When available, major workflows should also reference the StRS Stakeholder Need (SN-XX) they support
- When available, operational scenarios should reference the OpsCon scenario (OPS-XX) they demonstrate
- Format: `[SRS: FR-XX]`, `[StRS: SN-XX]`, or `[OpsCon: OPS-XX]`
- If no SRS exists, document assumptions from verified UI behavior in an Assumptions section

**Quality Criteria:**
- Procedures are tested and verified to work
- Language is appropriate for non-technical audience
- Screenshots reflect current UI
- All major user roles are covered
- Common questions are answered

## Example outputs

See `example/` subdirectory for sample user documentation outputs generated using this skill.
