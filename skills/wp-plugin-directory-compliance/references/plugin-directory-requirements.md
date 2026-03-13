# WordPress Plugin Directory Requirements

This guide is intentionally scoped to **WordPress.org plugin review/submission requirements** and related compliance checks.

Generic coding standards (PHP/JS/CSS conventions, baseline sanitization/escaping, hooks, architecture patterns) are already covered by the official WordPress skills in `~/.github/skills/`.

Use this file as a **review-gate checklist** before packaging or submitting a plugin.

## WordPress.org Plugin Directory Requirements

**CRITICAL requirements specific to WordPress.org plugin submission:**

### Exception Messages MUST Be Escaped

```php
// BAD - Dynamic content not escaped
throw new Exception( 'Failed for: ' . $user_input );

// GOOD - Dynamic content escaped
throw new Exception( 'Failed for: ' . esc_html( $user_input ) );

// GOOD - Error codes escaped
throw new Exception( 'API returned status ' . esc_html( $status_code ) );

// GOOD - Connection names escaped
throw new Exception( 'Connection failed: ' . esc_html( $connection_name ) );
```

### Nonce Verification Must Sanitize Input

```php
// WordPress.org requires sanitizing the nonce itself
if ( ! isset( $_POST['my_nonce'] ) ||
     ! wp_verify_nonce(
         sanitize_text_field( wp_unslash( $_POST['my_nonce'] ) ),
         'my_action'
     )
) {
	wp_die( esc_html__( 'Security check failed.', 'textdomain' ) );
}
```

### Direct File Access Protection

```php
// Add to TOP of ALL PHP files
if ( ! defined( 'ABSPATH' ) ) {
	exit; // Exit if accessed directly
}
```

### GPL License (Mandatory)

**Plugin Header:**
```php
/**
 * Plugin Name: My Plugin
 * Plugin URI: https://example.com
 * Description: Plugin description
 * Version: 1.0.0
 * Author: Your Name
 * Author URI: https://example.com
 * License: GPLv2 or later
 * License URI: https://www.gnu.org/licenses/gpl-2.0.html
 * Text Domain: my-plugin
 */
```

**Readme.txt:**
```
License: GPLv2 or later
License URI: https://www.gnu.org/licenses/gpl-2.0.html
```

**All Libraries Must Be GPL-Compatible:**
- Check licenses of ALL third-party code
- Include `composer.json` if using Composer
- Document library licenses in readme

### Code Must Be Human-Readable

**Requirements:**
- No obfuscated code (no packers, manglers, `$z12sdf813d` variable names)
- Include source code for ALL minified JS/CSS
- Include build tool documentation

**Options:**
1. Include source in `/src/` folder alongside `/build/` or `/dist/`
2. Link to public GitHub repository in readme.txt
3. Include build instructions in README.md

**Example readme.txt:**
```
== Installation ==

This plugin uses build tools. Source code is available at:
https://github.com/yourname/plugin-name

To build from source:
1. npm install
2. npm run build
```

### Unique Prefixes - More Strict Than General WordPress

**WordPress.org requires unique prefixes (4+ characters recommended):**

**NEVER use these prefixes:**
- `wp_` (reserved for WordPress core)
- `__` (double underscore - reserved)
- `_` (single underscore - reserved)
- Short prefixes (2-3 letters) - too common, will cause conflicts

### WordPress Libraries Only

**Use WordPress bundled versions:**
- jQuery (via `wp_enqueue_script('jquery')`)
- Backbone, Underscore
- SimplePie
- PHPMailer
- PHPass

**NEVER bundle these yourself** - use WordPress versions:
```php
// Enqueue WordPress jQuery
wp_enqueue_script( 'jquery' );

// Use WordPress HTTP API, not CURL
wp_remote_get( $url );
wp_remote_post( $url, $args );
```

**List of core scripts:** https://developer.wordpress.org/reference/functions/wp_enqueue_script/#default-scripts-and-js-libraries-included-and-registered-by-wordpress

### Direct Database Queries (Warnings - Acceptable with Justification)

**When Direct Queries Are ACCEPTABLE:**
- Batch operations with custom JOINs
- Complex queries WordPress does not support
- Performance-critical operations

**How to Handle:**
```php
// Add phpcs:ignore comment explaining WHY
// phpcs:ignore WordPress.DB.DirectDatabaseQuery.DirectQuery, WordPress.DB.DirectDatabaseQuery.NoCaching -- Required for batch sync with custom JOIN on wp_posts
$total_count = (int) $wpdb->get_var( $count_query );
```

**Always explain:**
- Why direct query is necessary
- Why caching is not appropriate
- What makes it safe (for example, "filtered via JOIN", "admin-only", "batch operation")

### Plugin File Requirements

**Main File Naming:**
- File name MUST match plugin slug
- Example: Slug `my-plugin` -> Main file `my-plugin.php`

**Required Headers:**
```php
/**
 * Plugin Name: My Plugin Name
 * Plugin URI: https://example.com/plugin
 * Description: Brief description
 * Version: 1.0.0
 * Requires at least: 6.0
 * Requires PHP: 7.4
 * Author: Your Name
 * Author URI: https://example.com
 * License: GPLv2 or later
 * License URI: https://www.gnu.org/licenses/gpl-2.0.html
 * Text Domain: my-plugin
 * Domain Path: /languages
 */
```

**Required readme.txt:** Must validate at https://wordpress.org/plugins/about/validator/

### Third-Party Services (Must Disclose)

**If plugin calls external APIs, MUST document in readme.txt:**

```
== Description ==

This plugin connects to Example Service API to provide [functionality].

**External Service Usage:**

This plugin sends data to Example Service (https://example.com) when:
- User clicks "Analyze" button
- Automatic daily sync is enabled

Data sent includes: post title, content, metadata

Service Terms: https://example.com/terms
Privacy Policy: https://example.com/privacy

No data is sent without explicit user action or opt-in.
```

**Requirements:**
- Clearly state what service is used
- Link to service URL
- Link to terms of service
- Link to privacy policy
- Explain what data is sent
- Explain when data is sent

### Internationalization - Text Domain Must Match Slug

**WordPress.org enforces text domain = plugin slug:**
```php
// If slug is 'my-awesome-plugin', text domain MUST be same
esc_html__( 'Settings', 'my-awesome-plugin' );
```

**NEVER use variables in i18n functions:**
```php
// BAD - Translators cannot see this
__( $variable, 'textdomain' );

// GOOD - String visible to translators
__( 'Settings Page', 'textdomain' );
```

## Compliance Validation Tools

**Primary review tool:**
- **Plugin Check (PCP)**: official checker used for WordPress.org readiness.

**Recommended companion checks:**
- PHPCS with WordPress ruleset
- ESLint / Stylelint for asset quality

**Understanding Plugin Check Results:**
- ERROR - fix before submission
- WARNING - fix when possible; justify exceptions
- INFO - advisory guidance

## Focused References

- [Detailed Plugin Guidelines (WordPress.org)](https://developer.wordpress.org/plugins/wordpress-org/detailed-plugin-guidelines/)
- [Plugin Check plugin](https://wordpress.org/plugins/plugin-check/)
- [Readme Validator](https://wordpress.org/plugins/about/validator/)
- [Default scripts bundled by WordPress](https://developer.wordpress.org/reference/functions/wp_enqueue_script/#default-scripts-and-js-libraries-included-and-registered-by-wordpress)
