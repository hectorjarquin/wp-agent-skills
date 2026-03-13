# WordPress Coding Standards: Tooling Setup

Sources:
- https://github.com/WordPress/WordPress-Coding-Standards
- https://developer.wordpress.org/coding-standards/

---

## PHP: PHP_CodeSniffer + WPCS

### Installation (Composer — required)

```bash
composer require --dev \
  wp-coding-standards/wpcs \
  phpcsstandards/phpcsutils \
  phpcsstandards/phpcsextra \
  dealerdirect/phpcodesniffer-composer-installer
```

The `dealerdirect/phpcodesniffer-composer-installer` package automatically registers WPCS as a PHPCS standard after install.

Verify:
```bash
./vendor/bin/phpcs -i
# Should list: WordPress, WordPress-Core, WordPress-Extra, WordPress-Docs, etc.
```

---

### phpcs.xml configuration skeleton

Place `phpcs.xml` (or `.phpcs.xml.dist`) in the plugin/theme root:

```xml
<?xml version="1.0"?>
<ruleset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         name="My Plugin"
         xsi:noNamespaceSchemaLocation="vendor/squizlabs/php_codesniffer/phpcs.xsd">

    <description>Coding standards for My Plugin</description>

    <!-- Scan these paths -->
    <file>.</file>

    <!-- Exclude build artifacts and vendor -->
    <exclude-pattern>./vendor/*</exclude-pattern>
    <exclude-pattern>./node_modules/*</exclude-pattern>
    <exclude-pattern>./build/*</exclude-pattern>
    <exclude-pattern>./dist/*</exclude-pattern>

    <!-- Target PHP version -->
    <config name="testVersion" value="7.4-"/>

    <!-- WordPress conventions -->
    <config name="minimum_wp_version" value="6.0"/>

    <!-- Apply the full WordPress ruleset -->
    <rule ref="WordPress">
        <!-- Allow short array syntax [] — WPCS still flags some; this silences it -->
        <exclude name="Universal.Arrays.DisallowShortArraySyntax"/>
    </rule>

    <!-- Tell WPCS what text domain to enforce -->
    <rule ref="WordPress.WP.I18n">
        <properties>
            <property name="text_domain" type="array">
                <element value="my-plugin"/>
            </property>
        </properties>
    </rule>

    <!-- Require doc comments on all public methods -->
    <rule ref="Squiz.Commenting.FunctionComment"/>

</ruleset>
```

**Ruleset hierarchy** (order of strictness, low → high):
| Ruleset | Includes |
|---|---|
| `WordPress-Core` | Core formatting, spacing, naming |
| `WordPress-Extra` | Core + recommended extra checks (deprecated functions, db calls) |
| `WordPress-Docs` | PHPDoc/inline comment standards |
| `WordPress` | Extra + Docs (most complete; recommended default) |

---

### Running PHPCS

```bash
# Lint entire project
./vendor/bin/phpcs

# Lint specific file
./vendor/bin/phpcs includes/class-my-class.php

# Show specific rule violations only
./vendor/bin/phpcs --standard=WordPress-Core --sniffs=WordPress.Arrays.ArrayKeySpacingRestrictions .

# Auto-fix fixable issues
./vendor/bin/phpcbf

# View a summary (no per-line output)
./vendor/bin/phpcs --report=summary
```

---

### WP-CLI / scripts integration

Add Composer scripts for convenience:

```json
{
  "scripts": {
    "lint:php": "phpcs",
    "fix:php": "phpcbf"
  }
}
```

---

## JavaScript: ESLint + @wordpress/eslint-plugin

### Installation

```bash
npm install --save-dev @wordpress/eslint-plugin
```

### `eslint.config.js` skeleton (flat config — ESLint ≥ 9)

```js
import wpRecommended from '@wordpress/eslint-plugin/configs/recommended.js';

export default [
    wpRecommended,
    {
        rules: {
            // Project-specific overrides
        },
    },
];
```

### `.eslintrc.json` skeleton (legacy config — ESLint ≤ 8)

```json
{
    "extends": [
        "plugin:@wordpress/eslint-plugin/recommended"
    ],
    "env": {
        "browser": true,
        "es2021": true
    },
    "rules": {}
}
```

### Running ESLint

```bash
# Lint all JS/TS files
npx eslint src/

# Fix auto-fixable issues
npx eslint --fix src/

# Lint a single file
npx eslint src/index.js
```

---

## CSS: Stylelint + @wordpress/stylelint-config

### Installation

```bash
npm install --save-dev stylelint @wordpress/stylelint-config
```

### `.stylelintrc.json` skeleton

```json
{
    "extends": "@wordpress/stylelint-config",
    "rules": {
        "no-descending-specificity": null
    }
}
```

For SCSS:
```json
{
    "extends": "@wordpress/stylelint-config/scss",
    "rules": {}
}
```

### Running Stylelint

```bash
# Lint CSS
npx stylelint "**/*.css"

# Lint SCSS
npx stylelint "**/*.scss"

# Fix auto-fixable issues
npx stylelint --fix "**/*.css"
```

---

## HTML: markuplint (optional)

For HTML validation in server-rendered templates, [markuplint](https://markuplint.dev/) can enforce:
- Attribute quoting
- Void element syntax
- Accessible attributes (aria, alt, etc.)

```bash
npm install --save-dev markuplint
```

---

## Pre-commit hook (optional but recommended)

Use `lint-staged` to run linters only on changed files:

```bash
npm install --save-dev lint-staged husky
```

In `package.json`:
```json
{
    "lint-staged": {
        "*.php": "composer run lint:php",
        "*.{js,ts}": "eslint --fix",
        "*.{css,scss}": "stylelint --fix"
    }
}
```

---

## @wordpress/scripts integration

If the project uses `@wordpress/scripts`, the ESLint and Stylelint configs are already wired in:

```bash
# Lint JS (uses wp-scripts internally)
npx wp-scripts lint-js src/

# Lint CSS/SCSS
npx wp-scripts lint-style src/

# Lint Markdown
npx wp-scripts lint-md-docs
```

The `wp-scripts` package references `@wordpress/eslint-plugin/recommended` and `@wordpress/stylelint-config` automatically. No additional config files needed unless overriding rules.

---

## Summary: minimum tooling per project type

| Project type | Required | Recommended |
|---|---|---|
| PHP plugin or theme | `phpcs` + WPCS | `phpcbf` auto-fix |
| Block (JS) | `@wordpress/eslint-plugin` | `@wordpress/scripts lint-js` |
| Block (CSS/SCSS) | `@wordpress/stylelint-config` | `@wordpress/scripts lint-style` |
| Full plugin with blocks | All of the above | `lint-staged` pre-commit |
