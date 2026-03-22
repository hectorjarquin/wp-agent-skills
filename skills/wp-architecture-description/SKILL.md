---
name: wp-architecture-description
description: "Use when a WordPress feature, plugin, or subsystem needs a formal architecture description before implementation. Produces architecture views, architectural decisions (with rationale), constraints, risks, and requirement-linked coverage references to SRS IDs, with SyRS enrichment when available. Aligns architecture work to ISO/IEC/IEEE 42010:2022 and integrates with the requirements flow from wp-requirements-specification."
compatibility: "Targets WordPress 6.9+ (PHP 7.4+). Applies to plugins, themes, block themes, standalone blocks, and service-style WordPress integrations."
---

# WordPress Architecture Description

## When to use

Use this skill after requirements are defined and before implementation starts for non-trivial work:

- New plugin architecture or major subsystem changes
- Cross-cutting changes spanning multiple components
- New external integrations (APIs, providers, queues, indexing, search)
- Significant quality attribute tradeoffs (performance, reliability, security)
- Team handoff where architectural intent must be explicit

## Purpose

This skill documents system architecture decisions and rationale.

It answers:
- What is the architecture boundary and context?
- How is the system decomposed at architecture level?
- Why were key architecture decisions made?
- What constraints and risks shape implementation?

It does not produce class-level technical design or task plans.

## Profile Selection

This skill is self-contained and must apply profile behavior without requiring any other workflow skill or repository-level policy file.

- **Default**: use the lightest architecture artifact set that still supports production delivery
- **Team**: add explicit collaboration structure, reviewer ownership, and readiness or handoff notes
- **Enterprise**: add formal traceability artifacts, approval records, and audit-oriented controls

Unless the user or organization asks for a stricter mode, use **Default**.

## Profile Application Rules

### Default

- Keep architecture views, decisions, and coverage mappings consolidated where practical.
- Map decisions to requirement IDs inline in the architecture record.
- Use lightweight readiness notes rather than formal approvals.

### Team

- Add explicit reviewer or decision-owner identification for key architecture outputs.
- Record readiness and handoff notes for architecture decisions that affect implementation teams.
- Keep risks, constraints, and coverage mappings structured for shared review.

### Enterprise

- Add controlled document metadata, approval status, and audit-oriented decision records.
- Produce formal architecture traceability artifacts when required by policy or audit needs.
- Record approval and baseline expectations for architecture outputs explicitly.

## Portability Rule

If this skill is used on its own, it must still select and apply the correct profile using the rules in this file alone.

## Standards baseline

- Primary architecture standard: ISO/IEC/IEEE 42010:2022
- Upstream requirements source: ISO/IEC/IEEE 29148 flow (BRS, StRS, OpsCon, SyRS, SRS)

## Policy Block

This skill uses a lightweight but disciplined policy:

1. **Requirement-linked coverage**
   - Every major architectural decision must map to one or more requirement IDs.
   - Minimum mapping: `AD-XX -> FR-XX`.
   - Recommended additional mapping: `View-XX -> SyRS-XX`.

2. **Architecture-only scope**
   - In scope: architecture context, views, decisions, constraints, risks, rationale.
   - Out of scope: class-level/file-level design, endpoint payload design detail, task-by-task execution plans.

3. **No hard phase-gate requirement**
   - Architecture supports implementation planning but does not impose mandatory phase gates.

## Inputs required

- SRS requirement artifact from `wp-requirements-specification` (mandatory)
- SyRS requirement artifact from `wp-requirements-specification` (optional but strongly recommended for multi-system or interface-heavy work)
- Project context (plugin/theme/block and repository structure)
- Stakeholders and concerns (at minimum: developer and business owner)
- Feature slug for artifact naming, normally derived from the project folder or canonical package name (for example `<feature-slug>`)

## What this skill produces

- Architecture Description document
- Architecture Decision Records (ADR entries)
- Architecture coverage mapping (requirements to architecture items)
- Architecture readiness checklist

Default output filename:

- `<feature-slug>-architecture.md`
- Generic examples: `example/feature-slug-architecture-default.md`, `example/feature-slug-architecture-team.md`, `example/feature-slug-architecture-enterprise.md`

## Procedure

### 1. Confirm scope and boundary

Define what system/subsystem this architecture covers and what is explicitly excluded.

Also confirm the feature slug used for artifact naming. The architecture document must use the same slug as the related requirements package.

### 2. Load upstream requirements

Collect SRS IDs and constraints.

- If SyRS exists, collect system-level constraints, interfaces, and boundary assumptions to enrich the architecture scope.
- If only SRS exists, proceed using SRS as the canonical upstream handoff artifact and record any missing system-level assumptions inline.

- If required IDs are missing, stop and produce a dependency note: architecture cannot be finalized without requirement IDs.

### 3. Identify stakeholders and concerns

At minimum capture:
- Business concern
- Engineering concern
- Operational concern

### 4. Select viewpoints and create views

Use only architecture-level views, such as:
- Context view
- Container/component view (architecture granularity)
- Runtime interaction view (high-level)
- Deployment/environment view (if relevant)

### 5. Record architecture decisions

For each key decision:
- Decision statement
- Alternatives considered
- Rationale
- Consequences and tradeoffs
- Linked requirement IDs

Use `references/adr-template.md` for each decision.

### 6. Build architecture coverage mapping

Use `references/architecture-traceability-matrix-template.md`.

Minimum requirement:
- Every AD-XX maps to at least one FR-XX.

Profile note:
- **Default** uses inline and local coverage mapping inside the architecture document.
- **Team** keeps explicit coverage and readiness sections in the architecture document.
- **Enterprise** may require a separate controlled traceability artifact in addition to the document.

### 7. Run architecture readiness checklist

Use `references/architecture-quality-gate.md` to verify completeness and identify gaps.

For **Default**, use the checklist as a lightweight self-review.
For **Team**, record reviewer and readiness outcome.
For **Enterprise**, record checklist outcome under controlled document metadata and approvals.

### 8. Handoff to implementation

When architecture is sufficiently complete, hand off to implementation skills:
- Plugin architecture execution -> `wp-plugin-development`
- REST contracts and routes -> `wp-rest-api`
- Block behavior and metadata -> `wp-block-development`
- Block theme structure and templates -> `wp-block-themes`
- Interactivity behavior -> `wp-interactivity-api`

## Do not use this skill for

- Writing requirements documents themselves (use `wp-requirements-specification`)
- Writing detailed technical design (future dedicated skill)
- Creating implementation task plans or sprint checklists
- Writing end-user or developer usage docs (use respective documentation skills)

## Required IDs and formats

- Architecture Decision ID: `AD-01`, `AD-02`, ...
- Architecture View ID: `AV-01`, `AV-02`, ...
- Requirement IDs: preserve upstream format (for example `FR-18`, `SYRS-06`)
- Preferred mapping format: `AD-XX -> FR-XX`

## Completion criteria

All must be true:

- Architecture Description has required sections completed
- All major decisions have ADR entries
- Coverage mapping exists and major decisions are linked to requirement IDs
- Readiness checklist reviewed and unresolved gaps are documented
- Handoff note identifies next implementation skill(s)
