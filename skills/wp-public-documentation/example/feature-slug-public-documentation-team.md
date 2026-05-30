# Public Documentation: <feature-slug>

**Profile:** Team
**Status:** Draft — Pending Review
**Reviewer:** [Name]
**Review due:** [Date]

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

**Review Checklist:**

- Purpose paragraph correctly identifies audience, goal, and mechanism
- H2 headings scannable as standalone TOC entries
- Editorial summary block complete
- Image placeholders placed at relevant points
- Next steps present on every page
- SRS traceability inline

---

# AI Abilities — Configure and Use Your Site's AI Capabilities

## Overview

This feature enables **site administrators** to manage AI-powered capabilities on their WordPress site by configuring data connections, browsing available models, and enabling AI site search for logged-in users.

[SRS: ABIL-FR-03, ABIL-FR-04, ABIL-FR-05]

### Prerequisites

- WordPress 6.9+ with Gregius Data plugin installed and activated
- At least one AI model registered and active on the site
- At least one data connection configured and active

**Support impact:** This feature may generate support requests around connection configuration and model selection. Ensure the support team has access to the Connections and Models admin screens.

---

## Understanding AI Abilities

AI Abilities are discrete capabilities your WordPress site exposes to AI agents and automation tools. Each ability has a defined purpose, input requirements, and permission level.

Three abilities are available:
- **AI Site Search** — Searches your site's content and answers questions
- **Data Connections** — Lists configured data sources the AI can access
- **AI Models** — Lists registered AI models for search, answers, and ranking

These abilities work together. You need at least one data connection and one AI model before the AI can answer questions.

[SRS: ABIL-FR-01, ABIL-FR-02]

---

## How to: Configure Data Connections

Data Connections tell the AI which data sources it can search. Each connection links to a database or API.

<!-- IMAGE: settings page showing the Connections list with active/inactive badges -->

### View connections

When you list your connections, each one shows:
- **Name** — A label identifying the connection
- **Type** — The data source type (PostgreSQL database or Supabase-style REST API)
- **Description** — What data this connection provides
- **Active status** — Whether the connection is currently enabled

### Get optional details

You can request additional information per connection:

- **Embedding model overview** — Shows which embedding model keys are active and how many
- **Full model details** — Shows model ID, type, provider, label, active status, and optional dimensions or description

[SRS: ABIL-FR-09, ABIL-DR-08, ABIL-DR-09]

### Tips

- Use this ability to find valid connection names before using AI Site Search
- Connections can be database-backed (PostgreSQL) or API-backed (Supabase-style REST)

---

## How to: Browse Available AI Models

AI Models are the engines that power search, answers, and relevance ranking. Different model types handle different tasks.

<!-- IMAGE: Models list page showing type filter and model cards -->

### View all models

Each model displays:
- **ID** — Unique identifier
- **Type** — What it's used for (embeddings, LLM, rerank)
- **Provider** — Where the model comes from
- **Label** — Display name
- **Active status** — Whether the model is enabled
- **Description** — What it does (if available)
- **Dimensions** — Technical reference (if applicable)

### Filter by type

You can narrow the list:
- **embeddings** — Convert text into searchable vectors
- **llm** — Language models that generate answers
- **rerank** — Improve result relevance

[SRS: ABIL-FR-10, ABIL-DR-05]

### Tips

- Use this ability to find valid model values before using AI Site Search
- Models are listed from your global registry, not per-connection storage

---

## How to: Use AI Site Search

Ask a question about your site's content and get an answer based on what the AI finds in your connected data sources.

<!-- IMAGE: AI Site Search input form showing required fields -->

### What you need

Before asking a question:
- A **data connection** configured and active
- An **embedding model** for search
- An **answer model** for generating responses
- Optionally, a **rerank model** for improved relevance

### Required inputs

| Input | What it is | Example |
|---|---|---|
| Query | Your question | "What features does the Pro plan include?" |
| Connection name | The data source to search | "docs-database" |
| Embedding model | Model for searching | "text-embedding-3-small" |
| Answer model | Model for generating the response | "gpt-4o-mini" |

### What you get back

- **Answer** — A generated response to your question
- **Sources** — References showing where the information came from
- **Metadata** — Information about the search and generation process

[SRS: ABIL-FR-05, ABIL-FR-08, ABIL-DR-04]

### Troubleshooting

**Problem:** "Missing query" error
- **Fix:** Include a question in your request

**Problem:** Answer doesn't seem accurate
- **Fix:** Verify your data connection includes the expected content. Try adding a rerank model.

**Problem:** Answer not appearing at all
- **Fix:** Check that the Abilities API is active. Verify Gregius Data is installed and activated.

[SRS: ABIL-OR-01]

---

## Permissions

| Ability | Who can use it |
|---|---|
| AI Site Search | Any logged-in user with read access |
| Data Connections | Administrators only |
| AI Models | Administrators only |

[SRS: ABIL-OR-02, ABIL-OR-03]

---

## Onboarding Note

New administrators should start with "Browse Available AI Models" to understand what models are registered, then proceed to "Configure Data Connections" to ensure at least one connection is active.

---

## Next Steps

**Local:**
- [Configure Data Connections](#how-to-configure-data-connections)
- [Browse Available AI Models](#how-to-browse-available-ai-models)

**Global:**
- [Gregius Data Plugin Overview](/docs/gregius-data/)
- [AI Model Management](/docs/ai-model-management/)

[SRS: ABIL-FR-01..10] [SRS: ABIL-DR-01..09] [SRS: ABIL-OR-01..04]
