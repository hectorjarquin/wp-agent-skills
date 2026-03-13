# WordPress Block Syntax Reference

Use this reference to understand WordPress-specific block syntax conventions. Modern AI models understand web patterns—this is for WordPress markup specifics.

## Block Comment Format

WordPress blocks use HTML comment syntax with block name and JSON attributes:

```html
<!-- wp:blockName {"attributeName":"value"} -->
<div class="wp-block-blockname">
  <!-- Block content -->
</div>
<!-- /wp:blockName -->
```

**Key rules:**
- Block names are `core/block-name` format (e.g., `core/heading`, `core/button`)
- Attributes are JSON objects inside the opening comment
- Closing comment uses `/wp:blockName`
- Self-closing format: `<!-- wp:blockName /-->`
- One block definition should map to one canonical HTML wrapper/content output.
- Never output two alternate versions of the same section in the same response.

## Common Attribute Patterns

### Colors (Class-Based)

```html
<!-- wp:paragraph {"textColor":"primary","backgroundColor":"light-gray"} -->
<p class="has-primary-color has-light-gray-background-color has-text-color has-background">Text</p>
<!-- /wp:paragraph -->
```

Use class-based preset colors only when the prompt explicitly asks for theme tokens/presets.

### Colors (Custom Values)

```html
<!-- wp:paragraph {"style":{"color":{"text":"#0073e6","background":"#f8f9fa"}}} -->
<p class="has-text-color has-background" style="color:#0073e6;background-color:#f8f9fa">Text</p>
<!-- /wp:paragraph -->
```

Use custom values by default for screenshot-to-block conversion. The screenshot color is the source of truth.

### Spacing & Styling

```html
<!-- wp:group {"style":{"spacing":{"padding":{"top":"2rem","bottom":"2rem"}},"border":{"radius":"8px"}}} -->
<div class="wp-block-group" style="border-radius:8px;padding-top:2rem;padding-bottom:2rem">
  <!-- Content -->
</div>
<!-- /wp:group -->
```

### Custom Colors with Background

```html
<!-- wp:group {"style":{"color":{"background":"#f5f5f5"}}} -->
<div class="wp-block-group has-background" style="background-color:#f5f5f5">
  <!-- Content -->
</div>
<!-- /wp:group -->
```

### Alignment

```html
<!-- wp:heading {"align":"center"} -->
<h2 class="wp-block-heading has-text-align-center">Centered Heading</h2>
<!-- /wp:heading -->
```

## Nesting & Container Blocks

Blocks can contain other blocks. Common parent-child relationships:

**Columns structure:**
```html
<!-- wp:columns {"align":"wide"} -->
<div class="wp-block-columns alignwide">
  <!-- wp:column -->
  <div class="wp-block-column">
    <!-- Child blocks here -->
  </div>
  <!-- /wp:column -->
</div>
<!-- /wp:columns -->
```

**Buttons container:**
```html
<!-- wp:buttons -->
<div class="wp-block-buttons">
  <!-- wp:button -->
  <div class="wp-block-button">
    <a class="wp-block-button__link wp-element-button">Click Me</a>
  </div>
  <!-- /wp:button -->
</div>
<!-- /wp:buttons -->
```

**Button with custom styling (border and font size):**
```html
<!-- wp:buttons -->
<div class="wp-block-buttons">
  <!-- wp:button {"className":"is-style-outline","style":{"border":{"radius":"8px"},"typography":{"fontSize":"1.125rem"}},"borderColor":"white"} -->
  <div class="wp-block-button is-style-outline">
    <a class="wp-block-button__link has-border-color has-white-border-color has-custom-font-size wp-element-button" style="border-radius:8px;font-size:1.125rem">Click Me</a>
  </div>
  <!-- /wp:button -->
</div>
<!-- /wp:buttons -->
```
**Note:** When using `borderColor` attribute, always include both `has-border-color` and `has-{color}-border-color` classes. When using custom `fontSize` in `style.typography`, include `has-custom-font-size` class. For `core/button`, keep the `<a class="wp-block-button__link ...">` classes aligned with the JSON attrs to prevent validation mismatch.

**Button with screenshot-faithful custom background color (default):**
```html
<!-- wp:buttons -->
<div class="wp-block-buttons">
  <!-- wp:button {"style":{"color":{"background":"#58c4dc"},"typography":{"fontSize":"14px","fontWeight":"600"},"border":{"radius":"999px"}}} -->
  <div class="wp-block-button">
    <a class="wp-block-button__link has-background has-custom-font-size wp-element-button" style="background-color:#58c4dc;border-radius:999px;font-size:14px;font-weight:600">Learn React</a>
  </div>
  <!-- /wp:button -->
</div>
<!-- /wp:buttons -->
```

**Button using theme preset color (only when explicitly requested):**
```html
<!-- wp:buttons -->
<div class="wp-block-buttons">
  <!-- wp:button {"backgroundColor":"cyan-bluish-gray","style":{"typography":{"fontSize":"14px","fontWeight":"600"},"border":{"radius":"999px"}}} -->
  <div class="wp-block-button">
    <a class="wp-block-button__link has-cyan-bluish-gray-background-color has-background has-custom-font-size wp-element-button" style="border-radius:999px;font-size:14px;font-weight:600">Learn React</a>
  </div>
  <!-- /wp:button -->
</div>
<!-- /wp:buttons -->
```

## Structural Anti-Patterns (Avoid)

These patterns often trigger block validation/recovery in the editor:

```html
<!-- Invalid: paragraph block containing another paragraph element -->
<!-- wp:paragraph -->
<p><p>Text</p></p>
<!-- /wp:paragraph -->

<!-- Invalid: duplicate anchor wrapper in a button -->
<!-- wp:button -->
<div class="wp-block-button"><a class="wp-block-button__link wp-element-button"><a>Label</a></a></div>
<!-- /wp:button -->

<!-- Invalid: button with borderColor but missing utility classes -->
<!-- wp:button {"borderColor":"white"} -->
<div class="wp-block-button">
  <a class="wp-block-button__link wp-element-button">Label</a>
</div>
<!-- /wp:button -->
<!-- Should include: has-border-color has-white-border-color classes -->

<!-- Invalid: button with custom fontSize but missing utility class -->
<!-- wp:button {"style":{"typography":{"fontSize":"1.5rem"}}} -->
<div class="wp-block-button">
  <a class="wp-block-button__link wp-element-button" style="font-size:1.5rem">Label</a>
</div>
<!-- /wp:button -->
<!-- Should include: has-custom-font-size class -->

<!-- Invalid: button attrs/style mismatch (real-world case) -->
<!-- wp:button {"backgroundColor":"cyan-bluish-gray","style":{"typography":{"fontSize":"14px","fontWeight":"600"}}} -->
<div class="wp-block-button">
  <a class="wp-block-button__link has-cyan-bluish-gray-background-color has-background wp-element-button" style="font-size:14px;font-weight:600">Get Started</a>
</div>
<!-- /wp:button -->
<!-- Should include: has-custom-font-size to match attrs -->
```

Use exactly one wrapper expected by the block serializer. Keep classes minimal and canonical for non-button blocks, and maintain strict utility-class parity for `core/button` when attrs require it.

## Critical Media Block Rules

**Always follow these for image-containing blocks:**

```html
<!-- wp:image -->
<figure class="wp-block-image">
  <img alt=""/>
</figure>
<!-- /wp:image -->
```

❌ **Never:** `<img src="..." alt="..."/>`  
✅ **Always:** `<img alt=""/>`

See [Media Handling Guide](./media-handling.md) for comprehensive media block requirements.

## Responsive Behavior

For mobile stacking in columns or media-text:

```html
<!-- wp:columns {"isStackedOnMobile":true} -->
<div class="wp-block-columns is-stacked-on-mobile">
  <!-- Columns will stack on mobile -->
</div>
<!-- /wp:columns -->
```

## Reference for Full Block Syntax

For complete attribute lists and block-specific structure:
- [Block Selection Guide](./block-selection.md) – All 30+ supported blocks
- [Media Handling Guide](./media-handling.md) – Image, cover, gallery, video blocks
- Official WordPress Block Editor Handbook: https://developer.wordpress.org/block-editor/
