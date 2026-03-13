---
name: wp-site-inventory
description: "Use when you need a comprehensive inventory of an unknown or existing WordPress site: content statistics, custom post types, ACF field groups, custom blocks, theme architecture, plugins, global features, and migration considerations. Generates a structured markdown report for AI analysis."
compatibility: "Requires WP-CLI access to a local WordPress installation. Targets WordPress 5.0+."
---

# WP Site Inventory

## When to use

Use this skill to systematically inventory a WordPress site when:

- Starting work on an unfamiliar client site
- Documenting an existing site before migration or redesign
- Understanding what content, plugins, and custom functionality exist
- Planning a classic-to-FSE theme migration
- Auditing a site before taking over maintenance

**This skill answers:** "What's installed, configured, and running on this WordPress site?"

## Inputs required

- WP-CLI access to a local WordPress installation
- Filesystem read access to theme and plugin directories
- Current working directory should be the WordPress root (or specify path with `--path=`)

## Procedure

### 1. Verify WP-CLI access

Confirm you can communicate with WordPress:

```bash
wp core version
```

If this fails, ensure:
- You're in the WordPress root directory (or use `--path=/path/to/wp`)
- WP-CLI is installed (`wp --info`)
- The site's database is accessible

### 2. Collect inventory data

Run commands from `references/inventory-commands.md` to collect:

1. **Site Metadata** - WordPress version, PHP version, database info, theme info
2. **Content Inventory** - Post type statistics, published/draft/trash counts
3. **Plugins** - Active and inactive plugins with versions
4. **Custom Post Types & Taxonomies** - All registered CPTs and taxonomies
5. **Advanced Custom Fields** - ACF field groups (if ACF is installed)
6. **Custom Blocks** - Registered blocks, shortcodes, block files
7. **Theme Architecture** - Directory structure, file counts, assets
8. **Global Features** - Navigation menus, widgets, customizer settings
9. **Assets & Dependencies** - External CDN dependencies, local assets
10. **Pages Requiring Attention** - Heuristics-based issue detection

Each layer is independent. Run commands sequentially or in parallel batches.

**Graceful handling:**
- If ACF is not installed, skip the ACF section and note "ACF not present" in the report
- If a command fails, log the error and continue with remaining layers
- For large sites (1000+ posts), all queries run in full (comprehensive inventory is the goal)

### 3. Generate markdown report

Use the template from `references/inventory-template.md` to structure the output.

Populate each section with data collected from step 2.

**Required sections:**
- Site Metadata
- Content Inventory (table format)
- Plugins (active/inactive split, table format)
- Custom Post Types
- Custom Blocks
- Theme Architecture
- Global Features

**Optional sections (include if data found):**
- Advanced Custom Fields
- Pages Requiring Attention
- Migration Considerations

### 4. Flag plugin redundancy

Use patterns from `references/plugin-redundancy-patterns.md` to detect:
- ACF free + ACF PRO both active
- Multiple block libraries (Spectra, Stackable, CoBlocks)
- Multiple SEO plugins
- Multiple caching plugins
- Multiple form plugins

Add warnings in the Plugins section for any duplicates found.

### 5. Detect pages requiring attention

Use heuristics from `references/pages-attention-heuristics.md` to flag:
- Empty post type archives
- High draft counts (e.g., 42 unpublished careers)
- Custom post types with zero content
- Missing homepage/front page assignments
- Pages with unusual shortcodes or iframes

Add these to the "Pages Requiring Attention" section with context.

### 6. Output the report

Save the markdown report to a file:

```bash
# Example filename format
site-inventory-YYYY-MM-DD.md
```

The report is now ready for:
- AI context loading (paste into conversation)
- Client documentation
- Migration planning
- Development kickoff reference

## Verification

- The markdown report should parse correctly and include all required sections
- Tables should be properly formatted (CSV-compatible)
- All command outputs should be captured (no "command not found" errors)
- Plugin redundancy should be flagged if present
- Content statistics should match what's visible in wp-admin

## Failure modes / debugging

**"Database connection error"**
- Check wp-config.php database credentials
- Verify database server is running
- Use `wp db check` to diagnose

**"Could not find theme"**
- Verify theme is activated: `wp theme list`
- Check theme directory exists: `ls -la wp-content/themes/`

**"ACF commands fail"**
- ACF may not be installed — skip gracefully and note in report

**"Block detection incomplete"**
- Some blocks registered in JS may not appear in PHP registry
- Supplement with filesystem grep for `registerBlockType`

**"Large site queries timeout"**
- Increase PHP memory limit in wp-config.php
- Use pagination for very large queries (modify commands as needed)

## Related skills

After inventorying the site:

- **`wp-project-triage`** - Analyze the theme/plugin codebase structure for development work
- **`wp-block-development`** - Modify custom blocks discovered in the inventory
- **`wp-block-themes`** - Plan FSE migration using the content inventory
- **`wp-plugin-development`** - Extend or modify custom post types and features

## Notes

- This skill works on **local environments only** (requires WP-CLI and filesystem access)
- For production sites, replicate locally first (via staging or migration plugin)
- The inventory is a **snapshot in time** — re-run after major changes
- Markdown output is optimized for AI context loading and human readability
