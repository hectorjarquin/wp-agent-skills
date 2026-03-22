# Architecture Readiness Checklist

Use this checklist before handing architecture to implementation.

## Recommended Readiness Criteria

### Completeness

- [ ] Architecture scope and boundary are explicit
- [ ] Stakeholders and concerns are documented
- [ ] At least one architecture context/view exists
- [ ] Major architecture decisions are documented (AD-XX)
- [ ] Constraints and assumptions are explicit
- [ ] Risks and mitigations are documented

### Coverage Mapping

- [ ] Every major AD-XX maps to one or more FR-XX IDs
- [ ] Coverage mapping exists and is understandable
- [ ] Any unresolved coverage gaps are documented with follow-up notes

### Quality

- [ ] Decisions include rationale and alternatives
- [ ] Tradeoffs are explicit
- [ ] No conflicting decisions across the document
- [ ] No unresolved placeholders in mandatory sections

## Escalation Conditions

Escalate before implementation if any of the following is true:

1. Major architecture decisions are not linked to requirements.
2. Critical risks exist without mitigation.
3. Core architecture sections are missing.

## Readiness Outcome

- **Result:** [READY | NEEDS FOLLOW-UP]
- **Reviewer:** [Name]
- **Date:** [YYYY-MM-DD]
- **Open issues (if any):**
  - [Issue 1]
  - [Issue 2]
