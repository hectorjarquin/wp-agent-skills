# WordPress HTML Coding Standards

Source: https://developer.wordpress.org/coding-standards/wordpress-coding-standards/html/

---

## Validation

All HTML must be valid and pass W3C validation. Use semantic markup to convey document structure correctly.

---

## Indentation

Use **tabs** for indentation, with nested elements indented one tab deeper than their parent:

```html
<!-- ✅ -->
<div class="site-header">
	<nav class="main-navigation">
		<ul>
			<li><a href="/">Home</a></li>
		</ul>
	</nav>
</div>

<!-- ❌ – 4-space indentation -->
<div class="site-header">
    <nav class="main-navigation">
```

---

## Attribute values

Always use **double quotes** for attribute values. Never use single quotes or unquoted values in HTML:

```html
<!-- ✅ -->
<input type="text" name="user_email" class="regular-text">
<a href="/contact" class="button button-primary">Contact</a>

<!-- ❌ – single quotes -->
<input type='text' name='user_email'>

<!-- ❌ – unquoted -->
<input type=text>
```

---

## Lowercase elements and attributes

All HTML element names and attribute names must be **lowercase**:

```html
<!-- ✅ -->
<p class="entry-content">
<img src="" alt="">
<a href="#" target="_blank">

<!-- ❌ -->
<P CLASS="entry-content">
<IMG SRC="" ALT="">
```

---

## Self-closing tags

Void elements must **not** use a closing slash in HTML5. Do not add ` />` to elements like `<img>`, `<br>`, `<input>`, `<hr>`, `<meta>`, `<link>`:

```html
<!-- ✅ HTML5 -->
<img src="" alt="Description">
<br>
<input type="text" name="q">
<hr>

<!-- ❌ – XHTML-style self-closing -->
<img src="" alt="Description" />
<br />
<input type="text" name="q" />
```

**Exception:** In Gutenberg block markup (`.html` template files or block `save` output), React-style JSX uses `<img />`. This is acceptable within the JSX/block context only.

---

## Boolean attributes

Boolean HTML attributes should be written without a value:

```html
<!-- ✅ -->
<input type="checkbox" checked>
<input type="text" disabled>
<script defer src="...">

<!-- ❌ -->
<input type="checkbox" checked="checked">
<input type="text" disabled="disabled">
```

---

## Semantic structure

Use semantic elements to convey meaning:

```html
<!-- ✅ Semantic -->
<header class="site-header">
<main class="site-main">
<article class="post">
<aside class="widget-area">
<footer class="site-footer">
<nav class="main-navigation">
<section class="featured-posts">

<!-- ❌ Non-semantic -->
<div id="header">
<div id="content">
<div id="footer">
```

---

## Accessibility

Every HTML element that conveys information or enables interaction must be accessible:

### Images

Always include a meaningful `alt` attribute on `<img>`:

```html
<!-- ✅ Descriptive alt for informative image -->
<img src="team-photo.jpg" alt="The Acme team at the 2024 company retreat">

<!-- ✅ Empty alt for decorative image -->
<img src="decorative-divider.svg" alt="">

<!-- ❌ Missing alt -->
<img src="photo.jpg">
```

### Links

Link text must describe the destination — no "click here" or bare URLs:

```html
<!-- ✅ -->
<a href="/about">About our company</a>
<a href="/report.pdf">Download the 2024 Annual Report (PDF)</a>

<!-- ❌ -->
<a href="/about">Click here</a>
<a href="/about">https://example.com/about</a>
```

### Headings

Do not skip heading levels. One `<h1>` per page (typically the post/page title):

```html
<!-- ✅ -->
<h1>Page title</h1>
<h2>Section heading</h2>
<h3>Subsection</h3>

<!-- ❌ – skipping from h1 to h3 -->
<h1>Page title</h1>
<h3>Section heading</h3>
```

### Form labels

Every form input must have a visible or accessible label:

```html
<!-- ✅ Explicit for/id association -->
<label for="user-email">Email address</label>
<input type="email" id="user-email" name="email">

<!-- ✅ aria-label when visual label not practical -->
<input type="search" aria-label="Search posts" name="s">

<!-- ❌ No label -->
<input type="email" name="email">
```

### ARIA

Use ARIA roles and attributes only when native semantic HTML is insufficient:

```html
<!-- ✅ – using semantic HTML (preferred) -->
<nav>
<main>
<button type="button">

<!-- ✅ – ARIA when semantic isn't sufficient -->
<div role="tabpanel" aria-labelledby="tab-general">

<!-- ❌ – unnecessary ARIA on semantic elements -->
<nav role="navigation">
<main role="main">
<button role="button">
```

---

## PHP + HTML templates

When mixing PHP and HTML in template files:

- Keep PHP logic minimal in templates; move complex logic to functions.
- Close HTML tags in the same scope they were opened, or make unclosed tags obvious:

```php
<!-- ✅ Clear opening/closing within same if block -->
<?php if ( have_posts() ) : ?>
	<div class="post-listing">
		<?php while ( have_posts() ) : the_post(); ?>
			<article id="post-<?php the_ID(); ?>" <?php post_class(); ?>>
				<h2><?php the_title(); ?></h2>
			</article>
		<?php endwhile; ?>
	</div>
<?php endif; ?>
```

Use the alternative syntax (`if():… endif;`) for multi-line PHP mixed with HTML — it is significantly more readable than `{}` braces.
