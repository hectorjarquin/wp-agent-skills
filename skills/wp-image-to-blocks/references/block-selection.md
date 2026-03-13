# Gutenberg Block Selection Guide

Use this reference when deciding which Gutenberg block to use for visual elements in screenshots.

## Visual Element to Gutenberg Block Mapping

| Visual Element | Recommended Gutenberg Block(s) | When to Use | Example |
|---|---|---|---|
| **Large image + text overlay** | `core/cover` | Heroes, banner sections, background imagery | Background image with centered heading and button |
| **Multiple columns (equal width)** | `core/columns` + `core/column` | Multi-column layouts, features grid, testimonials | 3-column feature grid, 2-column content layout |
| **Feature cards (with icons/images)** | `core/columns` with `core/group` children | Feature showcase, pricing tables, card-based layouts | 3 cards with icons, titles, descriptions |
| **Text + image side-by-side** | `core/media-text` | Content + media layouts, alternating layouts | Image on left with description on right |
| **Plain text** | `core/paragraph` | Body content, descriptions, body copy | Longer paragraphs, article content |
| **Headings/titles** | `core/heading` | Section titles, hierarchy, semantic structure | H1 hero title, H2 section headers, H3 subsections |
| **Call-to-action buttons** | `core/buttons` → `core/button` | CTAs, forms, interactive elements | Single or grouped buttons with onClick intent |
| **Standalone image** | `core/image` | Photos, illustrations, single media | Full-width image, centered photo, aligned image |
| **Group of images** | `core/gallery` | Image collections, portfolios | 3x3 image gallery, photo grid |
| **Quote text + attribution** | `core/quote` or `core/pullquote` | Testimonials, quotes, emphasis text | Customer testimonial with name/title |
| **Horizontal line/separator** | `core/separator` | Visual breaks, section dividers | Line dividing sections |
| **Arbitrary grouped content** | `core/group` | Container for styling, padding, bgcolor | Padded section with background color |
| **Bullet/numbered lists** | `core/list` | Feature lists, menu items, enumeration | Bullet points, numbered steps, feature highlights |
| **Data tables** | `core/table` | Pricing grids, comparison tables, data | Pricing comparison, feature matrix, data grid |
| **Vertical spacing** | `core/spacer` | Intentional white space between sections | Add breathing room, vertical rhythm |
| **Video content** | `core/video` | Embedded videos, video backgrounds | YouTube embed, video showcase |
| **Blog post grid/listing** | `core/query` + query children | Archive pages, post listings, blog grids | Latest posts, category archives, custom queries |
| **Site header logo** | `core/site-logo` | Site branding in headers/templates | Logo in header, footer branding |
| **Site title text** | `core/site-title` | Site name in headers/footers | Site name link, branding text |
| **Site tagline** | `core/site-tagline` | Site description/motto | Subtitle under site title |
| **Navigation menu** | `core/navigation` | Header/footer menus | Main nav, footer links, mobile menu |
| **Template part** | `core/template-part` | Reusable header/footer sections | Header template, footer template |

## Block Selection Decision Tree

### Step 1: Identify the visual structure

**Simple, single-element visual patterns:**
- Text-only → Use `core/paragraph` or `core/heading`
- Bullet/numbered list → Use `core/list`
- Single image → Use `core/image`
- Multiple images → Use `core/gallery`
- Single button → Use `core/buttons`
- Horizontal line → Use `core/separator`
- Vertical spacing → Use `core/spacer`
- Video → Use `core/video`
- Quote/testimonial → Use `core/quote` or `core/pullquote`
- Data table → Use `core/table`
- Logo/branding → `core/site-logo`, `core/site-title`, `core/site-tagline`
- Navigation menu → `core/navigation`
- Reusable section block → `core/template-part`
- Repeated post cards (blog grid) → `core/query` with query children

**Container patterns with multiple elements:**
- Fixed columns (equal width) → `core/columns`
- Media + text together → `core/media-text`
- Background image with overlay → `core/cover`
- Styled box/card → `core/group`

### Step 2: Evaluate complexity

**Simple structure (elements side-by-side)?**
- Use `core/columns` for flexibility
- Use `core/media-text` only if specifically media-on-one-side-text-on-other

**Complex structure (layered, nested)?**
- Start with `core/group` as container
- Nest other blocks inside for layout control

### Step 3: Verify responsive behavior

Will this layout need to stack on mobile?
- `core/columns` supports `isStackedOnMobile`
- `core/media-text` supports `isStackedOnMobile`
- `core/cover` is inherently responsive

## Common Anti-Patterns to Avoid

❌ **Don't use** `core/group` when `core/columns` is appropriate
- Groups don't provide equal-width column flexibility

❌ **Don't nest** `core/columns` inside `core/columns` excessively
- Usually 2–3 levels max

❌ **Don't use** `core/cover` for layouts with extensive text
- Cover is better for visual statements with minimal overlay text

❌ **Don't ignore** `isStackedOnMobile` for responsive designs
- Always set responsive stacking for multi-column layouts

## Supported WordPress Core Blocks

Your skill references these WordPress core blocks organized by category:

### Layout & Container Blocks (5)
1. **`core/cover`** – Background image/video with overlay content
2. **`core/columns`** – Multi-column container (parent of `core/column`)
3. **`core/column`** – Individual column (child of `core/columns`)
4. **`core/group`** – Generic container with styling options
5. **`core/media-text`** – Side-by-side media + text layout

### Content Blocks (8)
6. **`core/paragraph`** – Text content block
7. **`core/heading`** – Semantic headings (H1–H6)
8. **`core/list`** – Bullet or numbered lists
9. **`core/quote`** – Block quotes with citations
10. **`core/pullquote`** – Stylized large quotes
11. **`core/table`** – Data tables and grids
12. **`core/separator`** – Horizontal divider line
13. **`core/spacer`** – Vertical spacing control

### Media Blocks (4)
14. **`core/image`** – Standalone image with captions
15. **`core/gallery`** – Image collection/grid
16. **`core/video`** – Video embed or upload
17. **`core/audio`** – Audio player

### Interactive Blocks (2)
18. **`core/buttons`** – Button group container
19. **`core/button`** – Individual CTA button (nested in `core/buttons`)

### Query Loop Blocks (6)
20. **`core/query`** – Post query container (blog grids, archives)
21. **`core/post-title`** – Post title (inside query)
22. **`core/post-featured-image`** – Post thumbnail (inside query)
23. **`core/post-excerpt`** – Post excerpt (inside query)
24. **`core/post-date`** – Post publication date (inside query)
25. **`core/post-author`** – Post author name (inside query)

### Site Editor Blocks (5)
26. **`core/site-logo`** – Site logo/branding
27. **`core/site-title`** – Site name/title
28. **`core/site-tagline`** – Site description/motto
29. **`core/navigation`** – Menu/navigation links
30. **`core/template-part`** – Reusable template section (header/footer)

**Use only these blocks in generated output.** Other WordPress blocks (custom blocks, third-party plugins) are out of scope for this skill.

## Reference for Block Implementation Details

For detailed structure and attribute examples for each Gutenberg block, see:

- [Media Handling Guide](./media-handling.md) – Image, media-text, cover, gallery, video specifics
- [Patterns Guide](./patterns.md) – Common layout examples
- [Validation Checklist](./validation.md) – Pre-submission quality checks
- Official WordPress developer resources: https://developer.wordpress.org/block-editor/
