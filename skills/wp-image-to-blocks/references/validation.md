# Local Validation Deltas (MCP-First)

Use this file only for rules that are not fully covered by `wp-blockmarkup` MCP validation.

## MCP Is Primary Validator

Always run MCP validation first:

1. `search_blocks` to locate block candidates.
2. `get_block_schema` (and `list_block_attributes` when needed).
3. Generate markup.
4. `validate_markup`.
5. Iterate until valid.

Do not duplicate schema/type/nesting checks here unless they are project-specific exceptions.

## Keep Only Local Delta Rules

### 1) `core/image` Wrapper Policy

- Keep `core/image` wrappers minimal.
- Valid pattern:

```html
<!-- wp:image -->
<figure class="wp-block-image"><img alt=""/></figure>
<!-- /wp:image -->
```

### 2) `core/button` Parity Policy

- Keep `wp-block-button__link` classes synchronized with button attributes.
- If button attrs require class parity (for example custom font size), include the matching class on the link element.

### 3) Output Hygiene

- Return one canonical Gutenberg version only.
- Do not output multiple alternatives in the same block.
- Maintain strict opening/closing block comment parity.
- `core/paragraph` must serialize to exactly one root `<p>` element.
- Never nest `<p>` inside another `<p>` or wrap already-rendered paragraph HTML again.

### 4) Screenshot Fidelity Rule

- Screenshot visuals are source of truth for color/spacing/typography intent.
- If there is a conflict between generic examples and the screenshot, follow the screenshot.

### 5) Serializer Parity Rule (Editor Is Source of Truth)

- Treat Gutenberg editor serialization as canonical.
- If "Attempt recovery" rewrites attrs/classes, prefer the recovered version for final output.
- Preserve serializer-preferred class mappings when observed in the target environment.

Observed normalizations to keep:
- `core/columns`: mobile stacking may serialize as `className:"is-stacked-on-mobile"`.
- `core/separator`: decorative color/styling may be normalized or dropped; prefer the simplest recovered canonical form unless the target editor has already confirmed a styled variant.
- `core/group`: border utility classes may serialize via `className` instead of inline-only intent.
- `core/paragraph`: class ordering and `className` injection can be normalized by the editor.

Rule of thumb:
- After MCP `VALID`, do one editor parity pass for complex sections and keep the recovered serialization if it differs.
- For this environment, prefer recovered `core/columns` mobile-stacking serialization over authored `isStackedOnMobile` when the editor rewrites it.
- Treat `core/separator` as canonical-only: avoid decorative separator styling unless recovery has already confirmed the exact serializer output.

## Quick Local Delta Check (After MCP `VALID`)

1. `core/image` wrapper minimal? Fix if not.
2. `core/button` attr/class parity preserved? Fix if not.
3. `core/paragraph` blocks render as a single root `<p>` with no nested paragraph tags? Fix if not.
4. `core/columns` mobile stacking uses the recovered serializer-preferred form when known? Fix if not.
5. `core/separator` uses a canonical recovered pattern and not an unverified decorative variant? Fix if not.
6. Single canonical output only? Ensure yes.
7. Visual fidelity aligned to screenshot intent? Ensure yes.
8. If editor recovered any block, adopt recovered serialization as canonical.

If all pass, return final markup.
