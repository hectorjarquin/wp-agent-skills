# WordPress Site Inventory

**Date:** [CURRENT_DATE]  
**Site URL:** [SITE_URL]  
**WordPress Version:** [WP_VERSION]  
**PHP Version:** [PHP_VERSION]  
**Database Prefix:** [DB_PREFIX]  
**Active Theme:** [THEME_NAME]  
**Theme Version:** [THEME_VERSION]  
**Theme Type:** [Block Theme / Classic Theme]  
**Multisite:** [Yes / No]

---

## Table of Contents
1. [Content Inventory](#content-inventory)
2. [Advanced Custom Fields](#advanced-custom-fields)
3. [Custom Blocks](#custom-blocks)
4. [Global Features](#global-features)
5. [Pages Requiring Attention](#pages-requiring-attention)
6. [Plugins](#plugins)
7. [Theme Architecture](#theme-architecture)
8. [Custom Functionality](#custom-functionality)
9. [Assets & Dependencies](#assets--dependencies)
10. [Migration Considerations](#migration-considerations)

---

## Content Inventory

### Post Type Statistics

| Post Type | All | Published | Drafts | Trash | Notes |
|-----------|-----|-----------|--------|-------|-------|
| **[post_type]** | [count] | [published] | [drafts] | [trash] | [description] |
| **[post_type]** | [count] | [published] | [drafts] | [trash] | [description] |

**Total Content:** [total_count] items (excluding attachments)  
**Media Files:** [attachment_count] attachments

### Content Notes

- ⚠️ Flag any post types with unusual draft counts (e.g., 42 unpublished careers)
- ⚠️ Flag empty post types (registered but no content)
- ℹ️ Note standard vs custom post types

---

## Advanced Custom Fields

### ACF Status

[✅ ACF Installed / ❌ ACF Not Present]

**Plugin:** [ACF Free / ACF PRO] (v[version])

### ACF Field Groups

| Field Group | Post Type | Count | Purpose |
|-------------|-----------|-------|---------|
| [field_group_name] | [cpt] | [field_count] | [description/purpose] |

**Total Field Groups:** [count]

---

## Custom Blocks

### Registered Blocks (Theme-based)

1. **[Block Name]** (`[block_namespace/block_name]`)
   - Description: [purpose]
   - File: [path/to/block.json or registration file]
   - Type: [Static / Dynamic / Interactive]

2. **[Block Name]** (`[block_namespace/block_name]`)
   - Description: [purpose]
   - File: [path/to/file]

### Available Shortcodes

1. `[shortcode_name]` - [description]
2. `[shortcode_name]` - [description]

**Total Custom Blocks:** [count]  
**Total Shortcodes:** [count]

---

## Global Features

### Site-wide Components

- **Navigation Menus**
  - [Menu Name] ([location]) - [item_count] items
  - [Menu Name] ([location]) - [item_count] items

- **Search Functionality**
  - [✅ Custom search / ❌ Default WordPress search]
  - Searches: [post_types_searched]
  - Implementation: [AJAX / Standard / Plugin-based]

- **Contact Forms**
  - Plugin: [Contact Form 7 / Gravity Forms / etc.]
  - Form count: [count]
  - Notable forms: [list key forms]

- **Social Media Integration**
  - [Plugin name or custom implementation]
  - Platforms: [Facebook, Instagram, Twitter, etc.]

- **Other Integrations**
  - [Language translation / Analytics / Chat / etc.]
  - Implementation details

---

## Pages Requiring Attention

### Flagged Items

**1. [Page/Feature Name]**
- **URL:** [url]
- **Issue:** [description of what needs review]
- **Priority:** [High / Medium / Low]

**2. [Page/Feature Name]**
- **URL:** [url]
- **Issue:** [description]
- **Priority:** [High / Medium / Low]

### Automated Detection Flags

- ⚠️ [Issue type]: [details]
- ⚠️ [Issue type]: [details]

---

## Plugins

### Active Plugins ([count])

| Plugin Name | Version | Purpose |
|-------------|---------|---------|
| [plugin_name] | [version] | [description] |

### Inactive Plugins ([count])

| Plugin Name | Version | Purpose |
|-------------|---------|---------|
| [plugin_name] | [version] | [description] |

### Must-Use Plugins ([count])

| Plugin Name | Version | Purpose |
|-------------|---------|---------|
| [plugin_name] | [version] | [description] |

### Plugin Redundancy Warnings

⚠️ **[Redundancy Type]**: [details]
- [Plugin A] vs [Plugin B]
- **Recommendation:** [keep one, consolidate, etc.]

---

## Theme Architecture

### Directory Structure

```
[theme-name]/
├── assets/
│   ├── css/
│   ├── js/
│   ├── fonts/
│   └── images/
├── inc/
│   ├── blocks/
│   └── shortcodes/
├── template-parts/
├── templates/          (if block theme)
├── parts/              (if block theme)
├── patterns/           (if block theme)
├── theme.json          (if block theme)
├── functions.php
├── index.php
└── style.css
```

**Stats:**
- Total Files: [count]
- Theme Size: [size]
- Theme Type: [Classic / Block / Hybrid]

### Theme Features

**Registered Theme Support:**
- [✅/❌] Block Editor (Gutenberg)
- [✅/❌] Wide/Full Width Alignment
- [✅/❌] Post Thumbnails
- [✅/❌] Custom Logo
- [✅/❌] HTML5 Markup
- [✅/❌] Responsive Embeds
- [list other theme support features]

**Navigation Menus:**
- [location_id]: [description]

**Image Sizes:**
- [size_name]: [dimensions]

---

## Custom Functionality

### Custom Post Types ([count])

All registered in `[file_path]`:

1. **[Post Type Label]** (`[post_type_slug]`)
   - Supports: [title, editor, thumbnail, etc.]
   - REST API: [✅ Enabled / ❌ Disabled]
   - Hierarchical: [Yes / No]
   - URL slug: `/[slug]/`
   - Menu icon: [dashicon]

2. **[Post Type Label]** (`[post_type_slug]`)
   - [details]

### Custom Taxonomies ([count])

1. **[Taxonomy Label]** (`[taxonomy_slug]`)
   - Associated with: [post_types]
   - Hierarchical: [Yes / No]
   - REST API: [✅ Enabled / ❌ Disabled]

### Color Palette ([count] colors)

| Color Name | Slug | Hex Code |
|------------|------|----------|
| [name] | [slug] | [#hex] |

### Typography

**Custom Fonts:**
- [Font Name] - [weights available]

**Font files location:** `[path]`

### AJAX Functionality

**Endpoints Found:**
- `[ajax_action]` - [description]

### Custom Templating

**Templates:**
- [template-name.php] - [purpose]

---

## Assets & Dependencies

### External CDN Dependencies

**CSS:**
- [Library Name] [version] - [cdn_url]

**JavaScript:**
- [Library Name] [version] - [cdn_url]

### Theme Assets

**CSS Files:**
1. `[filename.css]` - [purpose] (v[version])

**JavaScript Files:**
1. `[filename.js]` - [purpose] (v[version])

### Dependency Analysis

**jQuery Usage:** [✅ Used / ❌ Not Used]
- Instances found: [count]
- Files using jQuery: [list]

**Bootstrap:** [✅ Used / ❌ Not Used]
- Version: [version]

**Other Frameworks:**
- [Framework name] [version]

---

## Migration Considerations

### Critical Items for FSE Migration

#### ✅ Must Preserve

1. **Custom Post Types**
   - [count] CPTs need migration
   - Maintain URL structure
   - Preserve REST API support

2. **ACF Field Groups**
   - [count] field groups with extensive data
   - Need block-based alternatives or keep ACF integration

3. **Custom Blocks**
   - [count] theme blocks need conversion to block.json format
   - Must maintain functionality

4. **Shortcodes**
   - [count] shortcodes in use
   - Consider converting to blocks or maintaining backward compatibility

5. **Navigation System**
   - [details about custom navigation]

6. **Search Functionality**
   - [details about custom search]

7. **Contact Form Integration**
   - [details]

8. **Color Palette & Typography**
   - [count] brand colors
   - [font families]
   - Editor integration needed

#### ⚠️ Dependencies to Address

1. **[Framework/Library]**
   - Heavily used throughout
   - [Replacement strategy]

2. **jQuery**
   - [count] files depend on jQuery
   - Modern JS refactoring needed

3. **External CDN Assets**
   - Move to local hosting or modern alternatives
   - Version control concerns

#### 🔍 Items Requiring Investigation

1. **[Feature/Page]**
   - [What needs to be investigated]
   - [Why it's flagged]

#### 📊 Content Migration Strategy

**High Volume:**
- [post_type]: [count] items
- [post_type]: [count] items

**Low Volume:**
- [post_type]: [count] items

### FSE Theme Requirements (If Migrating)

1. **Block Templates**
   - Home template
   - Single post templates ([count] post types)
   - Archive templates
   - Search template
   - 404 template

2. **theme.json Configuration**
   - Color palette ([count] colors)
   - Typography ([fonts])
   - Spacing scale
   - Layout settings

3. **Custom Block Development**
   - Convert [count] theme blocks to proper Gutenberg block.json format
   - Maintain ACF integration in blocks (if keeping ACF)

### Modernization Recommendations

**If Staying Classic Theme:**
- Refactor [dependency] dependencies
- Modernize JavaScript (jQuery → vanilla JS or WP Interactivity API)
- Improve performance ([specific recommendations])
- Code cleanup ([specific issues])

**If Migrating to FSE:**
- Build block templates for [count] post types
- Migrate custom walkers to block patterns
- Port theme support features to theme.json
- Create block versions of shortcodes

---

## Next Steps

Based on this inventory, recommended actions:

1. **Immediate:**
   - [action item]
   - [action item]

2. **Short-term (1-2 sprints):**
   - [action item]
   - [action item]

3. **Long-term (3+ sprints):**
   - [action item]
   - [action item]

4. **Requires Client Decision:**
   - [decision needed]
   - [decision needed]

---

**Document Version:** 1.0  
**Last Updated:** [CURRENT_DATE]  
**Generated by:** wp-site-inventory skill
