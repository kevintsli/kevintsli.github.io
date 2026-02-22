# Codebase Concerns

**Analysis Date:** 2026-02-22

## Tech Debt

**Outdated Script Dependencies:**
- Issue: `js/script.js` uses legacy IE6 and webkit detection code from circa 2010s, implementing custom `document.ready()` polyfill
- Files: `/js/script.js` (lines 1-29)
- Impact: Unnecessary complexity for browser support that no longer matters; IE detection logic (line 3) is dead code; adds ~30 lines of unmaintained legacy code
- Fix approach: Remove IE6/webkit detection shim entirely and rely on native DOMContentLoaded. Replace custom document.ready() with modern event listeners or eliminate if not used.

**Duplicate Inline CSS & JS:**
- Issue: Main styling and interactivity embedded directly in HTML (1784 lines in `index.html`, 497 lines in new `index.html`); theme toggle logic implemented twice (once in inline `<script>` tags and once in legacy `js/script.js`)
- Files: `/index.html` (lines 14-235 CSS, lines 471-495 JS), `/index_backup.html` (lines 16-1189 CSS, lines 1702-1781 JS)
- Impact: CSS cannot be cached, harder to maintain, inconsistent theming behavior between old and new implementations
- Fix approach: Extract inline CSS to dedicated `css/index.css`. Consolidate theme toggle into single implementation in `js/index.js`. Remove `index_backup.html` if no longer needed.

**Multiple Theme Implementation Approaches:**
- Issue: Two completely different theme toggle implementations coexist:
  - New `index.html`: Modern approach using data attributes and localStorage (lines 471-495)
  - Legacy `js/script.js`: Older approach targeting non-existent DOM elements like `#switch_default`, `mobile-toggle-theme`, `toggleBtn` (lines 32-88)
- Files: `/index.html`, `/js/script.js`, `/index_backup.html`
- Impact: Old script file tries to access DOM elements that don't exist in current HTML; causes silent failures; confusing maintenance
- Fix approach: Delete legacy `js/script.js` entirely. Use only the modern implementation in `index.html`. Verify the new theme implementation works across all pages.

**Unused CSS Files:**
- Issue: `/css/custom.css` is empty (0 bytes); `/css/variable.css` (39 bytes) contains minimal content while main CSS lives in HTML file
- Files: `/css/custom.css`, `/css/variable.css`
- Impact: Fragmented CSS organization; developers might expect custom styles here; orphaned variable definitions
- Fix approach: Delete unused CSS files. If custom styles needed, add to extracted `css/index.css`. Consolidate all CSS variables into single file.

**Broken CSS File Organization:**
- Issue: CSS split across multiple files (`normalize.css`, `base.css`, `font.css`, `layout.css`, `variable.css`, `media.css`, `style.css`) but never imported or used by any HTML file
- Files: `/css/` directory (all files except inline styles in HTML)
- Impact: Dead code; confusing codebase structure; impossible to know what CSS is active
- Fix approach: Verify if any pages use these CSS files. If not used, delete entire `/css/` directory or consolidate all CSS into single `index.css` file that's properly imported.

## Architectural Issues

**File Structure Dissonance:**
- Issue: Codebase contains two fundamentally different designs: old blog/archive structure (2019 archives, article pages) and new single-page portfolio (`index.html`, `index_backup.html`)
- Files: `/2019/`, `/archives/` directories vs. root `index.html`
- Impact: Confusing navigation; multiple entry points; unclear which version is primary; maintenance burden
- Fix approach: Decide on single canonical design. Either: (1) Fully migrate to new SPA and remove old `/2019/` and `/archives/` completely, or (2) Explicitly keep blog as separate legacy section with clear navigation

**Backup File Left in Production:**
- Issue: `index_backup.html` (1784 lines, 66KB) committed to repository and potentially served
- Files: `/index_backup.html`
- Impact: Risk of serving stale version if server misconfigured; confuses visitors; takes up repository space; unclear rollback strategy
- Fix approach: Remove `index_backup.html` from repository. Use proper git history for rollbacks instead. If testing needed, move to untracked `.gitignore` directory.

**Avatar Asset References:**
- Issue: Current `index.html` references `/image/avatar1.png` (647KB) but old version references `/image/avatar.jpeg` (56KB); `avatar2.jpg` was deleted (commit 6aef69c)
- Files: `/index.html` line 1243, `/image/avatar1.png`
- Impact: Large unoptimized image (647KB PNG when 56KB JPEG was adequate); affects page load time
- Fix approach: Replace `avatar1.png` with optimized version (JPEG ~100KB or WebP). Consider using responsive images with srcset.

## Code Quality Concerns

**Legacy Browser Detection (Dead Code):**
- Issue: `js/script.js` checks for IE and webkit <525 browsers (lines 3-4), conditions that never execute in modern browsers
- Files: `/js/script.js` (lines 3-4, 14-27)
- Impact: Misleading code; hard to understand intent; unnecessary complexity
- Fix approach: Delete IE/webkit detection block (lines 1-29). Modern browsers support native DOMContentLoaded.

**Missing Error Handling:**
- Issue: `js/script.js` and inline scripts in `index.html` don't validate DOM element existence before accessing
  - Line 40 in `js/script.js`: `document.getElementById("switch_default").checked = true` - no null check
  - Line 58: `document.getElementsByClassName('toggleBtn')[0]` - assumes class exists
- Files: `/js/script.js` (lines 40, 42, 58, 68), `/index.html` (lines 1743-1780)
- Impact: Silent failures when DOM changes; runtime errors in console; poor user experience
- Fix approach: Add defensive checks before DOM access. Use optional chaining (?.) or null checks.

**Inconsistent Element Targeting:**
- Issue: JavaScript mixes three DOM query methods inconsistently:
  - `getElementById()` (lines 40, 42, 52, 68 in `js/script.js`)
  - `getElementsByClassName()[0]` (line 58)
  - `getElementsByTagName('body')[0]` (lines 50, 59, 60, etc.)
  - Modern approach in new `index.html` uses `querySelector()` and `querySelectorAll()`
- Files: `/js/script.js`, `/index.html`
- Impact: Inconsistent patterns; harder to maintain; performance implications (getElementsBy* returns live collections)
- Fix approach: Standardize on `querySelector()/querySelectorAll()` or document.getElementById() for modern code. Use `const nav = document.getElementById('nav')` pattern.

**Trailing Commas in Function Calls:**
- Issue: Line 65 and 80 in `js/script.js` have trailing commas in localStorage.setItem() calls: `..., 'light',)`
- Files: `/js/script.js` (lines 65, 80)
- Impact: Non-standard syntax; may cause issues in older JS environments; flags as code quality problem
- Fix approach: Remove trailing commas after final function argument.

**Hardcoded Values Without Constants:**
- Issue: Magic numbers and strings throughout code:
  - Theme values hardcoded as strings: `'dark'`, `'light'` (appears 15+ times)
  - Scroll threshold: `window.scrollY > 20` in `index.html` line 1724
  - Animation delays and durations hardcoded in CSS
  - Email hardcoded in HTML: `Tsungju.li@grapeking.com.tw` (line 1614 in backup)
- Files: `/index.html`, `/index_backup.html`, `/js/script.js`
- Impact: Hard to maintain; difficult to update single value; no single source of truth
- Fix approach: Extract constants to top of JS file or CSS variables. Create config object for theme names, scroll thresholds, etc.

## Performance Issues

**Large Unoptimized Images:**
- Issue: Avatar image is 647KB PNG; could be 100-150KB with optimization
- Files: `/image/avatar1.png`
- Impact: Slower page load, especially on mobile networks
- Improvement path: Use ImageOptim or similar to compress to <150KB. Consider WebP format with fallback. Implement lazy loading.

**Synchronous Font Loading:**
- Issue: Google Fonts loaded via synchronous `<link>` tags (lines 11-13 in `index.html`)
- Files: `/index.html` (lines 11-13)
- Impact: Blocks render; slower First Contentful Paint
- Improvement path: Add `rel="preconnect"` (already done on lines 11-12) but should add `media="print"` to font display CSS or use `font-display: swap` to prevent FOUT.

**Inline Critical CSS Not Optimized:**
- Issue: All CSS (1000+ lines) inlined in `<head>`, but non-critical styles could be deferred
- Files: `/index.html` (lines 14-235)
- Impact: Larger initial HTML payload (~30KB); affects Time to Interactive
- Improvement path: Inline only critical above-fold CSS; defer rest via `<link>` tag with async loading.

**No Minification:**
- Issue: All HTML, CSS, JS served unminified
- Files: All files
- Impact: Larger payload sizes; slower downloads on slow connections
- Improvement path: Implement build step with minification (consider static site generator or build tool).

## Testing & Verification Gaps

**No Automated Tests:**
- Issue: No test framework, no unit tests, no integration tests, no E2E tests
- Impact: Manual testing only; risk of regressions; no confidence in changes
- Recommendation: Add basic HTML validation, Lighthouse CI for performance, or snapshot tests if using a static site generator.

**No Broken Link Detection:**
- Issue: Multiple hardcoded URLs that could break:
  - Line 1596 in `index_backup.html`: LinkedIn URL `https://www.linkedin.com/in/tjkevin` (verify format)
  - Line 1511: Google Scholar URL with user ID might need updating
  - Line 1668: CV file path `/CV/Tsung-Ju_Li_CV.pdf` (file not found in directory listing)
- Impact: Dead links hurt SEO and user experience
- Recommendation: Add link checker to CI/CD or use broken link checker tool regularly.

**Missing CV File:**
- Issue: Connect section links to `/CV/Tsung-Ju_Li_CV.pdf` (line 1668 in backup) but file not present in repository
- Files: Referenced in `/index.html` and `/index_backup.html`
- Impact: Users cannot download CV; broken user journey
- Fix approach: Add CV file to `/CV/` directory or update link to valid location.

## Security Considerations

**localStorage Usage Without Validation:**
- Issue: Theme preference stored in localStorage without content validation; theoretically could be exploited if user visits compromised site that sets malicious values
- Files: `/index.html` (lines 476, 483, 1709, 1718)
- Current mitigation: localStorage scoped to origin; theme validation enforces only 'dark'/'light' values
- Recommendation: Explicitly validate theme value against whitelist: `if (!['dark', 'light'].includes(theme)) theme = 'dark';`

**Hardcoded Email Address:**
- Issue: Personal email `Tsungju.li@grapeking.com.tw` hardcoded in HTML (visible in source and searchable by bots)
- Files: `/index.html` line 1614, `/index_backup.html` line 1614
- Risk: Spam/phishing targeting
- Mitigation: Already exposed in public portfolio; acceptable risk but consider email obfuscation if spam becomes issue

**No Content Security Policy (CSP):**
- Issue: No CSP headers defined; loads Google Fonts from googleapis.com
- Impact: Vulnerable to XSS attacks; no protection against unsafe inline scripts
- Recommendation: Add CSP header: `Content-Security-Policy: default-src 'self'; style-src 'self' 'unsafe-inline' https://fonts.googleapis.com; font-src https://fonts.gstatic.com`

**External Dependencies Without Integrity Checks:**
- Issue: Google Fonts loaded without Subresource Integrity (SRI) attributes
- Files: `/index.html` (lines 11-13)
- Impact: Vulnerable to MITM attacks on Google Fonts
- Recommendation: Add `integrity` and `crossorigin` attributes to font links.

## Fragile Areas

**Theme Toggle Implementation:**
- Files: `/index.html` (lines 471-495, 1703-1719), `/js/script.js` (lines 32-88)
- Why fragile: Two implementations that could conflict; legacy script still loads; localStorage data persists across versions
- Safe modification: Before changing theme logic, verify which implementation is actually used. Remove legacy script. Add integration tests.
- Test coverage: Manual testing only; no automated verification that theme persists correctly.

**Mobile Navigation Interaction:**
- Files: `/index.html` (lines 1226-1231, 1728-1738)
- Why fragile: Mobile nav toggle depends on specific ID selectors (`#navToggle`, `#navLinks`); no null checks
- Safe modification: Add defensive checks before addEventListener. Test on real mobile devices.

**HTML-Only Styling & JS:**
- Files: `/index.html` (embedded CSS and JS)
- Why fragile: Can't use CSS preprocessing; can't share code between pages; changes require editing 1784-line file
- Safe modification: Extract CSS and JS to separate files before making style changes. Use version control to compare before/after.

## Scaling Limits

**Single HTML File Contains Everything:**
- Current capacity: ~1784 lines in one file (`index_backup.html`)
- Limit: Beyond ~2000 lines, becomes difficult to edit and maintain
- Scaling path: Extract styles to `css/` folder, scripts to `js/` folder, consider static site generator (11ty, Hugo) if adding more pages

**No Build/Deployment Pipeline:**
- Current capacity: Manual git pushes to GitHub Pages
- Limit: Cannot minify, optimize, or automate testing
- Scaling path: Add build step with npm scripts or GitHub Actions to optimize assets, run tests, check links

## Dependencies at Risk

**Embedded MathJax 2.7.5:**
- Risk: `/js/mathjax2.7.5.js` is old version; MathJax 3.x is current
- Impact: Security vulnerabilities in old version; missing features
- Migration plan: Evaluate if MathJax needed (not used in current portfolio). If needed, update to MathJax 3.x or use KaTeX.

**Legacy tocbot Library:**
- Risk: `/js/tocbot.min.js` (0 bytes, file appears empty)
- Impact: Non-functional; unclear why included
- Migration plan: Remove if not used. If table of contents needed, add working tocbot from npm.

## Missing Critical Features

**No Responsiveness Testing Framework:**
- Problem: CSS has media queries (lines 1068-1188 in `index_backup.html`) but no automated testing
- Blocks: Cannot verify layout works across screen sizes without manual testing
- Recommendation: Add responsive design testing (e.g., Percy, Chromatic, or Lighthouse CI)

**No Analytics:**
- Problem: No tracking of visitor behavior, traffic sources, or engagement
- Blocks: Cannot measure impact of portfolio; no data for optimization
- Recommendation: Add Google Analytics or similar (remember GDPR compliance)

**No Sitemap or robots.txt:**
- Problem: No `/sitemap.xml` or `/robots.txt` for SEO
- Blocks: Search engines may not crawl all content; cannot specify crawl rules
- Recommendation: Add `sitemap.xml` with all portfolio pages; add `robots.txt` to allow all

## Technical Debt Summary

| Category | Severity | Effort | Impact |
|----------|----------|--------|--------|
| Remove legacy `js/script.js` | High | Low | Eliminates dead code, prevents conflicts |
| Extract inline CSS/JS to files | High | Medium | Enables caching, better maintainability |
| Delete unused CSS files | Medium | Low | Clean up repository |
| Remove `index_backup.html` | Medium | Low | Prevents accidental serving |
| Optimize avatar image | Medium | Low | ~500KB bandwidth savings |
| Update theme toggle to use whitelist validation | Medium | Low | Improves security |
| Add CV file or fix link | Medium | Low | Fixes broken user journey |
| Add CSP headers | Low | Medium | Improves security posture |

---

*Concerns audit: 2026-02-22*
