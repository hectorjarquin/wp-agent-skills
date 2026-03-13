---
name: wp-coding-standards
description: "Use when generating or reviewing PHP, JavaScript, CSS, or HTML code in any WordPress context (plugins, themes, block themes, mu-plugins, Gutenberg blocks) to enforce WordPress Coding Standards (WPCS). Covers naming conventions, formatting, sanitization/escaping patterns, SQL safety, and tooling setup."
compatibility: "Targets WordPress 6.9+ (PHP 7.4+). Applies to all WordPress project types."
---

# WordPress Coding Standards

## When to use

Use this skill whenever you are generating or reviewing WordPress code, regardless of project type:

- writing or editing PHP (plugins, themes, functions.php, mu-plugins, CLI scripts)
- writing or editing JavaScript / TypeScript for WordPress (vanilla, jQuery, `@wordpress/*` packages)
- writing or editing CSS / SCSS for WordPress frontend or admin
- writing or editing HTML templates or block markup
- setting up linting/PHPCS tooling for a WordPress project

Do **not** use this skill for:

- plugin submission readiness (use `wp-plugin-directory-compliance`)
- block implementation details (use `wp-block-development`)
- Gutenberg block markup serialization (use `wp-image-to-blocks`)
- PHPStan static analysis (use `wp-phpstan`)

## Authoritative sources

- PHP: https://developer.wordpress.org/coding-standards/wordpress-coding-standards/php/
- JavaScript: https://developer.wordpress.org/coding-standards/wordpress-coding-standards/javascript/
- CSS: https://developer.wordpress.org/coding-standards/wordpress-coding-standards/css/
- HTML: https://developer.wordpress.org/coding-standards/wordpress-coding-standards/html/
- WPCS tooling: https://github.com/WordPress/WordPress-Coding-Standards

## Procedure

### 1) Identify the language being generated

Use only the relevant reference:

- PHP code → `./references/php.md`
- JavaScript / TypeScript → `./references/js.md`
- CSS / SCSS → `./references/css.md`
- HTML templates / markup → `./references/html.md`
- Tooling setup → `./references/tooling.md`

### 2) Apply standards before returning code

Do not volunteer code without first checking the primary rules for that language. Key defaults that apply everywhere:

- **PHP**: tabs for indentation, Yoda conditions, snake_case functions/variables, PascalCase classes, `UPPER_CASE` constants, always sanitize input and escape output.
- **JS**: single quotes, strict equality (`===`), camelCase functions/variables, JSDoc for public APIs.
- **CSS**: lowercase hyphen-delimited selectors, alphabetical property order within rules.
- **HTML**: lowercase elements and attribute names, double-quoted attribute values, semantic structure.

### 3) Tooling setup (when asked to configure linting)

See `./references/tooling.md` for:

- `phpcs.xml` / `.phpcs.xml.dist` configuration for WPCS
- `composer.json` dev dependencies
- `.eslintrc` / `eslint.config.js` for WordPress JS
- Stylelint configuration for WordPress CSS

## Verification

Before returning any generated code, confirm:

- [ ] Indentation: tabs in PHP and JS, 2 spaces in HTML/CSS (or tabs for HTML per project)
- [ ] No space between function name and opening parenthesis in PHP
- [ ] Spaces inside parentheses for control structures in PHP: `if ( $x )`, not `if($x)`
- [ ] Yoda conditions used for equality comparisons in PHP
- [ ] Sanitization at input boundaries (`sanitize_text_field`, `intval`, `wp_unslash`, etc.)
- [ ] Escaping at output boundaries (`esc_html`, `esc_attr`, `esc_url`, `wp_kses_post`, etc.)
- [ ] SQL built with `$wpdb->prepare()`, never raw string interpolation
- [ ] No bundled copies of WordPress-core libraries (jQuery, etc.)
- [ ] Strict equality in JS (`===` / `!==`)
- [ ] Single quotes in JS, double quotes in HTML attribute values

## References

- [PHP Coding Standards](./references/php.md)
- [JavaScript Coding Standards](./references/js.md)
- [CSS Coding Standards](./references/css.md)
- [HTML Coding Standards](./references/html.md)
- [Tooling Setup](./references/tooling.md)
