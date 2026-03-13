# Media Handling for Gutenberg Blocks

**⚠️ CRITICAL**: These rules prevent "Attempt block recovery" errors in WordPress. Follow them strictly for all media Gutenberg blocks.

## CRITICAL MEDIA RULES

### Rule 1: NEVER Include `src=` Attributes

❌ **WRONG**:
```html
<img src="image.jpg" alt=""/>
```

✅ **CORRECT**:
```html
<img alt=""/>
```

Media placeholders must be empty. WordPress handles image upload on paste into the editor.

### Rule 2: ALWAYS Use Empty `alt` Attributes

❌ **WRONG**:
```html
<img alt="descriptive text"/>
```

✅ **CORRECT**:
```html
<img alt=""/>
```

The `alt` attribute must be empty in generated placeholders.

### Rule 3: NO `style` Attributes Directly on `<img>` Tags

❌ **WRONG**:
```html
<img alt="" style="width:100%; height:auto;"/>
```

✅ **CORRECT**:
```html
<img alt=""/>
```

All styling goes in Gutenberg block attributes or wrapper classes, not on img tags.

### Rule 4: ALWAYS Use Proper Gutenberg Block Class Names

Each media block type has required class names:

#### `core/image`
```html
<!-- wp:image -->
<figure class="wp-block-image">
  <img alt=""/>
</figure>
<!-- /wp:image -->
```

#### `core/cover`
```html
<!-- wp:cover -->
<div class="wp-block-cover">
  <img class="wp-block-cover__image-background" alt=""/>
  <!-- inner content -->
</div>
<!-- /wp:cover -->
```

#### `core/media-text`
```html
<!-- wp:media-text -->
<div class="wp-block-media-text">
  <figure class="wp-block-media-text__media">
    <img alt=""/>
  </figure>
  <div class="wp-block-media-text__content">
    <!-- content -->
  </div>
</div>
<!-- /wp:media-text -->
```

### Rule 5: DO NOT Add Extra Classes to Media HTML

Use only the structural classes required by the media block type.

❌ **WRONG**:
```html
<!-- wp:image -->
<figure class="wp-block-image size-large custom-card-image has-custom-font-size">
  <img alt=""/>
</figure>
<!-- /wp:image -->
```

✅ **CORRECT**:
```html
<!-- wp:image -->
<figure class="wp-block-image">
  <img alt=""/>
</figure>
<!-- /wp:image -->
```

For screenshot-to-block output:
- Keep `core/image` markup minimal.
- Do not add decorative or inferred classes to media wrappers.
- Use alignment classes (`aligncenter`, `alignwide`, etc.) only when block attributes explicitly set alignment.

## Standard Gutenberg Block Implementations

### Image Block (`core/image`)

**Basic image**:
```html
<!-- wp:image -->
<figure class="wp-block-image">
  <img alt=""/>
</figure>
<!-- /wp:image -->
```

**Aligned image**:
```html
<!-- wp:image {"align":"center"} -->
<figure class="wp-block-image aligncenter">
  <img alt=""/>
</figure>
<!-- /wp:image -->
```

**Wide image**:
```html
<!-- wp:image {"align":"wide"} -->
<figure class="wp-block-image alignwide">
  <img alt=""/>
</figure>
<!-- /wp:image -->
```

### Cover Block (`core/cover`) with Background Image

```html
<!-- wp:cover -->
<div class="wp-block-cover">
  <img class="wp-block-cover__image-background" alt=""/>
  <div class="wp-block-cover__inner-container">
    <!-- wp:paragraph -->
    <p>Overlay content here</p>
    <!-- /wp:paragraph -->
  </div>
</div>
<!-- /wp:cover -->
```

**Critical for cover blocks**: Use `wp-block-cover__image-background` class, not just any class.

### Media & Text Block (`core/media-text`)

```html
<!-- wp:media-text {"align":"wide","isStackedOnMobile":true} -->
<div class="wp-block-media-text alignwide is-stacked-on-mobile">
  <figure class="wp-block-media-text__media">
    <img alt=""/>
  </figure>
  <div class="wp-block-media-text__content">
    <!-- wp:heading -->
    <h2 class="wp-block-heading">Heading</h2>
    <!-- /wp:heading -->
    
    <!-- wp:paragraph -->
    <p>Content text here</p>
    <!-- /wp:paragraph -->
  </div>
</div>
<!-- /wp:media-text -->
```

### Gallery Block (`core/gallery`)

```html
<!-- wp:gallery {"columns":3,"linkTo":"none"} -->
<figure class="wp-block-gallery has-nested-images columns-3 is-cropped">
  <!-- wp:image -->
  <figure class="wp-block-image">
    <img alt=""/>
  </figure>
  <!-- /wp:image -->
  
  <!-- wp:image -->
  <figure class="wp-block-image">
    <img alt=""/>
  </figure>
  <!-- /wp:image -->
  
  <!-- wp:image -->
  <figure class="wp-block-image">
    <img alt=""/>
  </figure>
  <!-- /wp:image -->
</figure>
<!-- /wp:gallery -->
```

## Pre-Submission Validation Checklist

Before returning generated Gutenberg blocks, verify:

- [ ] **No `src=` attributes** on ANY `<img>` tags
- [ ] **All `alt` attributes are empty** (`alt=""`)
- [ ] **No `style` attributes** directly on img tags
- [ ] **Correct class names** for each Gutenberg block type
- [ ] **No extra classes on media wrappers** beyond required structural/alignment classes
- [ ] **Proper wrapper structure** (e.g., `<figure>` for images)
- [ ] **All block delimiters present** (`<!-- wp:blockname -->` and `<!-- /wp:blockname -->`)

## Upstream References

For detailed implementation specifications, see:

- WordPress Image Block: https://developer.wordpress.org/block-editor/reference-guides/core-blocks/image/
- WordPress Cover Block: https://developer.wordpress.org/block-editor/reference-guides/core-blocks/cover/
- WordPress Media & Text Block: https://developer.wordpress.org/block-editor/reference-guides/core-blocks/media-text/
- WordPress Gallery Block: https://developer.wordpress.org/block-editor/reference-guides/core-blocks/gallery/

---

**These rules are non-negotiable.** Violating them causes WordPress to trigger block recovery, which breaks the user experience.
