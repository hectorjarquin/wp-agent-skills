# WordPress CSS Coding Standards

Source: https://developer.wordpress.org/coding-standards/wordpress-coding-standards/css/

---

## Structure of a rule

```css
selector,
selector-two {
	property: value;
	property: value;
}
```

Rules:
- Opening brace on the same line as the last selector.
- One space before the opening brace.
- Each declaration on its own line.
- One tab before each property.
- No space before the colon; one space after the colon.
- Closing brace on its own line.
- One blank line between rules.

```css
/* ✅ */
.site-header,
.site-header-wrap {
	background: #fff;
	border-bottom: 1px solid #eee;
}

/* ❌ – all on one line */
.site-header { background: #fff; border-bottom: 1px solid #eee; }
```

---

## Selectors

- **Lowercase** hyphen-delimited names: `.site-header`, `.post-thumbnail`, `.entry-content`.
- Do **not** use ID selectors (`#header`) for styling; use classes.
- Do **not** use element type selectors combined with classes unnecessarily (`div.entry` → `.entry`).
- Use **BEM-adjacent** naming when applying component-level isolation: `.component`, `.component__element`, `.component--modifier`.

```css
/* ✅ */
.wp-block-group {}
.wp-block-group__inner-container {}
.is-content-justification-left {}

/* ❌ */
#header {}
DIV.entry-content {}
.A {}
```

---

## Property ordering

Group related properties together. Within each group, order **alphabetically**:

1. Display / layout (`display`, `position`, `top`, `right`, `bottom`, `left`, `float`, `clear`, `z-index`)
2. Box model (`width`, `height`, `margin`, `padding`, `border`, `box-sizing`, `overflow`)
3. Typography (`font-*`, `line-height`, `letter-spacing`, `text-*`, `white-space`, `color`)
4. Visual (`background`, `box-shadow`, `opacity`, `cursor`, `visibility`)
5. Animation / transition
6. Generated content (`content`, `counter-*`)

```css
/* ✅ */
.entry-title {
	display: block;
	margin: 0 0 1em;
	color: #333;
	font-size: 1.5em;
	font-weight: 700;
	line-height: 1.3;
}
```

---

## Values

- Use **shorthand properties** where readable: `margin: 0 auto`, not `margin-top: 0; margin-right: auto;`.
- Use **unitless `0`** — never `0px`, `0em`, `0rem`.
- Use **lowercase hex** values: `#fff`, `#3a3a3a`, not `#FFF`, `#3A3A3A`.
- Use **shorthand hex** where applicable: `#fff` not `#ffffff`.
- Use **single quotes** in CSS value strings: `content: 'x'`.
- Omit leading zeros in decimal values: `.5em` not `0.5em`.

```css
/* ✅ */
.element {
	margin: 0;
	color: #3a3a3a;
	background: #fff url( 'image.png' ) center no-repeat;
	opacity: .8;
}

/* ❌ */
.element {
	margin: 0px;
	color: #3A3A3A;
	opacity: 0.8;
}
```

---

## Comments

Single-line for quick notes, section dividers use a consistent format:

```css
/* This is a valid comment. */

/* ==========================================================================
   Section title
   ========================================================================== */

/* Subsection
   ------------------------------------------------------------------ */
```

Do not use `//` for CSS comments (it is not valid CSS, only valid in SCSS preprocessors).

---

## Media queries

- Group media queries with the CSS rule they affect, not at the bottom of the file.
- Mobile-first where applicable (`min-width`).

```css
/* ✅ */
.site-content {
	width: 100%;
}

@media ( min-width: 768px ) {
	.site-content {
		width: 75%;
		float: left;
	}
}
```

---

## Specificity

- Keep specificity low. Prefer single-class selectors.
- Avoid `!important` except for overriding third-party styles or utility classes.
- Document every use of `!important` with a comment explaining why.

```css
/* ✅ – justified override */
.wp-block-button .wp-block-button__link {
	font-size: 1rem !important; /* Override editor inline style from theme.json */
}
```

---

## Vendor prefixes

Do not write vendor prefixes manually. Use Autoprefixer (via PostCSS or build tooling) to add them based on the browser support target. In the source file, write only the standard property:

```css
/* ✅ Source */
.element {
	transform: rotate(45deg);
	transition: color 0.2s ease;
}
```

---

## SCSS / preprocessors (when used)

- WordPress core does not require SCSS, but many plugins and themes use it.
- Follow the same property ordering and naming conventions.
- Variables: `$prefix-property-variant` pattern: `$color-text-primary`, `$font-size-base`.
- Do not nest more than **3 levels** deep.

```scss
// ✅ – 2 levels of nesting, clear structure
.site-header {
	background: $color-header-bg;

	.site-title {
		font-size: $font-size-xl;
	}
}

// ❌ – too deeply nested
.site-header {
	.nav {
		ul {
			li {
				a { }
			}
		}
	}
}
```
