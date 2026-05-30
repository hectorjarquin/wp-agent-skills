# Public Documentation Template

**Recommended filename:** `<feature-slug>-public-documentation.md`

---

## Editorial Summary

Place the editorial summary right after the title, wrapped in brackets:

```
[Editorial: Feature: [Feature Name] | Pages: [count] | Total words: [count] | Sections: [count] (H2: X, H3: Y, H4: Z) | Screenshots needed: [count] | Doc category: /docs/category/[parent-category]/ | Audience: [audience role]]
```

---

## Single-page template

Use for features with 8 or fewer H2 sections and under 2,000 words.

```markdown
# [Feature Name] - [Verb phrase capturing the outcome]

[Editorial: Feature: [Feature Name] | Pages: 1 | Total words: [~count] | Sections: [count] (H2: X, H3: Y, H4: Z) | Screenshots needed: [~count] | Doc category: /docs/category/[parent-category]/ | Audience: [audience role]]

## Overview

This feature enables **[audience]** to **[goal]** by **[mechanism]**.

[SRS: FR-XX, FR-YY]

### Prerequisites

- [Permission or role required]
- [Dependency feature or setting]
- [Configuration needed]

---

## In this page

- [H2 Section 1 - scannable TOC entry](#h2-section-1)
  - [H3 Subsection A](#h3-subsection-a)
  - [H3 Subsection B](#h3-subsection-b)
- [H2 Section 2 - scannable TOC entry](#h2-section-2)

---

## [H2 Section 1 - scannable, standalone TOC entry]

### [H3 Subsection if needed]

Content content content.

<!-- IMAGE: screenshot showing [what the user should see] -->

Content content content.

[SRS: FR-ZZ]

---

## [H2 Section 2 - scannable, standalone TOC entry]

Content.

---

## [H2 Section N - max 8]

Content.

---

## Next Steps

**Local:**
- [Section 1](#h2-section-1)
- [Section 2](#h2-section-2)

**Global:**
- [Related Feature A](/docs/related-feature-a/)
- [Related Feature B](/docs/related-feature-b/)

[SRS: FR-XX] [StRS: SN-XX]
```

---

## Multi-page template

Use when content exceeds 8 H2 sections or 2,000 words, or when sections are sufficiently independent.

### Page 1: Overview + Table of Contents

```markdown
# [Feature Name] - [Verb phrase]

[Editorial: Feature: [Feature Name] | Pages: [count] | Total words: [~total across all pages] | Sections: [count] (H2: X, H3: Y, H4: Z) | Screenshots needed: [~count] | Doc category: /docs/category/[parent-category]/ | Audience: [audience role]]

## Overview

This feature enables **[audience]** to **[goal]** by **[mechanism]**.

### Prerequisites

- [Prerequisites]

---

## In this page

<!-- Full table of contents showing all sections across all pages -->

- [Page Title 2 - H2 Section 1](#page-title-2)
  - [H3 Subsection A](#h3-subsection-a)
  - [H3 Subsection B](#h3-subsection-b)
- [Page Title 3 - H2 Section 2](#page-title-3)
  - [H3 Subsection C](#h3-subsection-c)
- [Page Title N - H2 Section N](#page-title-n)

---

## Next Steps

**Local:**
- [Page 2: Section 1](#page-title-2)
- [Page 3: Section 2](#page-title-3)

**Global:**
- [Related Feature A](/docs/related-feature-a/)

[SRS: FR-XX]
```

### Page 2+: Content pages

```markdown
# [Feature Name] - [H2 Section Title]

## [H2 Section Title]

Content.

<!-- IMAGE: screenshot description -->

[H3 subsections as needed]

[SRS: FR-YY]

---

## Next Steps

**Local:**
- [Back to overview](#link-back-to-page-1)
- [Next section: Section Name](#link-to-page-3)

**Global:**
- [Related Feature A](/docs/related-feature-a/)

[SRS: FR-YY]
```

---

## Audience badge reference

Use these badges consistently in the editorial summary:

| Audience | Badge value |
|---|---|
| End users | `End user` |
| Site administrators | `Administrator` |
| Evaluators / decision-makers | `Evaluator` |
| AI agents / automation consumers | `Automation` |

---

## Image placeholder reference

Place `<!-- IMAGE: [description] -->` at the point in the content where the screenshot belongs.

**Good placeholders:**
- `<!-- IMAGE: settings page showing the Connections list with active/inactive status -->`
- `<!-- IMAGE: the AI Site Search input form with fields labeled -->`
- `<!-- IMAGE: admin menu navigation path to the Models page -->`

**Poor placeholders:**
- `<!-- IMAGE: screenshot -->`
- `<!-- IMAGE: settings -->`

---

## Heading quality checklist

Before handoff, verify every H2 heading:

- Can this heading be read as a standalone TOC entry?
- Would a reader scanning the "On this page" sidebar understand what this section covers?
- Does the heading use active, descriptive language? ("Configure Data Connections" not "Data Connections Configuration")
- Is the heading under 60 characters?
- Does the heading avoid internal jargon (class names, hooks, internal project names)?
- Does the text use hyphens (-) instead of em dashes (--) for list separators?
- Does the In this page TOC list every H2, H3, and H4 heading with a correct anchor link?
- Does every heading have an explicit anchor value to avoid collisions?

---

## Next steps checklist

Verify every page ends with a next steps section:

- Local links point to other sections or pages within the same feature
- Global links point to other features in the same doc category
- Links use descriptive anchor text, not "click here"
- At least one local and one global link present

---

## Block markup: In this page list format

When converting the markdown TOC to Gutenberg block markup, use `wp:list-item` wrappers around every list item. Nested sub-lists must use `wp:list` inside the parent `wp:list-item`.

```html
<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="#h2-section-1">H2 Section 1</a><!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="#h3-subsection-a">H3 Subsection A</a></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="#h2-section-2">H2 Section 2</a></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->
```

Without `wp:list-item` wrappers, the list renders flat instead of nested.
