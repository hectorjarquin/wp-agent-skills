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

### JSON-LD in Script Tags: Use JSON_HEX_TAG

When outputting JSON-LD or any JSON inside a `<script>` tag, use `JSON_HEX_TAG` instead of `JSON_UNESCAPED_SLASHES`. This prevents `</script>` sequence injection that could break page parsing.

```php
// BAD - Allows </script> injection via user data
echo '<script type="application/ld+json">' . wp_json_encode( $data, JSON_UNESCAPED_SLASHES ) . '</script>';

// GOOD - Escapes </script> sequences
echo '<script type="application/ld+json">' . wp_json_encode( $data, JSON_HEX_TAG ) . '</script>';
```

**Note:** `JSON_HEX_TAG` encodes `<` and `>` as `\u003C` and `\u003E`, which prevents premature script tag closure while keeping JSON valid.

### esc_url vs esc_url_raw

Use the right escaping function for the context:

- `esc_url()` - for display/output contexts (href, src, form action, redirect destination shown to user)
- `esc_url_raw()` - for database storage, redirect locations, HTTP API calls (raw URL, no entity encoding)

```php
// BAD - esc_url_raw on output (double-encodes or leaves raw entities)
echo '<a href="' . esc_url_raw( $url ) . '">Link</a>';

// GOOD - esc_url for display
echo '<a href="' . esc_url( $url ) . '">Link</a>';

// GOOD - esc_url_raw for storage
update_option( 'my_plugin_endpoint', esc_url_raw( $endpoint_url ) );
```

WordPress.org reviewers flag `esc_url_raw` when `esc_url` is the appropriate choice. Use `esc_url` for all output unless you specifically need a raw URL for storage or HTTP request context.

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

### Readme Repository Section

If your plugin has minified assets, the readme must document where source code lives. Add a `== Repository ==` section to readme.txt:

```text
== Repository ==

Source code and build instructions:
https://github.com/yourname/your-plugin

To build from source:
1. git clone https://github.com/yourname/your-plugin
2. cd your-plugin
3. npm install
4. npm run build
```

This covers the WordPress.org requirement that all minified code has a human-readable source.

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

**Plugin URI Must Be Reachable:**
- Plugin URI header must return HTTP 200.
- If no dedicated page exists for the plugin, omit the header or use a GitHub repository URL.
- A 404 Plugin URI is a review flag that must be corrected before submission.

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

**Readme.txt template for External Services:**

```text
== External Services ==

Service Name: Example Service
  Purpose: Describe what the service does
  Data sent: List specific data fields
  When sent: Describe trigger conditions (user action, schedule, etc.)
  Terms of Service: https://example.com/tos
  Privacy Policy: https://example.com/privacy
```

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

## Post-Fix Verification Checklist

Before resubmitting to WordPress.org after a review:

- Verify every flagged issue has a fix. Do not submit partial fixes.
- Activate the plugin in a test environment and exercise the fixed code paths.
- Re-run Plugin Check (PCP). Confirm no blocking errors remain.
- Re-read the plugin ZIP to confirm no stale files are packaged.
- Reply to the review thread with a per-item summary of fixes applied.

WordPress.org reviewers expect one resubmission with all issues resolved. If you miss an item, the review will likely be rejected rather than pended again.

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
