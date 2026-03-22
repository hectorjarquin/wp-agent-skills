---
name: wp-qa-testing
description: "Use when WordPress software needs procedural testing framework and verification approach. Produces test strategy, test plans, test procedures, and test case specifications for developers and QA teams. Aligns to ISO/IEC/IEEE 29119-2:2021 (Test Process) with ISO/IEC TR 29119-6:2021 (Agile Tailoring). Use during implementation planning before testing begins."
compatibility: "Targets WordPress 6.9+ (PHP 7.4+). Applies to plugins, themes, blocks, and custom features requiring systematic verification."
---

# WordPress QA Testing

## When to use

Use this skill during implementation planning or early in development:

- Software Requirements Specification (SRS) is complete or nearly complete
- Testing approach and scope need to be formalized
- Test environments or procedures need documentation
- Developers need to know how to verify functionality before UAT
- QA coverage or pass/fail criteria need to be defined
- Risk-based testing prioritization is needed

**Timing:** Before or early during development. Not after testing completes (use for planning, not post-mortems).

## Purpose

This skill produces QA planning and procedural documentation that defines *how* developers and QA teams will systematically verify that the software meets its requirements.

**Unlike the SRS:** The SRS specifies *what* must be built. This skill defines *how we prove it works*.

**Unlike UAT:** This is internal QA for technical verification. UAT (User Acceptance Testing) is stakeholder-facing validation (use `wp-ua-testing` for that).

## Profile Selection

This skill is self-contained and must apply profile behavior without requiring any other workflow skill or repository-level policy file.

- **Default**: use the lightest QA artifact set that still supports production delivery
- **Team**: add explicit collaboration structure, reviewer ownership, readiness notes, and test responsibilities
- **Enterprise**: add formal traceability artifacts, approval records, and audit-oriented controls

Unless the user or organization asks for a stricter mode, use **Default**.

## Profile Application Rules

### Default

- Favor consolidated QA planning and inline requirement coverage.
- Keep environments, procedures, and test case structure concise but reproducible.
- Do not require separate sign-off artifacts unless the user asks for them.

### Team

- Add explicit responsibilities, reviewer ownership, and readiness criteria.
- Make handoff expectations and review notes visible in the QA package.
- Keep coverage structured enough for collaborative execution and review.

### Enterprise

- Add controlled document metadata, approval status, and formal evidence expectations.
- Produce formal test traceability artifacts when required by policy or audit needs.
- Record approval and defect-governance expectations explicitly.

## Portability Rule

If this skill is used on its own, it must still select and apply the correct profile using the rules in this file alone.

## What this skill produces

- **Test Strategy**
  - Overall approach: what types of testing (functional, integration, performance, security, etc.)
  - Why this approach (risk-based, requirement-based, etc.)
  - Testing phases and sequencing
  - What is in scope / out of scope

- **Test Plan**
  - Test scope per feature or requirement
  - Environments (staging, local, playground, production)
  - Responsibilities (who tests what)
  - Entry/exit criteria (when to start/stop)
  - Timeline and milestones

- **Test Environments & Setup**
  - Required WordPress versions
  - PHP versions and extensions
  - Third-party plugin compatibility (e.g., Yoast)
  - Database setup, test data, fixtures
  - How to reproduce/reset the environment

- **Test Case Specifications**
  - Preconditions (what must be true before testing)
  - Test steps (numbered, clear, repeatable)
  - Expected outcomes (what should you see?)
  - Pass/fail criteria (simple: did it match expected outcome?)
  - Linked to SRS Functional Requirements (FR-XX)

- **Test Procedures**
  - How developers run tests
  - Which tests are manual vs. automated
  - Regression testing scope
  - Performance or load testing thresholds (if applicable)
  - Security testing approach (if applicable)

- **Defect Management**
  - What constitutes a defect vs. expected behavior
  - Severity levels
  - Escalation process

## Inputs required

- Software Requirements Specification (SRS) — **mandatory**
- WordPress project context (plugin, theme, block, etc.)
- Estimated team size and testing capacity
- Third-party or integration points that affect testing
- Feature slug for artifact naming, normally derived from the project folder or canonical package name (for example `<feature-slug>`)

## Procedure

### 1. Analyze the SRS

Review the SRS thoroughly:

- Identify all Functional Requirements (FR-XX)
- Categorize by type (core features, integrations, edge cases)
- Note any non-functional requirements (performance, security, compatibility)
- Flag ambiguous or vague requirements

Create a mapping: SRS FR → Test Case (to fill later).

### 2. Define test scope and strategy

Decide what testing is needed:

| Dimension | Decision |
|---|---|
| **Functional Testing** | Test each FR against its acceptance criteria? (Usually Yes) |
| **Integration Testing** | Test interactions between components? (Depends on architecture) |
| **Performance Testing** | Measure page load, database queries, or throughput? (If SRS non-functional requirements exist) |
| **Security Testing** | Test authentication, authorization, input validation? (If SRS has security FR) |
| **Compatibility Testing** | Test against different WordPress versions, plugins, PHP versions? (If SRS requires it) |
| **Regression Testing** | Re-run all previous tests on each build? (Usually Yes after initial development) |

Document your decisions and rationale. This becomes your "Test Strategy."

### 3. Define test environments

Describe where testing happens:

- **Local developer environment:** each dev tests on their machine before commit
- **Staging environment:** integrated testing before production
- **WordPress Playground:** fast, reproducible test instances
- **Production-like environment:** as close to live as possible

For each environment, document:
- WordPress version(s)
- PHP version(s)
- Required plugins (especially third-party ones like Yoast)
- Database setup (fixtures, seed data)
- How to reset/reproduce

### 4. Map test cases to requirements

For each SRS Functional Requirement (FR-XX):

- Create at least one test case
- Format: TC-001 tests FR-18 and FR-19
- Multiple test cases per FR if needed (happy path, error cases, edge cases)

Use the test case template from `references/test-case-template.md`.

Each test case must include an inline coverage reference. For example:

> Covers: FR-01, FR-04

This replaces a separate traceability matrix — coverage is embedded in context, searchable, and maintained alongside each test.

Profile note:
- **Default** keeps coverage inline in the consolidated QA document.
- **Team** keeps inline coverage and adds explicit coverage summaries and reviewer notes.
- **Enterprise** may require a separate traceability matrix in addition to inline references.

### 5. Define entry/exit criteria

**Entry Criteria (when to START testing):**
- SRS is complete
- Code is deemed ready for testing (unit tests pass, build is stable)
- Test environment is available and functional
- Test data/fixtures are prepared

**Exit Criteria (when to STOP testing and release):**
- All required test cases have been executed
- Pass rate is ≥ [X]% (e.g., ≥ 95%)
- All critical/high-severity defects are resolved
- No unresolved defects blocking UAT or production
- All SRS Functional Requirements (FR-XX) have at least one inline-referenced test case

### 6. Define pass/fail criteria

For each test case, be clear:

**Pass:** Test case result matches expected outcome with no defects.

**Fail:** Test case result differs from expected outcome, shows an error, or behaves unexpectedly.

**Blocked:** Test cannot run due to environment issue or prerequisite failure.

Document severity levels:
- **Critical:** Feature completely broken or data loss
- **High:** Feature unusable or major performance impact
- **Medium:** Workaround exists; minor impact
- **Low:** Cosmetic or edge-case issue

### 7. Draft the test plan document

Use `references/test-plan-template.md`:

- Scope
- Strategy
- Environments
- Responsibilities
- Entry/Exit Criteria
- Coverage approach
- Schedule
- Risks and mitigations

Use the same feature slug across all QA artifacts.

- **Default filename (consolidated):** `<feature-slug>-qa-testing.md`
- Generic examples: `example/feature-slug-qa-testing-default.md`, `example/feature-slug-qa-testing-team.md`, `example/feature-slug-qa-testing-enterprise.md`
- Alternative (granular decomposition): `<feature-slug>-qa-test-plan.md`, `<feature-slug>-qa-test-cases.md`

### 8. Prepare test execution infrastructure

Before testing begins:

- Set up test environments
- Prepare test data/fixtures
- Document how to reset between test runs
- Set up defect tracking (if not already done)
- Brief the team on test procedures

### 9. Hand off to development/QA

When complete:

- Test cases are ready to execute
- Environments are functional
- Every test case has an inline `> Covers: FR-XX` reference
- Pass/fail criteria are unambiguous

Ready for developers to begin testing.

## Do not use this skill for

- Stakeholder acceptance testing (use `wp-ua-testing`)
- General WordPress Coding Standards (use `wp-coding-standards`)
- End-user or developer documentation (use `wp-user-documentation` or `wp-developer-documentation`)
- Block markup generation (use `wp-image-to-blocks`)
- Requirements specification itself (use `wp-requirements-specification`)

## Standards & Traceability

**Standard:** ISO/IEC/IEEE 29119-2:2021 — "Test Process"

**Agile Tailoring:** ISO/IEC TR 29119-6:2021 — "Agile and DevOps"

**Upstream Source:** Software Requirements Specification (SRS) from `wp-requirements-specification` skill

**Traceability Rule:**
- Every test case must map to at least one SRS Functional Requirement (FR-XX)
- Use inline coverage references in each test case: `> Covers: FR-XX`
- Identify gaps: if an FR has no test case, that's a risk

Quality Attributes (ISO 25010) overlay:
- Functionality → Functional test cases
- Reliability → Robustness, error handling tests
- Usability → UI/workflow tests
- Performance → Performance test cases
- Security → Security and input validation tests
- Compatibility → Cross-version, cross-plugin tests

**Quality Criteria:**
- Every FR has at least one test case with an inline `> Covers: FR-XX` reference
- Test procedures are repeatable and unambiguous
- Pass/fail criteria are objective
- Environments are reproducible

## Example outputs

See `example/` subdirectory for sample QA documentation outputs generated using this skill.
