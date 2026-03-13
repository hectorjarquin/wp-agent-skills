# WP-CLI Commands for Site Inventory

This reference lists all WP-CLI commands needed to collect inventory data for each layer.

---

## Layer 1: Site Metadata

### WordPress Version
```bash
wp core version
```

### WordPress Version (with database)
```bash
wp core version --extra
```

### PHP Version
```bash
php -v | head -n 1
```

### Active Theme
```bash
wp theme list --status=active --format=csv
```

### Child Theme Check
```bash
wp theme list --status=active --fields=name,template --format=csv
```
If `name` ≠ `template`, it's a child theme.

### Database Prefix
```bash
wp db prefix
```

### Database Size
```bash
wp db size
```

### Site URL and Home
```bash
wp option get home
wp option get siteurl
```

### Multisite Status
```bash
wp core is-installed --network
```
Exit code 0 = multisite, 1 = single site

---

## Layer 2: Content Inventory

### All Post Types with Counts (Published, Drafts, Trash)
```bash
wp db query "
SELECT 
  post_type,
  post_status,
  COUNT(*) as count
FROM $(wp db prefix)posts
GROUP BY post_type, post_status
ORDER BY post_type, post_status
" --skip-column-names
```

### Total Content Count (Excluding Revisions)
```bash
wp db query "
SELECT COUNT(*) 
FROM $(wp db prefix)posts 
WHERE post_type NOT IN ('revision', 'nav_menu_item')
" --skip-column-names
```

### Attachment/Media Count
```bash
wp db query "
SELECT COUNT(*) 
FROM $(wp db prefix)posts 
WHERE post_type = 'attachment'
" --skip-column-names
```

### List All Unique Post Types
```bash
wp db query "
SELECT DISTINCT post_type 
FROM $(wp db prefix)posts 
ORDER BY post_type
" --skip-column-names
```

---

## Layer 3: Plugins

### Active Plugins
```bash
wp plugin list --status=active --format=csv --fields=name,version,title
```

### Inactive Plugins
```bash
wp plugin list --status=inactive --format=csv --fields=name,version,title
```

### Must-Use Plugins
```bash
wp plugin list --status=must-use --format=csv --fields=name,version,title
```

### Drop-in Plugins
```bash
wp plugin list --status=dropin --format=csv --fields=name,version,title
```

### Plugin Count Summary
```bash
wp plugin list --format=count
```

---

## Layer 4: Custom Post Types & Taxonomies

### List All Registered Post Types
```bash
wp post-type list --format=csv --fields=name,label,public,hierarchical,rest_base
```

### Custom Post Types Only (Exclude Built-in)
```bash
wp post-type list --format=csv --fields=name,label,public,hierarchical,rest_base | grep -v -E '^(post|page|attachment|revision|nav_menu_item|custom_css|customize_changeset|oembed_cache|user_request|wp_block|wp_template|wp_template_part|wp_global_styles|wp_navigation)'
```

### List All Registered Taxonomies
```bash
wp taxonomy list --format=csv --fields=name,label,public,hierarchical,rest_base
```

### Custom Taxonomies Only (Exclude Built-in)
```bash
wp taxonomy list --format=csv --fields=name,label,public,hierarchical,rest_base | grep -v -E '^(category|post_tag|nav_menu|link_category|post_format)'
```

---

## Layer 5: Advanced Custom Fields (ACF)

### Check if ACF is Installed
```bash
wp plugin list --name=advanced-custom-fields --format=count
wp plugin list --name=advanced-custom-fields-pro --format=count
```

### Get ACF Field Groups (If ACF PRO with CLI support)
```bash
wp acf field-group list --format=csv 2>/dev/null || echo "ACF CLI not available"
```

### Query ACF Field Groups from Database
```bash
wp db query "
SELECT 
  p.ID,
  p.post_title as field_group_name,
  p.post_name as field_group_key
FROM $(wp db prefix)posts p
WHERE p.post_type = 'acf-field-group'
  AND p.post_status = 'publish'
ORDER BY p.post_title
" --skip-column-names
```

### Count ACF Fields per Group
```bash
wp db query "
SELECT 
  p.post_parent as field_group_id,
  COUNT(*) as field_count
FROM $(wp db prefix)posts p
WHERE p.post_type = 'acf-field'
  AND p.post_status = 'publish'
GROUP BY p.post_parent
" --skip-column-names
```

---

## Layer 6: Custom Blocks

### List All Registered Block Types
```bash
wp block list --format=csv --fields=name,title,category 2>/dev/null || echo "Block list command not available (WP < 6.1)"
```

### Alternative: Query Block Registry from Options Table
```bash
wp option get unregistered_blocks --format=json 2>/dev/null || echo "[]"
```

### Search for Block Registration in Theme Files
```bash
# Find block.json files in active theme
THEME_PATH=$(wp theme path $(wp theme list --status=active --field=name))
find "$THEME_PATH" -name "block.json" -type f
```

```bash
# Find PHP block registration calls
grep -r "register_block_type" "$THEME_PATH" --include="*.php" -n
```

### Search for Shortcodes
```bash
# Find shortcode registration calls in theme
grep -r "add_shortcode" "$THEME_PATH" --include="*.php" -n
```

### List Shortcodes from Database (Global Shortcodes Option)
```bash
wp option get shortcodes --format=json 2>/dev/null || echo "[]"
```

---

## Layer 7: Theme Architecture

### Active Theme Path
```bash
wp theme path $(wp theme list --status=active --field=name)
```

### Theme Directory Structure
```bash
THEME_PATH=$(wp theme path $(wp theme list --status=active --field=name))
tree -L 3 "$THEME_PATH" 2>/dev/null || find "$THEME_PATH" -maxdepth 3 -type d
```

### Theme File Count
```bash
THEME_PATH=$(wp theme path $(wp theme list --status=active --field=name))
find "$THEME_PATH" -type f | wc -l
```

### Theme Directory Size
```bash
THEME_PATH=$(wp theme path $(wp theme list --status=active --field=name))
du -sh "$THEME_PATH"
```

### Check for theme.json (Block Theme Indicator)
```bash
THEME_PATH=$(wp theme path $(wp theme list --status=active --field=name))
test -f "$THEME_PATH/theme.json" && echo "Block Theme" || echo "Classic Theme"
```

### List Custom Fonts
```bash
THEME_PATH=$(wp theme path $(wp theme list --status=active --field=name))
find "$THEME_PATH" -type f \( -name "*.woff" -o -name "*.woff2" -o -name "*.ttf" -o -name "*.otf" \)
```

### List JavaScript Files
```bash
THEME_PATH=$(wp theme path $(wp theme list --status=active --field=name))
find "$THEME_PATH" -name "*.js" -type f
```

### List CSS Files
```bash
THEME_PATH=$(wp theme path $(wp theme list --status=active --field=name))
find "$THEME_PATH" -name "*.css" -type f
```

### Check for Common Theme Features in functions.php
```bash
THEME_PATH=$(wp theme path $(wp theme list --status=active --field=name))
grep -E "add_theme_support|register_nav_menus|add_image_size|register_sidebar" "$THEME_PATH/functions.php" | head -n 20
```

---

## Layer 8: Global Features

### Navigation Menus
```bash
wp menu list --format=csv --fields=term_id,name,slug,count
```

### Menu Locations (Registered by Theme)
```bash
wp menu location list --format=csv
```

### Widgets
```bash
wp widget list sidebar-1 --format=csv 2>/dev/null || echo "No widgets in sidebar-1"
```

### Sidebars (Registered)
```bash
wp sidebar list --format=csv 2>/dev/null || echo "Sidebar list not available"
```

### Search for AJAX Endpoints in Theme
```bash
THEME_PATH=$(wp theme path $(wp theme list --status=active --field=name))
grep -r "wp_ajax" "$THEME_PATH" --include="*.php" -n | head -n 10
```

### Check for Contact Form 7 Forms
```bash
wp db query "
SELECT ID, post_title 
FROM $(wp db prefix)posts 
WHERE post_type = 'wpcf7_contact_form' 
  AND post_status = 'publish'
" --skip-column-names
```

---

## Layer 9: Assets & Dependencies

### Enqueued Scripts (Frontend)
```bash
wp eval 'wp_head(); global $wp_scripts; foreach($wp_scripts->queue as $handle) { $script = $wp_scripts->registered[$handle]; echo $handle . "," . $script->src . "\n"; }'
```

### Enqueued Styles (Frontend)
```bash
wp eval 'wp_head(); global $wp_styles; foreach($wp_styles->queue as $handle) { $style = $wp_styles->registered[$handle]; echo $handle . "," . $style->src . "\n"; }'
```

### Check for CDN Dependencies in Theme Files
```bash
THEME_PATH=$(wp theme path $(wp theme list --status=active --field=name))
grep -rE "https?://(cdn\.|jsdelivr|unpkg|cdnjs|maxcdn|bootstrapcdn)" "$THEME_PATH" --include="*.php" --include="*.js" -n | head -n 20
```

### Check for jQuery Usage
```bash
THEME_PATH=$(wp theme path $(wp theme list --status=active --field=name))
grep -r "jquery" "$THEME_PATH" --include="*.js" --include="*.php" -i | wc -l
```

---

## Layer 10: Pages Requiring Attention

### Find Empty Archives (Post Types with Content but No Archive Page)
```bash
# List post types with published content
wp db query "
SELECT DISTINCT post_type 
FROM $(wp db prefix)posts 
WHERE post_status = 'publish' 
  AND post_type NOT IN ('revision', 'nav_menu_item', 'attachment')
" --skip-column-names
```

### Check for Missing Homepage Assignment
```bash
wp option get show_on_front
wp option get page_on_front
```
If `show_on_front` = "page" but `page_on_front` = 0, homepage is not assigned.

### Find Post Types with High Draft Counts
```bash
wp db query "
SELECT 
  post_type,
  COUNT(*) as draft_count
FROM $(wp db prefix)posts
WHERE post_status = 'draft'
GROUP BY post_type
HAVING draft_count > 10
ORDER BY draft_count DESC
" --skip-column-names
```

### Find Empty Custom Post Types (Registered but No Content)
```bash
# Compare registered CPTs with post counts
# (Requires combining wp post-type list with content inventory queries)
```

### Search for iframes (Potential External Integrations)
```bash
wp db query "
SELECT 
  ID,
  post_title,
  post_type
FROM $(wp db prefix)posts
WHERE post_content LIKE '%<iframe%'
  AND post_status = 'publish'
LIMIT 20
" --skip-column-names
```

### Search for Popup Shortcodes or Classes
```bash
wp db query "
SELECT 
  ID,
  post_title,
  post_type
FROM $(wp db prefix)posts
WHERE (post_content LIKE '%[popup%' OR post_content LIKE '%class=\"popup%')
  AND post_status = 'publish'
LIMIT 20
" --skip-column-names
```

---

## Aggregating Results

Run all commands sequentially, capturing output to variables or temp files. Use the data to populate the markdown template in `inventory-template.md`.

**Example workflow:**

```bash
# Capture metadata
WP_VERSION=$(wp core version)
PHP_VERSION=$(php -v | head -n 1)
THEME_NAME=$(wp theme list --status=active --field=name)

# Capture content stats (to temp file)
wp db query "..." > /tmp/content-inventory.txt

# Build markdown report
cat > site-inventory-$(date +%Y-%m-%d).md <<EOF
# WordPress Site Inventory

**Date:** $(date +"%B %d, %Y")
**WordPress Version:** $WP_VERSION
**PHP Version:** $PHP_VERSION
**Active Theme:** $THEME_NAME

## Content Inventory

...
EOF
```
