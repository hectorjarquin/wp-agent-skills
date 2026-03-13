# Visual Fidelity Priority

**CRITICAL PRINCIPLE**: Always prioritize matching the screenshot's visual appearance first. This is the highest priority when generating Gutenberg blocks from images.

## The Visual Fidelity Priority Rule

When you have a conflict between:
- What a documentation example shows
- What the screenshot displays

**ALWAYS choose the screenshot.**

### Why This Matters

Screenshots represent the user's **actual design intent**. Documentation examples are just structural guidance. Ignoring visual details from the screenshot results in blocks that don't match what the user expects.

## What "Visual Fidelity" Includes

### Styling Details to Extract Directly from Screenshot

1. **Colors**
   - Button colors (background + text)
   - Background colors of containers
   - Text colors
   - Border colors

### Color Matching Priority (CRITICAL)

Use this precedence order when choosing colors:

1. **Screenshot color values** (source of truth)
2. **Explicit prompt constraints** (if user requests specific tokens/presets)
3. **Theme presets** (`backgroundColor`, `textColor`) only when explicitly requested

Default behavior:
- Use explicit custom colors in `style.color.*` (hex/rgb) to match screenshot values.
- Do not map to "close enough" preset slugs unless the prompt asks for theme alignment.

Example:
- Screenshot CTA uses React cyan `#58c4dc`.
- Correct default output: `"style":{"color":{"background":"#58c4dc"}}` with `has-background`.
- Use `backgroundColor:"cyan-bluish-gray"` only when the user explicitly asks to use theme presets.

2. **Spacing & Sizing**
   - Padding around elements
   - Gaps between items
   - Relative sizing of components

3. **Borders & Shadows**
   - Border thickness, color, style
   - Shadow effects (drop shadow, inset)
   - Border radius (rounded corners)

4. **Typography**
   - Font sizes (title vs. body text)
   - Font weights (bold vs. regular)
   - Text alignment (center, left, right)

5. **Visual Hierarchy**
   - Which elements are visually prominent
   - Color contrast indicating importance
   - Size relationships between elements

6. **Layout Width & Alignment**
   - Full-width (edge-to-edge spanning)
   - Wide width (wider than content but contained)
   - Default content width (standard text measure)
   - Text alignment within containers (left, center, right)

### Layout Width Selection (Theme.json Compatibility)

WordPress themes define three layout width tiers through `theme.json`:

1. **Default Content Width** (~640-800px): Standard reading measure for text
2. **Wide Width** (~1200-1400px): Wider containers for galleries, media, or multi-column layouts
3. **Full Width** (edge-to-edge): Spans entire viewport width

#### Visual Indicators in Screenshots

**Use `alignfull` when:**
- Element visually touches or extends to viewport edges
- Background color/image fills entire screen width
- Hero sections, full-width banners, edge-to-edge covers
- No visible margin/padding on left and right edges

**Use `alignwide` when:**
- Element is wider than body text but doesn't reach edges
- Multi-column layouts that need more horizontal space
- Image galleries, feature grids that are "wider but contained"
- Visible margin on both sides, but wider than regular content

**Use default (no alignment) when:**
- Standard paragraph text blocks
- Single-column content matching body text width
- Elements that align with main content measure
- Typical blog post content flow

#### Common Patterns

| Screenshot Feature | Alignment Choice | Rationale |
|--------------------|------------------|-----------|
| Hero with edge-to-edge background | `alignfull` | Background visually spans full width |
| 3-column feature grid with margins | `alignwide` | Wider than text but contained |
| Paragraph text in main content | default | Standard content width |
| Full-screen image banner | `alignfull` | Image extends to edges |
| 2-column text layout with padding | `alignwide` | Needs width but not full edge |

#### Layout Width vs. Text Alignment (Don't Confuse!)

- **Layout width** (`align:"full"`, `align:"wide"`, or default): Controls container WIDTH
- **Text alignment** (`"align":"center"`): Controls text JUSTIFICATION within container

These are independent properties:
- You can have `alignfull` container with left-aligned text
- You can have default-width container with center-aligned text
- You can have `alignwide` columns with right-aligned headings

#### Nested Alignment Scenarios

Outer and inner blocks can have different alignments:

```html
<!-- Outer group spans full width -->
<div class="wp-block-group alignfull">
  <!-- Inner columns use wide width within the full-width group -->
  <div class="wp-block-columns alignwide">
    ...
  </div>
</div>
```

**Decision tree:**
1. Analyze outer container: Does it span edge-to-edge? → `alignfull`
2. Analyze inner content: Does it need to be constrained within outer? → `alignwide` or default
3. Common pattern: Full-width colored background with wide or default content inside

#### Extracting Width from Screenshot

**Step-by-step visual analysis:**
1. Look at element's left and right edges relative to viewport
2. Compare element width to body text width (if visible)
3. Check for visible margins/whitespace on sides
4. Identify if background extends beyond content edges

**Quick visual test:**
- **Edge-to-edge** = `alignfull`
- **Wider than text, not edge-to-edge** = `alignwide`  
- **Same as text width** = default (no alignment attribute)

## Implementation Workflow

### Step 1: Visual Analysis (First Pass)

Extract ALL visual styling from the screenshot:
- Note exact colors (hex values, RGB if visible)
- Measure relative spacing (use visual proportions)
- Identify borders, shadows, rounded corners
- Observe typography differences

**Write down:** "In this screenshot, I see..."
- Button: Blue background (#0073e6), white text
- Padding: Large gaps on all sides of content
- Cards: Subtle border (light gray), slight shadow
- Typography: Large heading (looks like 32px equivalent), regular body text

### Step 2: Block Structure (Second Pass)

Select Gutenberg blocks based on structure, not styling.
- Use `core/cover` for background images
- Use `core/columns` for multi-column layouts
- Use `core/group` for styled containers

### Step 3: Apply Visual Styling (Third Pass)

Map the visual details from Step 1 into Gutenberg block attributes and classes:
- Apply color attributes using parsed color info
- Apply spacing through `padding` attributes
- Apply styling through proper Gutenberg classes
- For color-critical UI (buttons, accents, brand tones), prefer explicit custom colors in `style.color.*`
- Use preset color slugs only when the prompt requests token/preset usage

## Common Scenarios

### Scenario: Screenshot shows centered blue button vs. Documentation example shows gray button

✅ **Correct**: Generate blue button (match screenshot)
❌ **Wrong**: Generate gray button (match example)

### Scenario: Screenshot shows tight spacing around cards vs. Documentation shows loose spacing

✅ **Correct**: Apply tight padding (match screenshot)
❌ **Wrong**: Apply loose padding (match example)

### Scenario: Screenshot shows rounded corners with shadow vs. Documentation shows sharp corners

✅ **Correct**: Add border radius and shadow (match screenshot)
❌ **Wrong**: Use sharp corners (match example)

## When Documentation Examples ARE Helpful

Examples are valuable for understanding **block structure and nesting**:
- How to nest buttons inside a buttons container
- How to structure columns correctly
- What class names are required for validation

But **never copy styling from examples**. Always extract from the screenshot.

## Balancing Visual Fidelity with WordPress Constraints

Sometimes the screenshot shows styling WordPress blocks can't achieve natively. In these cases:

1. **Use custom color values first** – Match screenshot colors as closely as possible
2. **Use theme presets only if requested** – Apply token alignment only when prompt asks for it
3. **Document the mismatch** – If WordPress truly cannot achieve the visual design
4. **Approximate as closely as possible** – Use best available options

Example: Screenshot shows brand-specific button color
- Default → Use exact custom color from screenshot
- If prompt requests presets → Map to nearest theme token and note the tradeoff if visibly different

## Key Principle: "Visual Accuracy Over Convention"

**Visual fidelity priority is not about being creative.** It's about being faithful to the user's design input (the screenshot). Your job is to translate that design into Gutenberg block code, not to improve or simplify it based on examples.

---

**Remember**: The screenshot is the source of truth. Everything else is supporting material.
