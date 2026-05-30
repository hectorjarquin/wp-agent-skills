# Public Documentation: <feature-slug>

**Profile:** Enterprise

---
Document ID: ENT-DOC-2026-001
Document Version: 1.0
Status: DRAFT — Pending Approval
Last Updated: [Date]
---

---
Editorial summary:
  Feature:     AI Abilities
  Pages:       3
  Total words: 1,840
  Sections:    9  (H2: 5, H3: 3, H4: 1)
  Screenshots needed: 4
  Doc category: /docs/category/ai-features/
  Audience:    Administrator
---

## Approval Record

| Role | Status | Date | Signature |
|---|---|---|---|
| Technical Reviewer | Pending | — | — |
| Documentation Lead | Pending | — | — |
| Product Owner | Pending | — | — |
| Compliance | Pending | — | — |

---

## Change History

| Version | Date | Author | Change Description |
|---|---|---|---|
| 1.0 | [Date] | [Author] | Initial draft |

---

## Traceability Matrix

| Section | SRS Requirements Covered |
|---|---|
| Overview | ABIL-FR-03, ABIL-FR-04, ABIL-FR-05 |
| Understanding AI Abilities | ABIL-FR-01, ABIL-FR-02 |
| Configure Data Connections | ABIL-FR-09, ABIL-DR-08, ABIL-DR-09 |
| Browse Available AI Models | ABIL-FR-10, ABIL-DR-05 |
| Use AI Site Search | ABIL-FR-05, ABIL-FR-08, ABIL-DR-04 |
| Troubleshooting | ABIL-OR-01 |
| Permissions | ABIL-OR-02, ABIL-OR-03 |

---

# AI Abilities — Configure and Use Your Site's AI Capabilities

## Overview

This feature enables **site administrators** to manage AI-powered capabilities on their WordPress site by configuring data connections, browsing available models, and enabling AI site search for logged-in users.

[SRS: ABIL-FR-03, ABIL-FR-04, ABIL-FR-05]

### Prerequisites

- WordPress 6.9+ with Gregius Data plugin installed and activated
- At least one AI model registered and active on the site
- At least one data connection configured and active

---

## Understanding AI Abilities

AI Abilities are discrete capabilities your WordPress site exposes to AI agents and automation tools. Each ability has a defined purpose, input requirements, and permission level.

[SRS: ABIL-FR-01, ABIL-FR-02]

---

## How to: Configure Data Connections

Data Connections tell the AI which data sources it can search.

<!-- IMAGE: settings page showing the Connections list with active/inactive badges -->

[SRS: ABIL-FR-09, ABIL-DR-08, ABIL-DR-09]

---

## How to: Browse Available AI Models

AI Models are the engines that power search, answers, and relevance ranking.

<!-- IMAGE: Models list page showing type filter and model cards -->

[SRS: ABIL-FR-10, ABIL-DR-05]

---

## How to: Use AI Site Search

Ask a question about your site's content and get an answer.

<!-- IMAGE: AI Site Search input form showing required fields -->

[SRS: ABIL-FR-05, ABIL-FR-08, ABIL-DR-04]

---

## Permissions

| Ability | Who can use it |
|---|---|
| AI Site Search | Any logged-in user with read access |
| Data Connections | Administrators only |
| AI Models | Administrators only |

[SRS: ABIL-OR-02, ABIL-OR-03]

---

## Next Steps

**Local:**
- [Configure Data Connections](#how-to-configure-data-connections)
- [Browse Available AI Models](#how-to-browse-available-ai-models)

**Global:**
- [Gregius Data Plugin Overview](/docs/gregius-data/)

---

## Release Control Note

This document is controlled content. Updates require compliance review before publication. See approval record above for current status.
