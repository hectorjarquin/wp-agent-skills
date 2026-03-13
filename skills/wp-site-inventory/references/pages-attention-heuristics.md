# Pages Requiring Attention - Detection Heuristics

This reference provides automated detection patterns for flagging pages, features, or configurations that may need manual review.

---

## Heuristic 1: Empty Custom Post Type Archives

### Detection
Custom post type has published content but no dedicated archive page or archive template

### Query
```bash
# Get all CPTs with published content
wp db query "
SELECT DISTINCT post_type, COUNT(*) as count
FROM $(wp db prefix)posts
WHERE post_status = 'publish'
  AND post_type NOT IN ('post', 'page', 'attachment', 'revision', 'nav_menu_item')
GROUP BY post_type
HAVING count > 0
" --skip-column-names

# For each CPT, check if has_archive is enabled
wp post-type get [cpt_name] --field=has_archive
```

### Flag If
- CPT has published posts
- `has_archive` = false or not set
- No custom archive page exists in database

### Output
```markdown
⚠️ **Empty Archive**: Custom post type `events` has 3 published posts but no archive page configured.
- **URL**: Likely `/events/` (may 404)
- **Issue**: Users cannot browse events in list view
- **Priority**: Medium
- **Recommendation**: Enable `has_archive` in CPT registration or create an archive template
```

---

## Heuristic 2: High Draft Content (Unpublished Content)

### Detection
Custom post type has significantly more drafts than published posts

### Query
```bash
wp db query "
SELECT 
  post_type,
  SUM(CASE WHEN post_status = 'publish' THEN 1 ELSE 0 END) as published,
  SUM(CASE WHEN post_status = 'draft' THEN 1 ELSE 0 END) as drafts
FROM $(wp db prefix)posts
WHERE post_type NOT IN ('revision', 'nav_menu_item', 'attachment')
GROUP BY post_type
HAVING drafts > 10 OR (drafts > published AND published > 0)
" --skip-column-names
```

### Flag If
- Drafts > 10, OR
- Drafts > Published AND Published > 0

### Output
```markdown
⚠️ **High Draft Count**: `careers` has 42 drafts vs only 2 published posts.
- **Issue**: Large amount of unpublished content
- **Priority**: High
- **Questions**: 
  - Are these active job postings that should be live?
  - Are these old drafts that need cleanup?
  - Is there a content approval bottleneck?
- **Recommendation**: Review with client — publish, archive, or delete
```

---

## Heuristic 3: Empty Custom Post Types (Registered but No Content)

### Detection
Custom post type is registered but has zero posts of any status

### Query
```bash
# Get all registered CPTs
wp post-type list --field=name | grep -v -E '^(post|page|attachment|revision|nav_menu_item|wp_block|wp_template|wp_template_part)'

# For each CPT, count posts
wp post list --post_type=[cpt_name] --format=count
```

### Flag If
- CPT is registered
- Post count = 0

### Output
```markdown
ℹ️ **Empty Custom Post Type**: `volunteer` is registered but has no content.
- **Issue**: Unused post type may be legacy or planned for future
- **Priority**: Low
- **Recommendation**: 
  - If not used, consider unregistering to reduce admin clutter
  - If planned for future, document intended use
```

---

## Heuristic 4: Missing Homepage Assignment

### Detection
WordPress is set to display a static page as the homepage, but no page is assigned

### Query
```bash
SHOW_ON_FRONT=$(wp option get show_on_front)
PAGE_ON_FRONT=$(wp option get page_on_front)

if [ "$SHOW_ON_FRONT" = "page" ] && [ "$PAGE_ON_FRONT" = "0" ]; then
  echo "Homepage not assigned"
fi
```

### Flag If
- `show_on_front` = "page"
- `page_on_front` = 0 or empty

### Output
```markdown
⚠️ **Missing Homepage**: WordPress is set to use a static page as homepage, but no page is assigned.
- **URL**: Site root likely shows blog posts instead
- **Issue**: Users may see unintended content on homepage
- **Priority**: High
- **Recommendation**: Go to Settings → Reading and assign a homepage
```

---

## Heuristic 5: Pages with iframes (External Integrations)

### Detection
Published pages containing `<iframe>` tags (donation forms, virtual tours, embedded content)

### Query
```bash
wp db query "
SELECT 
  ID,
  post_title,
  post_type,
  post_name
FROM $(wp db prefix)posts
WHERE post_content LIKE '%<iframe%'
  AND post_status = 'publish'
  AND post_type IN ('page', 'post')
" --skip-column-names
```

### Flag If
- Content contains `<iframe>`

### Output
```markdown
ℹ️ **iframe Integration**: Page "[page_title]" contains an iframe integration.
- **URL**: [permalink]
- **Issue**: External dependency (donation form, virtual tour, etc.)
- **Priority**: Medium
- **Recommendation**: 
  - Document what the iframe is for (CanadaHelps donation, virtual tour, etc.)
  - Test that iframe still loads correctly
  - Consider if iframe should be converted to native WordPress solution
  - Verify HTTPS (mixed content issues)
```

---

## Heuristic 6: Pages with Popup Shortcodes or Classes

### Detection
Pages containing popup-related shortcodes or CSS classes

### Query
```bash
wp db query "
SELECT 
  ID,
  post_title,
  post_type,
  post_name
FROM $(wp db prefix)posts
WHERE (
    post_content LIKE '%[popup%'
    OR post_content LIKE '%class=\"popup%'
    OR post_content LIKE '%data-popup%'
  )
  AND post_status = 'publish'
" --skip-column-names
```

### Flag If
- Content contains popup-related code

### Output
```markdown
ℹ️ **Popup Functionality**: Pages with popup implementations detected.
- **Pages**: [list of pages]
- **Issue**: Popup implementation may need verification
- **Priority**: Medium
- **Recommendation**:
  - Test popups still work correctly
  - Verify popup content is up-to-date
  - Review UX — are popups necessary or could content be inline?
  - Check accessibility (keyboard navigation, screen readers)
```

---

## Heuristic 7: Hardcoded Links in Content (Media, Resources)

### Detection
Pages with high counts of manual `<a>` tags that might be hardcoded links instead of dynamic content

### Query
```bash
wp db query "
SELECT 
  ID,
  post_title,
  post_type,
  (LENGTH(post_content) - LENGTH(REPLACE(post_content, '<a href', ''))) / LENGTH('<a href') as link_count
FROM $(wp db prefix)posts
WHERE post_status = 'publish'
  AND post_type = 'page'
HAVING link_count > 10
ORDER BY link_count DESC
" --skip-column-names
```

### Flag If
- Page has > 10 manual links in content

### Output
```markdown
⚠️ **High Link Count (Potential Hardcoding)**: Page "[page_title]" has [count] manual links in content.
- **URL**: [permalink]
- **Issue**: May be hardcoded links instead of dynamic content blocks
- **Priority**: Medium
- **Recommendation**: 
  - Review if links should be managed via custom post type + dynamic block
  - Consider converting to a queryable block pattern
  - Verify all links are still valid (link checker)
```

---

## Heuristic 8: Carousels or Sliders in Content

### Detection
Pages with carousel/slider shortcodes or classes

### Query
```bash
wp db query "
SELECT 
  ID,
  post_title,
  post_type
FROM $(wp db prefix)posts
WHERE (
    post_content LIKE '%carousel%'
    OR post_content LIKE '%slider%'
    OR post_content LIKE '%slick%'
  )
  AND post_status = 'publish'
" --skip-column-names
```

### Flag If
- Content contains carousel/slider references

### Output
```markdown
ℹ️ **Carousel Implementation**: Page "[page_title]" uses a carousel/slider.
- **URL**: [permalink]
- **Issue**: Carousel may depend on external library (Slick, etc.)
- **Priority**: Low
- **Recommendation**:
  - Test carousel functionality
  - Verify carousel is accessible (keyboard nav, ARIA labels)
  - Consider if carousel could be replaced with native block editor solution
```

---

## Heuristic 9: Virtual Tour or 360° Content

### Detection
Pages with virtual tour-related content or shortcodes

### Query
```bash
# Check for virtual tour custom post type content
wp post list --post_type=virtual-tour --format=count

# Check for virtual tour shortcodes
wp db query "
SELECT 
  ID,
  post_title
FROM $(wp db prefix)posts
WHERE post_content LIKE '%virtual%tour%'
  AND post_status = 'publish'
" --skip-column-names
```

### Flag If
- Virtual tour CPT exists with content, OR
- Pages reference virtual tour content

### Output
```markdown
⚠️ **Virtual Tour Implementation**: [count] virtual tour(s) detected.
- **URLs**: [list virtual tour pages]
- **Issue**: Virtual tours often require special viewer libraries or external services
- **Priority**: High (critical feature)
- **Recommendation**:
  - Test virtual tour loads and functions correctly
  - Document what service/library is used (Matterport, custom, etc.)
  - Verify media files are properly hosted
  - Check performance (360° images can be large)
```

---

## Heuristic 10: Unusual Shortcodes

### Detection
Shortcodes that don't match standard WordPress or known plugin shortcodes

### Query
```bash
# Extract all shortcodes from content
wp db query "
SELECT DISTINCT 
  SUBSTRING_INDEX(SUBSTRING_INDEX(post_content, '[', -1), ' ', 1) as shortcode
FROM $(wp db prefix)posts
WHERE post_content LIKE '%[%'
  AND post_status = 'publish'
" --skip-column-names | grep -v '^]'
```

### Flag If
- Shortcode doesn't match known WordPress/plugin patterns
- Shortcode starts with custom theme prefix (like `emb_`)

### Output
```markdown
ℹ️ **Custom Shortcodes Detected**: [count] custom theme shortcodes in use.
- **Shortcodes**: `[emb_display_blog]`, `[emb_share]`, etc.
- **Issue**: Custom shortcodes may need migration to blocks for FSE compatibility
- **Priority**: Medium
- **Recommendation**: 
  - Document what each shortcode does
  - Consider converting to native blocks
  - If keeping theme, ensure shortcodes are well-documented
```

---

## Heuristic 11: Contact Forms with Dynamic Dropdowns

### Detection
Contact Form 7 forms with dynamic dropdown functionality

### Query
```bash
# Check for CF7 dynamic dropdown plugins
wp plugin list --name=cf7 --status=active --format=count

# Check functions.php for dynamic dropdown filters
THEME_PATH=$(wp theme path $(wp theme list --status=active --field=name))
grep -n "cf7.*dropdown" "$THEME_PATH/functions.php"
```

### Flag If
- CF7 is active
- Theme has custom CF7 dropdown filters

### Output
```markdown
ℹ️ **CF7 Dynamic Dropdowns**: Custom Contact Form 7 integration with dynamic dropdowns detected.
- **Issue**: Forms may auto-populate options (e.g., job listings)
- **Priority**: Medium
- **Recommendation**:
  - Test forms to ensure dropdowns populate correctly
  - Document what data source feeds the dropdowns
  - Verify form still works if source CPT is empty
```

---

## Heuristic 12: "Exit This Site Now" Safety Button

### Detection
Theme implements a quick-exit safety button (common for domestic violence support sites)

### Query
```bash
THEME_PATH=$(wp theme path $(wp theme list --status=active --field=name))
grep -rn "exit.*site" "$THEME_PATH" --include="*.php" --include="*.js" -i
```

### Flag If
- Code references "exit site" functionality

### Output
```markdown
⚠️ **Safety Feature Detected**: "Exit This Site" button implementation found.
- **Issue**: Critical safety feature for vulnerable users
- **Priority**: High
- **Recommendation**:
  - Test button functionality thoroughly (should redirect to safe site immediately)
  - Verify button is visible on all pages
  - Review implementation quality (consider best practices from JFCY, etc.)
  - Test keyboard accessibility
  - Consider if button should clear browser history
```

---

## Heuristic 13: Chatbot Integration

### Detection
Custom chatbot implementation with Q&A custom post types

### Query
```bash
# Check for chatbot-related custom post types
wp post list --post_type=chatbot-questions --format=count
wp post list --post_type=chatbot-answers --format=count

# Check for chatbot plugins
wp plugin list --name=chatbot --format=count
```

### Flag If
- Chatbot CPTs exist with content, OR
- Chatbot plugins active

### Output
```markdown
ℹ️ **Chatbot Integration**: Custom chatbot system detected ([count] Q&A pairs).
- **Issue**: Custom chatbot implementation needs documentation and testing
- **Priority**: Medium
- **Recommendation**:
  - Test chatbot functionality on frontend
  - Document chatbot triggers and logic
  - Review Q&A content for accuracy
  - Consider if chatbot should be replaced with modern AI solution
```

---

## Priority Levels

Flags are prioritized as:

- **High**: Issues that affect site functionality or user experience
  - Missing homepage
  - Empty archives with content
  - High draft counts (possible publishing bottleneck)
  - Critical features (virtual tours, safety buttons)

- **Medium**: Issues that need review but don't break the site
  - iframe integrations
  - Popup implementations
  - Hardcoded content
  - Custom shortcodes

- **Low**: Informational flags for documentation
  - Empty CPTs (no content)
  - Carousels
  - Analytics integrations

## Output Format

All flagged items should appear in the **Pages Requiring Attention** section of the inventory report:

```markdown
## Pages Requiring Attention

### Critical (High Priority)

**1. Missing Homepage Assignment**
- **Issue:** WordPress set to show static page as homepage, but no page assigned
- **Priority:** High
- **Action:** Settings → Reading → assign homepage

**2. High Draft Count: Careers**
- **Issue:** 42 unpublished career posts vs 2 published
- **Priority:** High
- **Action:** Review with client — publish, archive, or delete?

### Requires Review (Medium Priority)

**3. iframe Integration: Donate Page**
- **URL:** https://example.com/donate/
- **Issue:** CanadaHelps iframe. Verify still works, check HTTPS.
- **Priority:** Medium

### Informational (Low Priority)

**4. Empty Custom Post Type: Volunteer**
- **Issue:** CPT registered but no content
- **Priority:** Low
- **Action:** Document if planned for future, or unregister
```
