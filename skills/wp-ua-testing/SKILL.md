---
name: wp-ua-testing
description: "Use when WordPress software is ready for stakeholder validation before release or handoff. Produces stakeholder-friendly acceptance criteria, test scenarios, execution evidence, and a release decision summary. Uses SRS as the canonical upstream handoff artifact, with StRS and OpsCon enriching business-fit coverage when available. Aligns to ISO/IEC/IEEE 29119-3:2021 (Test Documentation) and ISO/IEC 25010:2023 (Quality Model)."
compatibility: "Targets WordPress 6.9+ (PHP 7.4+). Applies to plugins, themes, blocks, and features requiring stakeholder validation."
---

# WordPress User Acceptance Testing

## When to use

Use this skill near the end of development, before production release or stakeholder handoff:

- Software is functionally complete and stable
- Internal QA (developer testing) is complete
- Software needs stakeholder validation
- Business outcomes/acceptance criteria need formal documentation
- Go/no-go decision for production release is needed
- Acceptance evidence is required

**Timing:** After internal QA is complete, before production deployment or handoff.

## Purpose

This skill produces stakeholder-facing acceptance validation documentation that proves the software meets business needs and stakeholder expectations.

**Unlike QA Testing:** QA testing verifies *technical correctness* (does it work?). UAT proves *business fitness* (does it solve the problem?).

When StRS or OpsCon are unavailable, this skill can still operate from the SRS, but it must record that acceptance scenarios were derived from software requirements and stakeholder concerns rather than formal stakeholder-needs artifacts.

**Unlike developer documentation:** UAT is for stakeholders and approvers, not developers or operators.

**Unlike user documentation:** UAT validates the software; user documentation teaches how to *use* it after acceptance.

## Profile Selection

This skill is self-contained and must apply profile behavior without requiring any other workflow skill or repository-level policy file.

- **Default**: use the lightest acceptance package that still supports production delivery
- **Team**: add explicit collaboration structure, decision ownership, and release or handoff notes
- **Enterprise**: add formal approval records, controlled evidence, and audit-oriented controls

Unless the user or organization asks for a stricter mode, use **Default**.

## Profile Application Rules

### Default

- Keep acceptance criteria, scenarios, and execution records consolidated where practical.
- Record outcomes clearly without requiring a separate formal approval artifact.
- Use concise release notes when they help stakeholders understand readiness.

### Team

- Add explicit decision-owner identification and readiness or release notes.
- Record stakeholder participation and review outcomes in a structured way.
- Keep acceptance coverage easy to review across shared roles.

### Enterprise

- Add controlled evidence expectations, approval status, and audit-oriented release records.
- Produce formal approval artifacts when policy or audit requirements demand them.
- Record decision authority and retained evidence explicitly.

## Portability Rule

If this skill is used on its own, it must still select and apply the correct profile using the rules in this file alone.

## What this skill produces

- **Acceptance Criteria**
  - Measurable, business-oriented success conditions
  - Linked to SRS requirements first, and to Stakeholder Needs (StRS) or business outcomes (BRS) when available
  - Quality attributes (performance, reliability, usability) with thresholds
  - Example: "FAQ content loads in < 2 seconds on 4G connection"

- **Acceptance Scenarios**
  - Real-world business workflows that stakeholders care about
  - Preconditions (what must be true to start)
  - Steps (simple, business language)
  - Expected outcomes (what success looks like)
  - Linked to Operational Concepts (OpsCon) when available

- **UAT Execution Record**
  - Who tested what, when
  - Evidence collection (screenshots, logs, outcomes)
  - Defect reports (if any)
  - Pass/fail status per scenario
  - Test result date

- **Release Decision**
  - Go/no-go summary
  - Pass/fail status per acceptance scenario
  - Outstanding issues (if any) and severity
  - Notes or conditions for release

## Inputs required

- Software Requirements Specification (SRS) — mandatory
- Stakeholder Requirements Specification (StRS) — strongly recommended
- Operational Concept (OpsCon) — optional, strongly recommended for workflow-heavy features
- Stable build ready for stakeholder review
- Identified stakeholders and approvers
- Feature slug for artifact naming, normally derived from the project folder or canonical package name (for example `<feature-slug>`)

## Procedure

### 1. Identify stakeholders and their concerns

Who must accept this software?

- **Primary Stakeholder:** e.g., content manager, site owner, business sponsor
- **Secondary Stakeholders:** e.g., IT ops, end-users, compliance officer
- **Decision Owner:** e.g., project manager, client contact, executive sponsor

For each: What do they care about? What would make them say "no"?

Examples:
- "It must handle 1000 articles without performance degradation"
- "Editors must be able to set up new FAQ blocks in < 5 minutes"
- "No data loss on plugin deactivation"

Document these as "Stakeholder Concerns."

### 2. Review upstream acceptance context

Start with the SRS, then enrich with stakeholder and operational context when available:

- Locate all relevant Functional Requirements (FR-XX) and quality requirements in the SRS
- If StRS exists, locate all Stakeholder Needs (SN-XX) and link them to candidate acceptance scenarios
- If OpsCon exists, locate all Operational Scenarios (OPS-XX) that shape real-world workflows
- If StRS or OpsCon do not exist, derive candidate acceptance scenarios from SRS behavior plus stakeholder concerns captured in Step 1, and record the missing upstream context in an Assumptions section
- Example: SRS FR-18 through FR-21 plus stakeholder concern "No duplicate schema with Yoast" → UAT scenario "Test FAQ block when Yoast is active"

### 3. Map quality attributes to acceptance criteria

Use ISO 25010 quality characteristics:

| Quality Attribute | Acceptance Criterion Example |
|---|---|
| **Functional Suitability** | All SRS FR are working as described |
| **Performance Efficiency** | Pages load in < 2 seconds; blocks render in < 500ms |
| **Compatibility** | Works with WordPress 6.5+ and Yoast 21.0+ |
| **Usability** | Non-technical editors can operate features with < 5 minutes training |
| **Reliability** | 99% uptime; no data loss on plugin deactivation |
| **Security** | No SQL injection or XSS vulnerabilities; proper capability checks |
| **Maintainability** | Code is documented; developers can add features without breaking existing ones |
| **Portability** | Works on multiple hosting providers; no hardcoded paths |

Define thresholds: "< 2 seconds" is measurable. "Fast" is not.

### 4. Create acceptance scenarios

For each major workflow or stakeholder concern, create a scenario:

**Format:**
- **Scenario ID:** AS-001
- **Title:** "Admin sets up FAQ block with Yoast active"
- **Stakeholder:** Content manager
- **Business Value:** Ensures FAQ content appears in search results without duplicates
- **Maps to:** SRS FR-18 through FR-21; optionally StRS SN-05 and OpsCon OPS-06 when available
- **Preconditions:**
  - WordPress 6.5+
  - Yoast SEO plugin active
  - FAQ content exists
- **Steps:**
  1. Admin navigates to page edit screen
  2. Admin clicks "Add Block"
  3. Admin selects "FAQ"
  4. Admin selects existing FAQ category
  5. Admin publishes page
- **Expected Outcome:**
  - FAQ block displays questions and answers correctly
  - Yoast does not show duplicate schema warning
  - Page loads in < 2 seconds
  - No console errors
- **Acceptance Criteria:**
  - ✓ Block displays without errors
  - ✓ Schema is valid (use Google Rich Results Test)
  - ✓ Page performance meets threshold
  - ✓ Yoast recognizes FAQ without conflict

### 5. Define stakeholder roles and decision ownership

For each scenario or feature area:

- Who is the decision owner?
- What is their authority level (decide alone or escalate)?
- How is the decision recorded (issue comment, document note, release note)?

Document in a RACI matrix if multiple stakeholders:

| Feature | Admin | Editor | Site Owner | Decision Owner |
|---|---|---|---|---|
| FAQ Block | Tests | Uses | Approves | X |
| Yoast Integration | Tests | Tests | Approves | X |

### 6. Create the UAT execution template

Use `references/uat-execution-template.md`:

- Scenario ID & title
- Stakeholder/tester name
- Test date
- Environment (URL, WordPress version, plugins active)
- Steps and actual results (space to write observations)
- Pass/fail
- Evidence collection (screenshot names, logs, etc.)
- Comments/feedback
- Decision note line

Use the same feature slug across all UA artifacts.

- **Default filename (consolidated):** `<feature-slug>-ua-testing.md`
- Generic examples: `example/feature-slug-ua-testing-default.md`, `example/feature-slug-ua-testing-team.md`, `example/feature-slug-ua-testing-enterprise.md`
- Alternative (granular decomposition): `<feature-slug>-uat-plan.md`, `<feature-slug>-uat-execution.md`

### 7. Create the release decision summary

Add a release decision section at the end of the UAT document:

- Overall pass/fail status
- Any outstanding defects and their severity
- Go/no-go recommendation

No formal sign-off template is required. A brief summary section in `<feature-slug>-ua-testing.md` is sufficient.

Profile note:
- **Default** records a concise release decision in the main document.
- **Team** records decision owner, reviewer, and any conditions for release.
- **Enterprise** may require a formal sign-off or approval artifact in addition to the UAT record.

### 8. Document pass/fail criteria for release

**Go (Release Ready):**
- All acceptance scenarios pass
- No critical defects unresolved
- Performance thresholds met

**No-Go (Hold/Rework):**
- Any critical acceptance scenario fails
- Unresolved critical defects
- Quality attributes not met

**Conditional Release:**
- Minor defects logged for post-release fix
- Known limitations noted in the release decision

### 9. Hand off UAT package

When complete, provide:

- UAT document with scenarios, acceptance criteria, results, and release decision
- Environment setup instructions
- Contact/escalation path if issues arise

This package supports stakeholder review and a recorded release decision without requiring formal sign-off artifacts.

## Do not use this skill for

- Internal QA or developer testing (use `wp-qa-testing`)
- General WordPress Coding Standards (use `wp-coding-standards`)
- End-user or developer documentation (use `wp-user-documentation` or `wp-developer-documentation`)
- Block markup generation (use `wp-image-to-blocks`)
- Requirements specification itself (use `wp-requirements-specification`)

## Standards & Traceability

**Standard:** ISO/IEC/IEEE 29119-3:2021 — "Test Documentation"

**Quality Model:** ISO/IEC 25010:2023 — "System and Software Quality Model"

**Upstream Sources:**
- Software Requirements Specification (SRS) from `wp-requirements-specification` skill as the canonical upstream handoff artifact
- Business Requirements Specification (BRS) from `wp-requirements-specification` skill when business-outcome traceability is required
- Stakeholder Requirements Specification (StRS) from `wp-requirements-specification` skill when stakeholder-needs traceability is available
- Operational Concept (OpsCon) from `wp-requirements-specification` skill when operational workflow realism matters

**Traceability Rule:**
- Every acceptance scenario must map to at least one SRS functional or quality requirement
- When StRS or BRS exist, every major acceptance scenario should also map to at least one stakeholder need or business outcome
- When OpsCon exists, workflow-oriented scenarios should reference the relevant operational scenario
- Every acceptance criterion should reference one or more SRS FRs or quality attributes
- Format: "AS-001 validates FR-18 through FR-21"
- Format: "AS-001 also validates SN-05 and OpsCon OPS-06"
- Format: "Performance criterion (< 2 seconds) measures ISO 25010 Responsiveness"
- Mandatory: Link to business value or record the acceptance assumption when no upstream stakeholder artifact exists

**Quality Criteria:**
- Acceptance scenarios use business language, not technical jargon
- Criteria are measurable (quantified, objective)
- Stakeholders are identified and roles are clear
- Assumptions are explicit when StRS or OpsCon are unavailable
- Pass/fail is unambiguous
- Release decision (go/no-go) is recorded in the UAT artifact

## Example outputs

See `example/` subdirectory for sample UAT documentation outputs generated using this skill.
