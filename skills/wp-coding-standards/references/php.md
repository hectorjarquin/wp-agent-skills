# WordPress PHP Coding Standards

Source: https://developer.wordpress.org/coding-standards/wordpress-coding-standards/php/

---

## Indentation

Use **real tabs**, not spaces. Tab width is typically 4 spaces in display, but the file must use the tab character.

```php
// ✅ Correct
function my_function() {
	$value = true;

	if ( $value ) {
		do_something();
	}
}

// ❌ Wrong (spaces instead of tabs)
function my_function() {
    $value = true;
}
```

Align array values and multi-line assignments using spaces after the tab when alignment improves readability.

---

## Brace style

Opening braces go on the **same line** as the control structure or function declaration. Always use braces, even for single-line bodies.

```php
// ✅ Correct
if ( $condition ) {
	do_something();
} elseif ( $other ) {
	do_other();
} else {
	fallback();
}

// ✅ Correct – function
function my_function( $arg ) {
	return $arg;
}

// ❌ Wrong – missing braces
if ( $condition )
	do_something();
```

---

## Spacing

**Control structures** (`if`, `else`, `elseif`, `for`, `foreach`, `while`, `switch`) require spaces inside parentheses:

```php
// ✅
if ( $x === 1 ) {
foreach ( $items as $item ) {
while ( $running ) {

// ❌
if($x === 1) {
foreach($items as $item) {
```

**Function calls** do NOT have a space between the function name and the opening parenthesis:

```php
// ✅
$result = my_function( $arg );

// ❌
$result = my_function ( $arg );
```

**Operators** — put spaces on both sides:

```php
// ✅
$x = $a + $b;
$result = $foo . $bar;

// ❌
$x=$a+$b;
```

**Concatenation** — single space on each side of the `.` operator:

```php
// ✅
$full = $first . ' ' . $last;

// ❌
$full = $first.' '.$last;
```

**Type casts** — space between cast and variable:

```php
// ✅
$int = (int) $value;
$array = (array) $value;
```

---

## Naming conventions

| Context | Convention | Example |
|---|---|---|
| Functions | `lowercase_with_underscores` | `get_post_meta()` |
| Variables | `lowercase_with_underscores` | `$post_id` |
| Classes | `PascalCase` (UpperCamelCase) | `WP_Query`, `My_Plugin` |
| Class methods | `lowercase_with_underscores` | `get_results()` |
| Constants | `UPPER_CASE_WITH_UNDERSCORES` | `ABSPATH`, `MY_PLUGIN_VERSION` |
| Hook names | `lowercase_with_underscores` | `'init'`, `'save_post'` |
| File names | `lowercase-with-hyphens.php` | `class-my-plugin.php` |

Class files must be named `class-{classname}.php` (lowercased, hyphens replace underscores). Non-class files use descriptive lowercase hyphenated names.

---

## Yoda conditions

Put the constant, literal, or function return value on the **left** side of a comparison:

```php
// ✅ Yoda – safe against accidental assignment
if ( true === $is_active ) {
if ( null === $value ) {
if ( 'publish' === $post->post_status ) {
if ( 0 === strpos( $str, 'wp_' ) ) {

// ❌ Non-Yoda
if ( $is_active === true ) {
if ( $post->post_status === 'publish' ) {
```

**Exception**: when both sides are variables, Yoda does not apply.

---

## Ternary operator

Ternaries are allowed but must remain readable. For complex logic, use a full `if/else`. Do **not** use the short ternary `?:` (Elvis) in a way that could be confusing.

```php
// ✅ Simple ternary
$label = ( $count > 1 ) ? 'items' : 'item';

// ✅ Null coalescing (PHP 7+)
$value = $input ?? 'default';

// ❌ Too complex for ternary
$x = ( $a && $b ) ? ( $c ? $d : $e ) : $f;
```

---

## Arrays

Prefer **short array syntax** (`[]`) over `array()`:

```php
// ✅
$items  = [];
$data   = [ 'key' => 'value' ];

// ❌ (legacy, avoid in new code)
$items = array();
```

For multi-line arrays, each element on its own line with trailing comma:

```php
// ✅
$args = [
	'post_type'   => 'post',
	'post_status' => 'publish',
	'numberposts' => 10,
];
```

---

## Commenting

### Inline comments

Use `//` for single-line inline comments. Put the comment on a line by itself when it describes the next statement:

```php
// Retrieve all published posts with a custom field set.
$posts = get_posts( $args );
```

### PHPDoc blocks

Required for all functions, methods, classes, and file headers:

```php
/**
 * Retrieves a formatted list of post titles.
 *
 * @since 1.0.0
 *
 * @param int    $limit   Number of posts to retrieve.
 * @param string $status  Post status. Default 'publish'.
 * @return string[] Array of post titles.
 */
function get_post_titles( $limit = 10, $status = 'publish' ) {
```

---

## Sanitization and escaping

**Sanitize at input, escape at output.**

### Sanitization functions (input)

| Use case | Function |
|---|---|
| Plain text | `sanitize_text_field()` |
| Textarea / multiline | `sanitize_textarea_field()` |
| Email | `sanitize_email()` |
| URL | `esc_url_raw()` |
| Integer | `intval()` / `absint()` |
| Float | `floatval()` |
| HTML | `wp_kses_post()` / `wp_kses()` |
| Filename | `sanitize_file_name()` |
| Key/slug | `sanitize_key()` |
| Unslash before sanitize | `wp_unslash( $_POST['field'] )` → then sanitize |

### Escaping functions (output)

| Context | Function |
|---|---|
| Inside HTML text | `esc_html()` |
| Inside HTML attribute | `esc_attr()` |
| URL in `href`/`src` | `esc_url()` |
| JavaScript inline | `esc_js()` |
| Translatable text | `esc_html__()`, `esc_attr__()`, `esc_html_e()` |
| Trusted HTML | `wp_kses_post()` |

```php
// ✅ Correct pattern
$value = sanitize_text_field( wp_unslash( $_POST['my_field'] ) );

echo '<input value="' . esc_attr( $value ) . '">';
echo '<p>' . esc_html( $message ) . '</p>';
echo '<a href="' . esc_url( $url ) . '">' . esc_html( $label ) . '</a>';
```

---

## SQL — always use `$wpdb->prepare()`

Never concatenate user-controlled values directly into SQL:

```php
// ✅ Safe
$results = $wpdb->get_results(
	$wpdb->prepare(
		'SELECT * FROM %i WHERE user_id = %d AND status = %s',
		$wpdb->users,
		$user_id,
		$status
	)
);

// ❌ Unsafe – SQL injection risk
$results = $wpdb->get_results(
	"SELECT * FROM {$wpdb->users} WHERE user_id = $user_id"
);
```

Placeholders: `%d` integer, `%s` string (auto-quoted), `%f` float, `%i` identifier (table/column name, WP 6.2+).

---

## Null safety and type handling

```php
// ✅ isset() before accessing superglobals
if ( isset( $_POST['action'] ) ) {
	$action = sanitize_key( wp_unslash( $_POST['action'] ) );
}

// ✅ Strict comparison
if ( false === $result ) {

// ❌ Loose comparison
if ( !$result ) {
```

---

## PHP closing tags

**Omit the closing `?>` tag** in PHP-only files to prevent accidental whitespace output:

```php
// ✅ End of file – no closing tag
function my_function() {
	return true;
}
// EOF
```

---

## Short PHP open tags

Always use the full `<?php` tag. Short tags (`<?`) and short echo tags (`<?=`) are not acceptable in WordPress files.

```php
// ✅
<?php echo esc_html( $value ); ?>

// ❌
<? echo esc_html( $value ); ?>
<?= esc_html( $value ) ?>
```

---

## File structure conventions

- One class per file.
- File name matches class name: `class-my-plugin-admin.php` for `My_Plugin_Admin`.
- Files must include the ABSPATH guard at the top:

```php
if ( ! defined( 'ABSPATH' ) ) {
	exit;
}
```

---

## Error handling

Exception messages must escape dynamic content (required for WordPress.org):

```php
// ✅
throw new Exception( 'API error: ' . esc_html( $error_message ) );

// ❌
throw new Exception( 'API error: ' . $error_message );
```
