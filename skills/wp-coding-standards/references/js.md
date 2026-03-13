# WordPress JavaScript Coding Standards

Source: https://developer.wordpress.org/coding-standards/wordpress-coding-standards/javascript/

---

## Overview

WordPress JavaScript follows a style derived from the jQuery style guide. For modern `@wordpress/*` packages written in ES2015+, these rules still apply but are enforced via `@wordpress/eslint-plugin`.

---

## Indentation

Use **tabs** (same as PHP). Do not use spaces for indentation.

```js
// ✅
function myFunction() {
	const result = doSomething();

	if ( result ) {
		handleResult( result );
	}
}
```

---

## Semicolons

Always include semicolons. Do not rely on ASI (Automatic Semicolon Insertion).

```js
// ✅
const value = 42;
doSomething();

// ❌
const value = 42
doSomething()
```

---

## Quotes

Use **single quotes** for strings, unless the string contains a single quote or you need template literals.

```js
// ✅
const label = 'Save Changes';
const html = '<span class="label">Name</span>';

// ✅ Template literal (when interpolation or multiline needed)
const message = `Hello, ${ name }!`;

// ❌
const label = "Save Changes";
```

---

## Naming conventions

| Context | Convention | Example |
|---|---|---|
| Variables | `camelCase` | `postTitle`, `isLoading` |
| Functions | `camelCase` | `getPostData()`, `handleClick()` |
| Constructors / Classes | `PascalCase` | `PostEditor`, `MyComponent` |
| Constants | `UPPER_CASE_WITH_UNDERSCORES` | `MAX_RETRIES`, `API_URL` |
| Private/internal | `_camelCase` (prefix underscore) | `_handleInternal()` |
| WordPress hooks (JS) | `camelCase` | `addFilter`, `applyFilters` |

---

## Equality

Always use **strict equality** (`===` and `!==`). Never use `==` or `!=` unless you explicitly need type coercion (which is uncommon and must be commented).

```js
// ✅
if ( value === 'publish' ) {
if ( count !== 0 ) {

// ❌
if ( value == 'publish' ) {
```

---

## Spacing

Spaces inside parentheses for control structures (mirrors PHP style):

```js
// ✅
if ( condition ) {
for ( let i = 0; i < items.length; i++ ) {
while ( isRunning ) {

// ❌
if(condition) {
```

No space between function name and parenthesis in calls:

```js
// ✅
doSomething( arg );
const result = getValue();

// ❌
doSomething ( arg );
```

Spaces around operators:

```js
// ✅
const total = count + 1;
const label = prefix + ' ' + suffix;

// ❌
const total=count+1;
```

---

## Objects and arrays

Use **trailing commas** in multiline object literals and arrays:

```js
// ✅
const config = {
	postType: 'post',
	status: 'publish',
	count: 10,
};

const items = [
	'alpha',
	'beta',
	'gamma',
];
```

Use **shorthand property names** where applicable (ES2015+):

```js
// ✅
const { title, content } = post;
const updated = { ...post, title: newTitle };
```

---

## Functions

Prefer **named function declarations** over anonymous function expressions for top-level reusable functions. Use arrow functions for callbacks and short expressions:

```js
// ✅ Named function declaration
function fetchPosts( query ) {
	// ...
}

// ✅ Arrow function as callback
items.forEach( ( item ) => {
	process( item );
} );

// ✅ Short arrow
const double = ( n ) => n * 2;
```

Always include parentheses around arrow function parameters, even single ones:

```js
// ✅
const square = ( n ) => n * n;

// ❌
const square = n => n * n;
```

---

## Strict mode

In non-module contexts, declare `'use strict';` at the top of each function scope (or file when appropriate):

```js
( function() {
	'use strict';

	// Plugin code.
} )();
```

Modern `@wordpress/*` packages in ES modules do not require `'use strict'` (modules are strict by default).

---

## No `eval()`

Never use `eval()`. It is a security risk and violates WordPress coding standards.

```js
// ❌ Never
eval( userInput );
```

---

## Global variables and namespacing

When writing scripts that run in the global scope (not via `@wordpress/*` packages), namespace everything under a single global object:

```js
// ✅ Namespaced
window.MyPlugin = window.MyPlugin || {};
MyPlugin.init = function() { ... };

// ❌ Polluting global scope
function myPluginInit() { ... }
```

---

## JSDoc

All exported/public functions and classes require JSDoc:

```js
/**
 * Retrieves posts matching the given query.
 *
 * @since 1.0.0
 *
 * @param {Object}  query          Query parameters.
 * @param {string}  query.postType Post type slug. Default 'post'.
 * @param {number}  query.limit    Maximum number of results. Default 10.
 * @return {Promise<Array>} Resolving to an array of post objects.
 */
async function fetchPosts( query = {} ) {
```

---

## WordPress-specific APIs

Prefer WordPress JS APIs over native equivalents when available:

| Instead of | Use |
|---|---|
| `jQuery.ajax()` | `wp.apiFetch()` |
| Custom event bus | `@wordpress/hooks` (`addFilter`, `applyFilters`, `addAction`, `doAction`) |
| Local storage manually | `@wordpress/data` store where appropriate |
| `console.log` in production | Remove before commit |

---

## Modern WordPress (`@wordpress/*` packages)

When writing blocks or editor extensions, follow the additional conventions in `wp-block-development` skill. Core rules that still apply:

- Import specificity: `import { useState } from '@wordpress/element'` not React directly.
- Avoid `window.*` globals when a `@wordpress/*` equivalent exists.
- Use `@wordpress/i18n` for translations: `__()`, `_n()`, `_x()`, `sprintf()`.

```js
// ✅
import { __ } from '@wordpress/i18n';
const label = __( 'Save Changes', 'my-plugin' );

// ❌
const label = 'Save Changes'; // not translatable
```
