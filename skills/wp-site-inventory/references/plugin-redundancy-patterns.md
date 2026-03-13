# Plugin Redundancy Detection Patterns

This reference lists common plugin redundancies to flag during site inventory.

## Detection Logic

For each pattern below, check if multiple plugins from the same category are active simultaneously. Flag as a warning in the inventory report.

---

## ACF (Advanced Custom Fields)

### Pattern
Both ACF Free and ACF Pro are active

### Detection
```
active: advanced-custom-fields
active: advanced-custom-fields-pro
```

### Warning Message
> ⚠️ **ACF Redundancy**: Both ACF Free and ACF Pro are active. ACF Pro includes all features of the free version. Deactivate and remove the free version.

### Recommendation
- Keep: `advanced-custom-fields-pro`
- Remove: `advanced-custom-fields`

---

## Block Libraries

### Pattern
Multiple block library plugins active (Spectra, Stackable, CoBlocks, EditorsKit, Block Slider)

### Detection
```
Multiple active from:
- spectra (formerly ultimate-addons-for-gutenberg)
- stackable-ultimate-gutenberg-blocks
- coblocks
- editorskit
- block-slider
```

### Warning Message
> ⚠️ **Block Library Redundancy**: [count] block library plugins detected ([names]). Using multiple block libraries can cause:
> - Performance overhead (loading many unused blocks)
> - Style conflicts between libraries
> - Maintenance complexity
>
> **Recommendation:** Audit which blocks from each library are actually used, then consolidate to 1-2 libraries maximum.

### Recommendation
- Inventory which blocks are actually in use on pages
- Keep only the library(ies) with blocks actively used
- Rebuild pages using the remaining library if needed

---

## SEO Plugins

### Pattern
Multiple SEO plugins active

### Detection
```
Multiple active from:
- yoast-seo
- wordpress-seo (Yoast SEO)
- all-in-one-seo-pack
- seo-press
- rank-math
- slim-seo
```

### Warning Message
> ⚠️ **SEO Plugin Redundancy**: Multiple SEO plugins active ([names]). This can cause:
> - Conflicting meta tags
> - Duplicate sitemaps
> - Performance issues
> - Confusing admin interfaces
>
> **Recommendation:** Choose one SEO plugin and deactivate the others.

### Recommendation
- Audit which plugin's meta data is currently serving to search engines
- Migrate settings to chosen plugin if needed
- Remove redundant plugins

---

## Caching Plugins

### Pattern
Multiple caching plugins active

### Detection
```
Multiple active from:
- wp-rocket
- w3-total-cache
- wp-super-cache
- wp-fastest-cache
- autoptimize (with other caching)
- litespeed-cache
- wp-optimize
- object-cache-pro (with page caching)
```

### Warning Message
> ⚠️ **Caching Plugin Redundancy**: Multiple caching plugins active ([names]). This can cause:
> - Cache conflicts
> - Stale content
> - Performance degradation
> - Debugging difficulties
>
> **Recommendation:** Use one caching solution. Object Cache Pro pairs well with a page cache, but avoid multiple page caching plugins.

### Recommendation
- Keep one page caching solution
- Object caching (like Object Cache Pro) can coexist with page caching
- Test thoroughly after removing redundant plugins

---

## Form Plugins

### Pattern
Multiple form builder plugins active

### Detection
```
Multiple active from:
- contact-form-7
- gravityforms
- wpforms (free or premium)
- ninja-forms
- formidable
- caldera-forms
```

### Warning Message
> ⚠️ **Form Plugin Redundancy**: Multiple form plugins active ([names]). 
>
> **Note:** This may be intentional if different forms serve different purposes (e.g., CF7 for simple contact, Gravity Forms for complex workflows). However, consolidation may simplify maintenance.

### Recommendation
- If forms can be rebuilt in one plugin, consolidate
- If forms serve distinctly different purposes, document why both are needed
- Deactivate unused form plugins

---

## Security Plugins

### Pattern
Multiple security plugins active

### Detection
```
Multiple active from:
- wordfence
- sucuri-scanner
- ithemes-security
- all-in-one-wp-security-and-firewall
- jetpack (with security features enabled)
```

### Warning Message
> ⚠️ **Security Plugin Redundancy**: Multiple security plugins active ([names]). This can cause:
> - Conflicting firewall rules
> - Performance overhead (multiple scans)
> - False positives in threat detection
>
> **Recommendation:** Choose one comprehensive security plugin.

### Recommendation
- Compare feature sets
- Migrate settings to chosen plugin
- Test firewall rules after consolidation

---

## Backup Plugins

### Pattern
Multiple backup plugins active

### Detection
```
Multiple active from:
- updraftplus
- backupbuddy
- all-in-one-wp-migration
- duplicator
- backwpup
- jetpack (with backup enabled)
```

### Warning Message
> ⚠️ **Backup Plugin Redundancy**: Multiple backup plugins active ([names]).
>
> **Note:** Some overlap may be intentional (e.g., one for automated backups, one for migration). Verify both are needed.

### Recommendation
- Keep one for automated scheduled backups
- Keep one migration tool if needed for site transfers
- Deactivate plugins not actively used

---

## Image Optimization Plugins

### Pattern
Multiple image optimization plugins active

### Detection
```
Multiple active from:
- shortpixel-image-optimiser
- smush
- imagify
- ewww-image-optimizer
- optimole
```

### Warning Message
> ⚠️ **Image Optimization Redundancy**: Multiple image optimization plugins active ([names]). This can cause:
> - Double optimization (quality degradation)
> - Conflicting resizing rules
> - Wasted API credits
>
> **Recommendation:** Use one image optimization plugin.

### Recommendation
- Choose one based on quality/performance preference
- Deactivate others to avoid re-optimization

---

## Analytics Plugins

### Pattern
Multiple analytics plugins active

### Detection
```
Multiple active from:
- google-analytics-for-wordpress (MonsterInsights)
- google-analytics-dashboard-for-wp
- ga-google-analytics
- jetpack (with analytics enabled)
- exact-metrics
```

### Warning Message
> ⚠️ **Analytics Plugin Redundancy**: Multiple analytics plugins active ([names]). This can cause:
> - Duplicate tracking (inflated metrics)
> - Performance overhead (multiple scripts loading)
>
> **Recommendation:** Use one analytics plugin or direct Google Analytics integration.

### Recommendation
- Keep one plugin or implement GA directly in theme
- Verify tracking codes aren't duplicated

---

## Migration/Import Plugins

### Pattern
Multiple migration plugins active (often left active after migration)

### Detection
```
Multiple active from:
- all-in-one-wp-migration
- duplicator
- wp-migrate-db
- wordpress-importer
```

### Warning Message
> ⚠️ **Migration Plugin Warning**: [count] migration plugins active ([names]). These are typically used once for site migration/import.
>
> **Recommendation:** If migration is complete, deactivate these plugins. They can be security risks if left active.

### Recommendation
- Deactivate after migration is complete
- Delete if no future migrations planned
- Keep inactive if regular staging/production sync needed

---

## WordPress Importer

### Pattern
WordPress Importer plugin active long-term

### Detection
```
active: wordpress-importer
```

### Warning Message
> ⚠️ **WordPress Importer Active**: This plugin is typically used once for content import. If import is complete, it can be deactivated.

### Recommendation
- Deactivate if import is complete
- Delete if not needed for future imports

---

## Duplicate Post/Content Plugins

### Pattern
Multiple duplicate content plugins active

### Detection
```
Multiple active from:
- duplicate-post
- yoast-duplicate-post
- post-duplicator
```

### Warning Message
> ⚠️ **Duplicate Content Plugin Redundancy**: Multiple plugins for duplicating posts/pages detected ([names]).

### Recommendation
- Keep one if feature is actively used
- Deactivate/remove if duplication is rarely needed

---

## Conditional Logic Plugins (for Contact Form 7)

### Pattern
Multiple CF7 extension plugins providing similar functionality

### Detection
```
Multiple active from:
- conditional-fields-for-contact-form-7
- cf7-conditional-fields
- contact-form-7-dynamic-text-extension
```

### Warning Message
> ⚠️ **CF7 Extension Overlap**: Multiple Contact Form 7 extension plugins detected. Review to ensure they serve distinct purposes.

### Recommendation
- Audit which features from each plugin are actually used
- Consolidate if features overlap

---

## Detection Algorithm

When running inventory:

1. Get list of all active plugins
2. For each redundancy pattern above:
   - Check if multiple plugins from that category are active
   - If yes, add warning to the "Plugin Redundancy Warnings" section
   - Include specific recommendation
3. Sort warnings by priority:
   - High: Caching, SEO (can break site)
   - Medium: Block libraries, forms, security
   - Low: Analytics, duplicators, migration tools

## Output Format

```markdown
### Plugin Redundancy Warnings

⚠️ **ACF Redundancy**: Both ACF Free (v6.7.0) and ACF Pro (v5.8.4) are active.
- **Issue:** ACF Pro includes all features of the free version
- **Recommendation:** Deactivate and remove `advanced-custom-fields` (free version)

⚠️ **Block Library Redundancy**: 3 block library plugins detected (Spectra v2.19.19, Stackable v3.19.6, CoBlocks v3.1.16)
- **Issue:** Performance overhead, potential style conflicts
- **Recommendation:** Audit which blocks are actively used, consolidate to 1-2 libraries maximum
```
