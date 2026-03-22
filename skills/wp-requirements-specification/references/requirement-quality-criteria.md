# Requirement Quality Criteria

**Standard:** ISO/IEC/IEEE 29148:2018 — Section 5.2: Characteristics of requirements

> Use this checklist to review **every requirement** in StRS, SyRS, and SRS before handoff to implementation. A requirement that fails any criteria is not ready. Do not accumulate `[REVIEW NEEDED]` flags — resolve them before closing the specification package.

> BRS and OpsCon are not requirement catalogs in the same format, but they still require quality checks for clarity, consistency, verification and acceptance readiness, and traceability into StRS, SyRS, and SRS.

> This checklist assumes the standardized document family structure used by these templates:
> 1) Introduction, 2) References, 3) Core Content, 4) Verification and Acceptance, 5) Traceability, 6) Appendices.

---

## Part 1: Individual Requirement Criteria

Apply the following nine criteria to each requirement statement, one at a time.

---

### 1. Necessary

The requirement represents something that, if absent, would leave a known gap in capability or constraint needed by stakeholders. Remove it, and something important is missing.

> **Fails if:** The requirement is "nice to have" with no traceable stakeholder rationale.
> **Test:** Can you point to a use case, stakeholder need, or system function in the StRS or SyRS that this requirement addresses?

---

### 2. Appropriate

The requirement is expressed at the correct level of abstraction for its document type. An SRS requirement states **what** the software must do — not **how** to implement it.

> **Fails if:** The requirement names a PHP class, a specific function, a file path, a WordPress hook name, or a design pattern.

| ✅ Correct | ❌ Incorrect |
|---|---|
| "The block MUST output a `<details>` element for each FAQ item." | "The `FAQBlock::render()` method MUST use a `foreach` loop to output `<details>` elements." |
| "All HTML output containing user-supplied content MUST be sanitized to an allowed HTML subset." | "MUST use `wp_kses_post()` to sanitize output." |
| "JSON-LD structured data MUST be present in the page `<head>` section." | "MUST hook into `wp_head` at priority 10." |

---

### 3. Unambiguous

The requirement has exactly one possible interpretation. No vague quantifiers, no pronouns with unclear antecedents, no relative adjectives like "fast", "secure", "easy", or "scalable".

> **Fails if:** Two developers could read the requirement differently and implement it legitimately in two incompatible ways.

| ✅ Correct | ❌ Incorrect |
|---|---|
| "The block MUST render in under 100ms on a simulated 3G connection as measured by Lighthouse." | "The block MUST load quickly." |
| "The system MUST NOT add more than 3 database queries per page render." | "The system MUST be performant." |

---

### 4. Complete

The requirement contains all the information needed to understand it without referencing undefined terms, external knowledge, or undisclosed context.

> **Fails if:** The requirement references a capability, system, term, or threshold that is not defined anywhere else in the same specification document.

---

### 5. Singular

The requirement states exactly one thing. It does not use "and" or "or" to combine two separately satisfiable or violable conditions.

> **Fails if:** You can comply with one part of the requirement and violate another part and still argue partial compliance.

| ✅ Correct (two requirements) | ❌ Incorrect (combined) |
|---|---|
| "FR-01: The block MUST output JSON-LD structured data." | "The block MUST output JSON-LD structured data and the data MUST conform to schema.org FAQPage type." |
| "FR-02: The JSON-LD output MUST conform to the schema.org FAQPage type." | |

---

### 6. Feasible

The requirement can be implemented within the known technical constraints and resource boundaries of the project.

> **Fails if:** The requirement conflicts with WordPress core limitations, PHP/WP version targets, known plugin incompatibilities, budget, or timeline.

---

### 7. Verifiable

A practical method exists to confirm whether the requirement has been met: automated test, static analysis tool, Lighthouse audit, manual inspection, or formal demonstration.

> **Fails if:** There is no conceivable way to write a test, run an audit, or perform an inspection that would produce a definitive pass/fail result.

| ✅ Verifiable | ❌ Not verifiable |
|---|---|
| "All HTML output MUST be escaped." (PHPCS rule: `WordPress.Security.EscapeOutput`) | "The implementation MUST be clean." |
| "The block MUST NOT add more than 3 queries." (Query Monitor) | "The implementation MUST have good performance." |
| "The block MUST pass `wp plugin check` with zero blocking issues." | "The plugin MUST follow best practices." |

---

### 8. Correct

The requirement accurately represents an actual stakeholder need or system constraint. It is traceable back to a StRS stakeholder need, a SyRS system function, or a compliance policy.

> **Fails if:** No parent document or policy supports this requirement. It was invented during SRS authoring without a traceable origin.

---

### 9. Conforming

The requirement follows the project's agreed style, ID scheme, priority notation, and template conventions.

> **Fails if:** The requirement deviates from the document's ID scheme (`PREFIX-NN`), priority notation (`MUST / SHOULD / MAY`), or table format.

---

## Checklist: Individual Requirement Review

Copy and complete for each requirement before marking a specification ready:

```
Requirement ID:  [ID]
Requirement:     "[REQUIREMENT TEXT]"

[ ] 1. Necessary      — Has clear, traceable stakeholder rationale
[ ] 2. Appropriate    — Says WHAT, not HOW; correct abstraction level for document type
[ ] 3. Unambiguous    — Only one possible interpretation; no vague qualifiers
[ ] 4. Complete       — All information present; no undefined references
[ ] 5. Singular       — States exactly one thing; no hidden "and" conditions
[ ] 6. Feasible       — Achievable within known constraints
[ ] 7. Verifiable     — Can be confirmed by test, audit, or formal inspection
[ ] 8. Correct        — Traceable to a stakeholder need, system function, or policy
[ ] 9. Conforming     — Follows project template, ID scheme, and notation

Result: PASS  /  REVIEW NEEDED
Notes:  [If REVIEW NEEDED — describe exactly what fails and what change is needed]
```

---

## Part 2: Set of Requirements Criteria

Apply these four criteria to the **specification document as a whole** after all individual requirements have passed Part 1.

### 1. Complete (Set)

Every function or capability stated in the StRS or SyRS is covered by at least one requirement in this document. No known use case, stakeholder need, or system function is left unaddressed.

### 2. Consistent (Set)

No two requirements contradict each other. Where requirements are conditional on different modes or states, those conditions are explicitly documented. No requirement in this document conflicts with requirements in a related SRS.

### 3. Affordable (Set)

The total set of requirements can realistically be implemented within the project's budget, schedule, and team capacity. If the cumulative scope exceeds constraints, requirements must be reprioritized before implementation begins.

### 4. Bounded (Set)

The scope of the requirements is clearly delimited. No requirement is phrased so broadly that it would justify unbounded scope expansion if interpreted liberally.

---

## Part 2B: Cross-Artifact Integrity Checks

Apply these checks to the full package when BRS and OpsCon are included.

### 1. BRS to StRS alignment

Each business requirement or objective in BRS maps to at least one stakeholder need in StRS. If a BRS item has no StRS mapping, it is planning debt and must be resolved.

### 2. OpsCon to SyRS and SRS alignment

Each operational scenario in OpsCon maps to system capabilities in SyRS and software requirements in SRS. Operational behaviors with no requirement mapping are unverifiable and must be corrected.

### 3. No orphan requirements

Every SyRS and SRS requirement has an upstream source (BRS objective, StRS need, or OpsCon scenario). Mark missing lineage as `[UNTRACED]` and resolve before implementation.

### 4. Verification and acceptance completeness

Each artifact (BRS, StRS, OpsCon, SyRS, SRS) includes a complete Section 4 "Verification and Acceptance" table with owner and exit criteria. Missing acceptance criteria is a release risk.

### 5. Traceability completeness

Each artifact includes Section 5 "Traceability" and all IDs referenced in Section 5 exist in the linked upstream/downstream artifacts. Any broken link must be marked `[TRACE GAP]` and resolved.

---

## Part 3: WordPress-Specific Requirement Smells

Additional failure patterns common when writing WordPress requirements.

| Smell | Problematic Example | Corrected Version |
|---|---|---|
| Names a WordPress function | "MUST use `wp_kses_post()`" | "All HTML output containing user-supplied content MUST be sanitized to an allowed HTML subset." |
| Implies a specific hook without stating the behavior | "MUST hook into `wp_head`" | "MUST output JSON-LD structured data within the page `<head>` section." |
| Mixes editor and frontend behavior | "MUST show a toggle in the editor and on the frontend" | Split: one requirement for editor UX, one for frontend rendering. |
| Vague performance claim | "MUST be performant" | "MUST NOT register scripts on pages where no block instance is present." |
| Assumes plugin activation = feature available | "After activation, blocks are available" | "The block MUST be registered during the WordPress `init` action." |
| Implementation detail in SRS | "MUST use a PHP class named `[ClassName]`" | Move to Technical Design document — not an SRS requirement. |
| Untestable quality claim | "Code MUST be maintainable" | Remove or replace with measurable proxy: "All public PHP methods MUST have PHPDoc blocks." |
