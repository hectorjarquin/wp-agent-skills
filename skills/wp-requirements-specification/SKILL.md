---
name: wp-requirements-specification
description: "Use when a WordPress feature, plugin, or block needs a formal pre-implementation specification package. Uses SRS as the default downstream handoff artifact and adds BRS, StRS, OpsCon, and SyRS only when stronger business, stakeholder, operational, or system traceability is needed. Aligns requirements work to ISO/IEC/IEEE 29148:2018 and enforces traceability from business outcomes to verifiable software requirements. Use before wp-block-development, wp-plugin-development, or wp-rest-api skills are invoked."
compatibility: "Targets WordPress 6.9+ (PHP 7.4+). Applies to plugins, themes, block themes, and standalone blocks."
---

# WordPress Requirements Specification

## When to use

Use this skill before implementation begins on any non-trivial WordPress feature:

- New plugin or major plugin feature
- New Gutenberg block or block collection
- REST API extension or data model change
- Site-wide architectural decision
- Client brief or stakeholder request needs formalizing

**This skill produces one or more specification documents for an ISO-aligned requirements package:**

| Document | Acronym | Answers |
|---|---|---|
| Business Requirements Specification | BRS | Why are we doing this and what business outcomes are required? |
| Stakeholder Requirements Specification | StRS | What do stakeholders need? |
| System Operational Concept | OpsCon | How will the system be used and operated in real scenarios? |
| System Requirements Specification | SyRS | What must the whole system do? |
| Software Requirements Specification | SRS | What must the software do? |

**Conformance note:** The ISO requirement quality review in this skill applies to StRS, SyRS, and SRS requirements. BRS and OpsCon are treated as upstream feeder artifacts that must remain traceable into the requirement specifications.

**ISO anchors used by this skill:**

- Section 8.2 outlines Business Requirements Specification (BRS)
- Section 8.3 outlines Stakeholder Requirements Specification (StRS)
- Section 8.4 outlines System Requirements Specification (SyRS)
- Section 8.5 outlines Software Requirements Specification (SRS)
- Operational concept and scenarios are covered in the stakeholder content guidance (for example, Section 9.4.16 and 9.4.17)

Do **not** use this skill for:

- Reviewing existing code — use `wp-project-triage`
- Block markup generation — use `wp-image-to-blocks`
- Plugin submission review — use `wp-plugin-directory-compliance`
- General coding standards — use `wp-coding-standards`

## Choosing the Right Specification Level

| Scenario | Produce |
|---|---|
| Sponsorship or value case is unclear | BRS first |
| Stakeholder expectations conflict or are underspecified | StRS |
| Real-world workflows, operations, or lifecycle states are unclear | OpsCon |
| Architectural decision spans subsystems or external interfaces | SyRS |
| Single plugin, block, or API feature with clear system context | SRS |
| Large or high-risk project | BRS + StRS + OpsCon + SyRS + SRS |

## Downstream Handoff Policy

Use SRS as the default downstream handoff artifact for architecture, QA, UAT, developer documentation, and user documentation.

Treat the other requirement-package artifacts as optional enrichments that are invoked only when they materially improve downstream traceability or decision quality:

- Generate **BRS** when business outcome traceability, sponsorship clarity, or value-case justification is needed.
- Generate **StRS** when stakeholder-needs traceability, formal acceptance framing, or competing stakeholder expectations must be preserved.
- Generate **OpsCon** when real-world workflows, operating scenarios, support paths, or lifecycle behavior materially affect acceptance, architecture, or user guidance.
- Generate **SyRS** when the solution spans subsystems, external interfaces, deployment boundaries, or non-software system constraints.
- Generate **SRS** by default for any implementable WordPress feature, block, plugin, API, or theme capability that needs downstream execution.

Do not generate all five artifacts unless project risk, scope, governance, or user direction justifies the full package.

If downstream work proceeds from SRS alone, record assumptions for any omitted upstream business, stakeholder, operational, or system context.

## Operating Modes

This skill supports two explicit operating modes:

- **SRS-first default mode**: produce SRS as the primary implementation handoff, and add BRS, StRS, OpsCon, or SyRS only when they materially improve downstream traceability or decision quality.
- **Full traceability package mode**: produce the full requirement stack when project scale, risk, governance, or user direction requires end-to-end traceability across business, stakeholder, system, and software layers.

Unless the user or organization asks for stricter governance, use **SRS-first default mode**.

## Profile Selection

This skill is self-contained and must apply profile behavior without requiring any other workflow skill or repository-level policy file.

- **Default**: use the lightest specification package that still supports production delivery
- **Team**: add explicit collaboration structure, reviewer ownership, and readiness or handoff notes
- **Enterprise**: add formal traceability artifacts, approval records, and controlled specification baselines

Unless the user or organization asks for a stricter mode, use **Default**.

## Profile Application Rules

### Default

- Favor consolidated specification outputs and concise assumptions.
- Keep upstream and downstream coverage inline unless a separate matrix is specifically requested.
- Do not require formal sign-off records.

### Team

- Add explicit reviewer or decision-owner identification to the specification package.
- Include readiness or handoff notes wherever work moves between roles.
- Keep traceability visible and structured enough for collaborative review, even if still inline.

### Enterprise

- Add controlled document metadata, approval status, and baseline information where applicable.
- Produce formal traceability artifacts when required by policy or audit needs.
- Record approval and change-control expectations explicitly in the specification package.

## Portability Rule

If this skill is used on its own, it must still select and apply the correct profile using the rules in this file alone.

## Inputs Required

- A feature brief, client request, or problem statement (any format)
- WordPress project context — run `wp-project-triage` if a repo is available
- Stakeholder list: business contact, developer, end-user representative

## Procedure

### 1. Classify the request

Determine which specification artifact(s) are needed using the table above.

Select an operating mode before drafting:

1. **SRS-first default mode**
2. **Full traceability package mode**

Use **SRS-first default mode** for ordinary WordPress feature delivery, constrained implementation asks, or projects with clear business context. In this mode:

1. Draft SRS first.
2. Add SyRS only if subsystem boundaries, interfaces, or deployment constraints matter.
3. Add StRS only if stakeholder-needs traceability or formal acceptance framing is needed.
4. Add OpsCon only if workflows, support paths, or lifecycle states materially affect downstream work.
5. Add BRS only if business outcome traceability, sponsorship clarity, or value-case justification is needed.

Use **Full traceability package mode** for large, high-risk, regulated, contract-heavy, or explicitly requested work. In this mode, produce:

1. BRS
2. StRS
3. OpsCon
4. SyRS
5. SRS

If you proceed in SRS-first default mode without one or more upstream artifacts, require an explicit assumptions section documenting the missing business, stakeholder, operational, or system context.

### 2. Run project triage (if repo available)

If a WordPress repository is in scope, load and run the `wp-project-triage` skill first. Extract these values from its JSON output for use in the specification:

- `project.kind` → informs architecture and scope decisions in SyRS/SRS
- `tooling.php`, `tooling.node` → sets technology constraint baselines
- `versions` → populates compatibility constraint sections
- `signals.paths` → identifies existing patterns that requirements must not break

### 3. Draft the specification

Use the appropriate template from `references/`:

- BRS → `references/brs-template.md`
- StRS → `references/strs-template.md`
- OpsCon → `references/opscon-template.md`
- SyRS → `references/syrs-template.md`
- SRS → `references/srs-template.md`

Use a single feature slug for the full artifact set and include it in every generated filename.

- Derive the slug from the canonical feature or package name, usually the project folder name, for example `<feature-slug>`.
- Default filename pattern: `<feature-slug>-<artifact>.md`.
- Minimum required example in SRS-first default mode: `<feature-slug>-srs.md`.
- Additional examples when optional artifacts are invoked: `<feature-slug>-brs.md`, `<feature-slug>-strs.md`, `<feature-slug>-opscon.md`, `<feature-slug>-syrs.md`.
- Do not mix generic filenames like `brs.md` with prefixed filenames in the same package.

Generic profile examples live in `example/feature-slug-srs-default.md`, `example/feature-slug-srs-team.md`, and `example/feature-slug-srs-enterprise.md`.

Replace every `[PLACEHOLDER]` with project-specific content.

For every section that cannot be completed yet, use one of these explicit markers:
- `TBD — requires stakeholder input`
- `N/A — not applicable to this project`

Do not leave empty sections. An incomplete SRS is a risk, not a draft.

### 4. Apply ISO requirement quality criteria

Before finalizing any normative requirement artifact you produce, review **every requirement** against the checklist in `references/requirement-quality-criteria.md`.

At minimum, this applies to SRS in SRS-first default mode, and to StRS, SyRS, and SRS in full traceability package mode.

Treat this as an internal review step. Do not cite the checklist file path inside the final specification artifacts.

Every requirement must pass all nine individual criteria. Flag any that fail with `[REVIEW NEEDED: reason]` inline.

Resolve all `[REVIEW NEEDED]` flags where feasible before implementation; if any remain, document risk and owner in the artifact.

### 5. Enforce traceability across all artifacts

Traceability rules depend on the operating mode:

- **SRS-first default mode**:
	- Every SRS requirement must trace to the best available upstream source.
	- Preferred order: SyRS capability, StRS stakeholder need, BRS business objective, or explicit documented assumption when no richer upstream artifact exists.
	- If OpsCon exists, operational scenarios should reference affected SRS requirement IDs.

- **Full traceability package mode**:
	- Every SRS requirement must trace upward through the full stack:
		- SRS requirement → SyRS capability
		- SyRS capability → StRS stakeholder need
		- StRS stakeholder need → BRS business objective
	- OpsCon scenarios must reference affected SyRS capabilities and SRS requirement IDs.

If a requirement cannot be traced to an upstream source, mark it `[UNTRACED]` and document remediation or rationale before implementation.

Profile note:
- **Default** keeps traceability directly in the core artifacts and allows assumption-backed traceability in SRS-first mode.
- **Team** keeps the same traceability with clearer review ownership and package completeness notes.
- **Enterprise** may require separate controlled traceability tables and approval metadata.

### 6. Separate requirements from design

Apply this split rule strictly:

| Statement type | Belongs in |
|---|---|
| What the system must do or be | SRS (requirement) |
| How to build it in WordPress | Technical Design document (separate) |
| Business rationale, outcomes, value metrics | BRS |
| Stakeholder goals and acceptance expectations | StRS |
| Operational scenarios, actors, and lifecycle contexts | OpsCon |

Do not embed implementation decisions (class names, hook names, file paths) inside SRS requirements. Move them to a companion Technical Design document that the developer produces during planning.

### 7. Hand off to implementation skills

Once specification is complete and requirement quality criteria pass, route to the correct implementation skill:

| SRS section routes to | Skill |
|---|---|
| Block functional requirements | `wp-block-development` |
| Interactivity / dynamic UI requirements | `wp-interactivity-api` |
| REST API contracts | `wp-rest-api` |
| Plugin architecture requirements | `wp-plugin-development` |
| Theme / template requirements | `wp-block-themes` |
| Code quality gate | `wp-coding-standards` |
| WordPress.org submission gate | `wp-plugin-directory-compliance` |
