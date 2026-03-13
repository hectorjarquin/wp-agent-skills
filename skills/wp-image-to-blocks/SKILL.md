---
name: wp-image-to-blocks
description: "Convert website screenshots into WordPress Gutenberg blocks using the G10n system with visual fidelity priority."
compatibility: "WordPress 6.9+ (PHP 7.2.24+). Image analysis: frontend-friendly; no CLI required."
---

# Convert Screenshots into Gutenberg Blocks

## When to use

Use this skill when:
- User provides a screenshot, wireframe, or image of web content
- Goal is to generate WordPress block editor HTML ready to paste directly
- Content targets either block editor (posts/pages) or site editor (templates/headers/footers)
- Visual design fidelity matters—match the screenshot's appearance, not documentation examples

Do **not** use this skill for:
- Custom Gutenberg block development (use `wp-block-development`)
- Theme customization or style rebuilds (use `wp-block-themes`)
- Complex server-rendered components beyond native Gutenberg blocks
- Content already in block editor (just edit directly)

## Inputs required

1. **Image** – Screenshot, wireframe, or design mockup

## Procedure

### 1) Analyze the image systematically

- **Identify overall structure**: Full width vs. constrained? Columns? Groups?
- **Map visual elements to Gutenberg blocks**: Headings, paragraphs, buttons, media, layout containers
- **Extract styling details**:
  - Colors (backgrounds, text), borders, shadows, spacing
  - Typography (sizes, weights, line heights)
  - Alignment and responsiveness cues
- **Note any complexity**: Pricing tables, feature cards, testimonials, etc.

### 2) Select native Gutenberg blocks

Use **only native WordPress core Gutenberg blocks**. Priority mapping:

| Visual Element | Gutenberg Block(s) | Why |
|---|---|---|
| Large image with text overlay | `core/cover` | Background image + inner content |
| Two-column layout | `core/columns` + `core/column` | Responsive, simple |
| Feature cards (grid) | `core/columns` with `core/group` children | Groups provide padding/borders |
| Content + image side-by-side | `core/media-text` | Purpose-built for this pattern |
| Text only | `core/paragraph`, `core/heading` | Semantic, minimal |
| Buttons | `core/buttons` → `core/button` | Grouped for alignment |
| Single image | `core/image` | Standard media Gutenberg block |
| Hero section | `core/cover` or `core/group` | See visual complexity |
| Blog post grid | `core/query` + query children | Archive pages, post listings |
| Site logo/branding | `core/site-logo` | Site editor: header/footer logos |
| Navigation menu | `core/navigation` | Site editor: header/footer menus |

**Note:** This table shows common patterns. See [Block Selection & Implementation](./references/block-selection.md) for the full list of 30+ supported core blocks including query loops, site editor blocks, lists, tables, galleries, and more.

Do **not** use:
- Custom CSS classes (only WordPress-standard classes)
- Hand-authored arbitrary inline CSS not represented by block attributes
- Non-core Gutenberg blocks (ACF, custom plugins, etc.)

Note: Gutenberg may serialize block attributes into inline `style` values in the final HTML. That is valid.

### 3) Apply visual fidelity priority (CRITICAL)

**ALWAYS match the screenshot first; use documentation examples only for structure.**

- Extract colors, spacing, borders, shadows directly from the image
- Override any example styling with what's visible in screenshot
- Treat the screenshot as the color source of truth: use custom color values in `style.color.*` by default
- Use theme preset slugs (`backgroundColor`, `textColor`) only if the prompt explicitly asks for theme tokens/presets
- For buttons: minimal styling—avoid custom padding unless visually required

### 4) Generate Gutenberg block HTML

Use local references in this skill for structure and implementation:

- [Block Selection & Implementation](./references/block-selection.md) – Map visual elements to the right core Gutenberg blocks
- [Common UI Patterns](./references/patterns.md) – Reusable layouts like hero, cards, media-text, and CTAs
- [Media Handling (Critical)](./references/media-handling.md) – Required media rules to avoid recovery errors
- [Visual Fidelity Guidelines](./references/visual-fidelity.md) – Keep screenshot fidelity as the top priority
- [Validation Checklist](./references/validation.md) – Final pre-return quality checks

**Critical rule for media Gutenberg blocks:**
- ❌ **Never** include `src=` in image placeholders: `<img src="..."/>`
- ✅ **Always** use empty img tags with empty alt: `<img alt=""/>`
- ✅ **Always** use proper Gutenberg block class names: `wp-block-cover__image-background`, `wp-block-image`, etc.

**Critical rule for block integrity (paste safety):**
- Output exactly one canonical Gutenberg version of the section.
- Do not provide multiple alternative versions in the same code block.
- Do not wrap already-rendered block HTML inside the same tag again (for example `<p><p>...</p></p>`).
- Do not add extra wrapper tags not represented by block comments.
- Keep opening/closing block comments and wrapper HTML in strict 1:1 order.

**Critical rule for classes (validation requirement):**
- Use only canonical WordPress classes required for the block structure.
- Do not invent custom classes.
- Prefer block JSON attributes (`style`, `textColor`, `backgroundColor`, `fontSize`, `borderColor`) as the styling source of truth.
- For media blocks, keep only required structural classes (`wp-block-image`, `wp-block-cover__image-background`, `wp-block-media-text__media`, etc.).
- For `core/button`, keep class/attribute parity because button serialization is strict.
- Examples:
   ```html
   <!-- ✅ Better: rely on attrs, keep canonical structural classes -->
   <!-- wp:heading {"textAlign":"center","level":3,"style":{"color":{"text":"#ffffff"},"typography":{"fontSize":"31px","lineHeight":"1.1"}}} -->
   <h3 class="wp-block-heading has-text-align-center" style="color:#ffffff;font-size:31px;line-height:1.1">Heading</h3>
   <!-- /wp:heading -->

   <!-- ✅ Correct: minimal core/image wrapper -->
   <!-- wp:image -->
   <figure class="wp-block-image"><img alt=""/></figure>
   <!-- /wp:image -->

   <!-- ❌ Wrong: extra manual classes -->
   <!-- wp:heading {"style":{"color":{"text":"#ffffff"},"typography":{"fontSize":"31px"}}} -->
   <h3 class="wp-block-heading has-text-color has-custom-font-size my-extra-class" style="color:#ffffff;font-size:31px">Heading</h3>
   <!-- /wp:heading -->
   ```

**Critical rule for color fidelity (source of truth):**
- Match colors from the screenshot first; do not substitute with nearby theme preset colors
- Default to custom color values via `style.color.*` (e.g., `"style":{"color":{"background":"#58c4dc"}}`)
- Only use palette preset attrs (`backgroundColor`, `textColor`) when explicitly requested in the prompt
- If preset mapping changes the visible color (for example `cyan-bluish-gray` -> `#abb8c3`), switch to explicit custom color values

**Color Preflight (run before generating HTML):**
1. Did the user explicitly request theme presets/tokens?
2. If **no**: use screenshot-matched custom values in `style.color.*`.
3. If **yes**: use `backgroundColor`/`textColor` slugs and keep utility classes valid.
4. If preset output visibly drifts from screenshot brand color, prefer custom values unless user requires presets.

**Critical rule for text content:**
- Prefer literal characters over HTML entities when the intended glyph is straightforward and paste-safe.
- Example: prefer the literal trademark symbol over `&trade;` unless entity encoding is required.

### 5) Structure and nesting

- Group related content in `core/group` Gutenberg blocks for consistent padding/borders
- Use proper heading hierarchy (h1 → h2 → h3, not jumps)
- Nest media content (images, video) inside appropriate containers
- For site editor: use `core/template-part`, `core/site-logo`, etc. where applicable
- For block editor: use standard Gutenberg blocks

### 6) Validate before returning

See **Verification** section below.

## Verification

Before returning the generated Gutenberg block HTML, confirm:

1. **No media `src=` attributes**  
   - Scan all `<img>` tags: should be empty (`alt=""`) or class-only
   - This prevents "Attempt block recovery" errors

2. **Valid Gutenberg block names**  
   - All blocks are `core/*` (e.g., `core/heading`, not `my-heading`)
   - Cross-check against [Block Selection & Implementation](./references/block-selection.md)

3. **Minimal canonical classes**  
   - Use only WordPress-standard structural classes (e.g., `wp-block-heading`, `wp-block-image`, `alignwide`).
   - No custom CSS classes like `my-custom-card`.
   - For `core/image`, keep wrappers minimal (`wp-block-image` plus alignment classes only when explicitly set).
   - For `core/button`, keep class/attribute parity strict.

4. **Correct Gutenberg block nesting**  
   - `core/columns` contains `core/column` children only
   - `core/buttons` contains `core/button` children only
   - `core/group` can contain any Gutenberg block

5. **No duplicated HTML wrappers (CRITICAL)**
   - No nested identical wrappers generated by mistake (for example `<p><p>...</p></p>` or `<a><a>...</a></a>`)
   - No duplicate content inside the same block
   - Each block should render exactly one expected wrapper element

6. **Output format**  
   - Generate HTML directly—**do not create or update files**
   - Format for easy copy-paste into Gutenberg block editor
   - Include explanatory comments if non-obvious

7. **Responsive behavior**  
   - Mobile stacking: Gutenberg columns should stack on small screens
   - If the target editor recovers columns into `className:"is-stacked-on-mobile"`, prefer that recovered serialization in future output for that environment

8. **Color source-of-truth check**
   - If the prompt does not request presets, avoid `backgroundColor`/`textColor` slugs for brand-critical colors
   - Ensure screenshot-matched colors are encoded as custom values in `style.color.*`

9. **Paste-safety check**
   - Markup should survive direct paste into the block editor without triggering "Attempt Block Recovery".
   - If uncertain, reduce non-structural classes and keep canonical wrappers only.
   - Keep `core/separator` output conservative; avoid decorative separator styling unless the exact serializer form is already confirmed in the target editor.

## Failure modes / debugging

| Problem | Solution |
|---|---|
| Text in image unreadable | Ask user for clarification or insert `[text needed]` placeholder |
| Complex custom component (not standard Gutenberg block) | Suggest approximating with native Gutenberg blocks; escalate if genuinely impossible |
| "Invalid block" error after pasting | Check for: `src=` in media, wrong parent Gutenberg blocks, typo in Gutenberg block names |
| "Attempt Block Recovery" due malformed structure | Check for nested duplicate wrappers (especially `<p>` and `<a>`), duplicated inner HTML, or extra wrapper tags not represented by block comments |
| Block validation errors for colors/styling | Remove manually added non-canonical classes; keep style intent in block attributes and keep HTML classes minimal |
| `core/button` validation mismatch | `style.typography.fontSize` exists in block attrs but link is missing `has-custom-font-size` | Add `has-custom-font-size` to `wp-block-button__link` so HTML matches serialized attrs |
| Block recovery after paste | Manually-added utility classes or class/attr mismatch | Remove non-canonical classes and rely on block attrs |
| Image block recovery | Extra classes on `core/image` wrapper | Keep `<figure class="wp-block-image">` minimal |
| Text content changes after recovery | Entity normalization by Gutenberg | Prefer literal characters over entities when safe |
| Color drift from screenshot | Used theme preset slug (for example `backgroundColor:"cyan-bluish-gray"`) instead of explicit screenshot color | Replace preset slug with `style.color.*` custom color value matching the screenshot |
| Color matching uncertain | Prioritize the screenshot and use explicit color values that match visible UI |
| Spacing looks off | Verify Gutenberg block padding/margin attributes match visual spacing in screenshot |
| Image too low-resolution | Ask user for higher-res version or wireframe notes |

## Escalation

Escalate to user or a broader skill when:

1. **Image quality too poor** – Cannot reliably determine structure or styling
2. **Custom functionality required** – Buttons that trigger modals, form validation, etc. (these need JavaScript/plugins)
3. **Unsupported Gutenberg block context** – Non-native Gutenberg blocks or features WordPress doesn't support
4. **Security/performance questions** – Defer to `wp-plugin-development` or `wp-performance` skills

## References

- [Block Selection & Implementation](./references/block-selection.md)
- [Visual Fidelity Guidelines](./references/visual-fidelity.md)
- [Media Handling (Critical)](./references/media-handling.md)
- [Validation Checklist](./references/validation.md)
- [Common UI Patterns](./references/patterns.md)

## Example workflow

```
User: "Here's a hero section screenshot with a background image, heading, text, and button."

1. Analyze: Full-width, background image, centered text overlay → use core/cover Gutenberg block
2. Select: core/cover (image + overlay), core/heading, core/paragraph, core/buttons Gutenberg blocks
3. Extract: Color from overlay (dark), button color (blue from screenshot)
4. Generate: HTML with proper Gutenberg block structure, empty img tag, centered content
5. Verify: No src= in img, all classes correct, Gutenberg block nesting valid
6. Return: Ready-to-paste Gutenberg block HTML code
```
