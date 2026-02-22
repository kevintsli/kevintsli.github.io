# Coding Conventions

**Analysis Date:** 2026-02-22

## Naming Patterns

**Files:**
- JavaScript files: lowercase with `.js` extension - `script.js`, `tocbot.min.js`
- CSS files: lowercase with `.css` extension - `style.css`, `custom.css`, `variable.css`
- HTML files: lowercase - `index.html`
- Minified files: use `.min.js` suffix - `tocbot.min.js`, `mathjax2.7.5.js`

**Functions:**
- Camel case for functions: `toggleTheme()`, `addEventListener()`
- Utility/helper functions: lowercase with underscores when used in custom code
- Event handlers: descriptive verb-based names like `toggleTheme`, `handleScroll`

**Variables:**
- Local variables: camelCase - `currentTheme`, `isDark`, `navElement`
- Constants: camelCase (not UPPER_SNAKE_CASE) - `currentTheme`, `scrollY`
- Flags/booleans: prefix with `is` or boolean-indicating verbs - `isDark`, `wk` (webkit detection)
- Private/internal: prefix with underscore - `_Blog` (namespace object)
- Loop variables: single letters acceptable for short loops - `i`, `t` (time)

**Types:**
- No TypeScript in this codebase - vanilla JavaScript only
- DOM element references: suffix with Element when verbose - `htmlElement`, `themeToggle`
- CSS custom properties: kebab-case with double dashes - `--bg-primary`, `--accent-primary`, `--font-sans`

## Code Style

**Formatting:**
- No Prettier configuration detected
- Indentation: 4 spaces (observed in `script.js`)
- Line length: no strict limit enforced
- Semicolons: used inconsistently - present in some statements, occasionally omitted

**Linting:**
- No ESLint configuration detected
- No formal linting rules enforced
- Mixed code quality - some sections show inconsistent style (e.g., inconsistent spacing, trailing commas)

**Spacing:**
- Spaces around operators: consistent - `var ie = !!`, `isDark === 'dark'`
- Function declarations: space before parentheses inconsistent
- Conditional blocks: proper spacing observed

## Import Organization

**JavaScript Modules:**
- No module import system (no ES6 imports/exports)
- Inline script tags in HTML: `<script>` tags at end of HTML body
- Third-party libraries loaded separately: `tocbot.min.js`, `mathjax2.7.5.js` loaded in header or body
- Global namespace pollution: uses `window._Blog` as namespace object to avoid conflicts

**CSS:**
- Multiple CSS files included separately in `<link>` tags
- File organization by concern: `normalize.css`, `font.css`, `style.css`, `layout.css`, `custom.css`, `variable.css`
- CSS variables (custom properties) defined in `variable.css` and inline in `index.html` within `<style>` tags
- Google Fonts imported via CDN link

**Path Aliases:**
- Absolute paths used for assets - `/js/`, `/css/`, `/image/`, `/fonts/`
- No path aliases or import maps configured

## Error Handling

**Patterns:**
- Try-catch blocks used for browser compatibility detection in `script.js` (IE/webkit detection)
- Feature detection over error catching: checks for `window.attachEvent`, `addEventListener` before using
- Graceful degradation: fallback mechanisms for old browsers
- No explicit error logging or error boundaries
- Silent failures: errors are caught but not reported (e.g., in `document.ready()` function)

**Error conditions:**
```javascript
// Example: Silent error handling with try-catch
try {
    d.documentElement.doScroll('left');
    run();
} catch (err) {
    setTimeout(arguments.callee, 0);  // Retry without reporting
}
```

**No error objects thrown** - focus on feature detection and graceful fallback instead

## Logging

**Framework:** None - uses `console` only in minified third-party code (`tocbot`)

**Patterns:**
- No custom logging implemented in main `script.js`
- Third-party libraries may contain `console.warn()` calls (observed in minified tocbot)
- No logging framework (Winston, debug, etc.) integrated
- Development logging not evident in production code

## Comments

**When to Comment:**
- JSDoc-style comments for significant functions: `// declaraction of document.ready() function.`
- Inline comments for non-obvious logic: `// toggleTheme function. this script shouldn't be changed.`
- Section headers with comment blocks using equals: `/* ===== CSS Reset & Base ===== */`
- HTML comments for component separation: `<!-- Insights / Thoughts Sector (RESTORED) -->`

**JSDoc/TSDoc:**
- Not used - no TypeScript and minimal documentation
- Functional descriptions in plain English comments above functions
- CSS variable comments inline with color descriptions: `--accent-primary: #ef4444; /* red-500 */`

**Comment style:**
- Single-line comments for brief notes: `// mobile`
- Multi-line CSS comments for sections: `/* ===== Dark Theme ===== */`
- Short comments allowed: typos noted (`// declaraction`, `// moblie` in script.js)

## Function Design

**Size:**
- Functions kept relatively small and focused
- Theme toggle: ~40 lines including event listeners
- Document ready polyfill: ~30 lines for cross-browser support
- Event listeners: inline or as small anonymous functions

**Parameters:**
- Minimal parameter passing - most functions access global state
- Anonymous functions used for callbacks: `function (f) { ... }`, `function () { ... }`
- No destructuring or rest parameters observed

**Return Values:**
- Functions often used for side effects (DOM manipulation, state changes)
- Return values not consistently used
- Theme toggle: returns undefined, modifies state directly via `classList`, `localStorage`

## Module Design

**Exports:**
- No module system (ES6 modules, CommonJS not used)
- Global namespace object: `window._Blog` acts as module for shared functions
- Functions attached to window object for access: `_Blog.toggleTheme = function () { ... }`

**Barrel Files:**
- Not applicable - no module system
- CSS organization mimics this concept: separate files for concerns (`variable.css`, `layout.css`)

**Namespacing:**
- Uses `window._Blog` namespace to avoid global scope pollution
- Pattern: `var _Blog = window._Blog || {};` followed by `_Blog.functionName = function () { ... };`

**File organization:**
- Related CSS split across multiple files but not true modules
- JavaScript kept minimal: only `script.js` for custom code (87 lines)
- Third-party code isolated: `tocbot.min.js`, `mathjax2.7.5.js`

## CSS Conventions

**Variable naming:**
- Semantic names for colors: `--bg-primary`, `--text-secondary`, `--accent-primary`
- Functional purpose in name: `--nav-scrolled-bg`, `--btn-glow-shadow`
- Numerical scale for sizes: `--radius-sm`, `--radius-md`, `--radius-lg`, `--radius-xl`
- Theme variants: variables defined per `data-theme` value

**Selector naming:**
- BEM-like patterns: `.bento-card`, `.card-header`, `.card-title`
- Utility classes: `.col-span-2`, `.row-span-2`, `.animate-delay-1`
- State classes: `.scrolled`, `.dark-theme`

**Responsive approach:**
- Mobile-first: base styles for mobile, `@media (max-width: 768px)` for desktop adjustments
- Relative units: `clamp()` for responsive typography, `vw` for viewport-relative sizes

---

*Convention analysis: 2026-02-22*
