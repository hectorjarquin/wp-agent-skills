# Architecture Description: [SYSTEM_OR_FEATURE_NAME]

**Recommended filename:** `[feature-slug]-architecture.md`

## 1. Purpose and Scope

**Purpose:**
[What architecture does this document describe?]

**In scope:**
- [Architecture boundary item 1]
- [Architecture boundary item 2]

**Out of scope:**
- [Detailed technical design]
- [Task plans]
- [Class/file-level design]

## 2. Stakeholders and Concerns

| Stakeholder | Concern | Priority |
|---|---|---|
| [Business owner] | [Concern] | [High/Med/Low] |
| [Engineering lead] | [Concern] | [High/Med/Low] |
| [Operations] | [Concern] | [High/Med/Low] |

## 3. Architecture Context (AV-01)

**System context summary:**
[Describe external systems, users, and major boundaries]

**Interfaces at context level:**
- [Interface 1]
- [Interface 2]

## 4. Architecture Views

### 4.1 Container or Component View (AV-02)

**Major elements:**
- [Element 1]
- [Element 2]
- [Element 3]

**Element responsibilities:**
| Element | Responsibility |
|---|---|
| [Element] | [Responsibility] |

### 4.2 Runtime Interaction View (AV-03)

[Describe high-level interaction flow between major architecture elements]

## 5. Architectural Decisions

### Decision AD-01: [Decision title]

- **Status:** [Proposed/Accepted/Superseded]
- **Decision:** [Decision statement]
- **Rationale:** [Why this decision]
- **Alternatives considered:** [List]
- **Consequences:** [Positive/negative consequences]
- **Traceability:** [FR-XX, FR-YY]

### Decision AD-02: [Decision title]

- **Status:** [Proposed/Accepted/Superseded]
- **Decision:** [Decision statement]
- **Rationale:** [Why this decision]
- **Alternatives considered:** [List]
- **Consequences:** [Positive/negative consequences]
- **Traceability:** [FR-XX]

## 6. Constraints and Assumptions

**Constraints:**
- [Technology/version constraint]
- [Operational constraint]

**Assumptions:**
- [Assumption 1]
- [Assumption 2]

## 7. Risks and Mitigations

| Risk ID | Description | Impact | Mitigation |
|---|---|---|---|
| [R-01] | [Risk] | [High/Med/Low] | [Mitigation] |
| [R-02] | [Risk] | [High/Med/Low] | [Mitigation] |

## 8. Architecture Coverage Mapping

| Architecture Item | Requirement IDs | Coverage Note |
|---|---|---|
| AD-01 | FR-XX, FR-YY | [How it fulfills requirement] |
| AD-02 | FR-ZZ | [How it fulfills requirement] |
| AV-02 | SYRS-XX | [View coverage note] |

## 9. Architecture Readiness Summary

- [ ] All major decisions traced to requirement IDs
- [ ] No major architecture decision left undocumented
- [ ] Risks documented for critical decisions
- [ ] No placeholder left in mandatory sections

## 10. Handoff

**Next implementation skill(s):**
- [wp-plugin-development]
- [wp-rest-api]

**Architecture handoff notes:**
[What implementation teams must preserve from architecture]

## 11. Document Control

- **Version:** [x.y]
- **Status:** [Draft/Reviewed]
- **Author:** [Name]
- **Last Updated:** [Date]
