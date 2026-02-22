# Codebase Structure

**Analysis Date:** 2026-02-22

## Directory Layout

```
kevintsli.github.io/
├── index.html                    # Main portfolio page (entry point)
├── index_backup.html             # Backup of previous version
├── README.md                      # GitHub profile README
├── GITHUB_PROFILE_README.md       # GitHub profile information
├── CNAME                          # GitHub Pages custom domain file
├── favicon.ico                    # Site favicon
├── .planning/                     # GSD planning documents (not deployed)
│   └── codebase/                  # Architecture & code analysis
├── css/                           # Stylesheets
│   ├── normalize.css              # CSS reset (269 lines)
│   ├── base.css                   # Base typography & elements (61 lines)
│   ├── style.css                  # Main component styles (1292 lines)
│   ├── layout.css                 # Layout utilities (35 lines)
│   ├── media.css                  # Responsive breakpoints (153 lines)
│   ├── font.css                   # Font declarations (6 lines)
│   ├── variable.css               # Unused/empty (2 lines)
│   └── custom.css                 # Unused/empty (0 lines)
├── js/                            # JavaScript files
│   ├── script.js                  # Theme toggle & DOM ready logic (88 lines)
│   ├── tocbot.min.js              # Table of contents generator (minified)
│   └── mathjax2.7.5.js            # Math rendering library (minified)
├── fonts/                         # Font assets
│   ├── iconfont/                  # Custom icon fonts
│   │   ├── iconfont.js            # Icon font loader
│   │   ├── iconfont.css           # Icon font styles
│   │   ├── demo.css               # Demo styles
│   │   └── demo_index.html        # Icon preview page
│   └── lanting/                   # Lanting font family files
├── image/                         # Image assets
│   └── (avatar, screenshots, etc.)
├── 2019/                          # Archive: Old blog posts (by date)
│   └── 10/13/page/index.html
├── archives/                      # Archive index pages
│   ├── index.html                 # Main archive page
│   ├── 2019/index.html            # 2019 archive
│   └── 2019/10/index.html         # October 2019 archive
└── .git/                          # Git repository (not deployed)
```

## Directory Purposes

**Root Level:**
- Purpose: GitHub Pages entry point and site configuration
- Contains: Main HTML file, configuration files (CNAME), backup files
- Key files: `index.html` (deployed page), `README.md` (GitHub profile), `CNAME` (custom domain)

**css/ Directory:**
- Purpose: All visual styling for the site
- Contains: CSS stylesheets organized by concern (normalize, base, component styles, layout, responsive)
- Key files: `style.css` (1292 lines - all component styling), `normalize.css` (cross-browser reset), `media.css` (responsive queries)
- Note: `variable.css` and `custom.css` are empty/unused

**js/ Directory:**
- Purpose: Client-side JavaScript functionality
- Contains: Interactive scripts and third-party libraries
- Key files: `script.js` (theme toggle, DOM ready), `tocbot.min.js` (table of contents), `mathjax2.7.5.js` (math rendering)

**fonts/ Directory:**
- Purpose: Custom font assets and icon fonts
- Contains: Icon font system and typography font files
- Key files: `iconfont/iconfont.css` (icon declarations), `iconfont/iconfont.js` (icon loader), `lanting/` (font family files)

**image/ Directory:**
- Purpose: Static image assets
- Contains: Photos, screenshots, avatars used in portfolio
- Note: Some images deleted in recent commits (avatar2.jpg)

**archives/ and 2019/ Directories:**
- Purpose: Old blog post hierarchy (legacy blog structure)
- Contains: Archive index pages and old blog post HTML files
- Note: Not used in current design; maintained for backward compatibility

**.planning/ Directory:**
- Purpose: GSD planning documents (not deployed to GitHub Pages)
- Contains: Architecture analysis, conventions, testing patterns, concerns
- Note: Git-tracked but not served publicly

## Key File Locations

**Entry Points:**
- `index.html`: Main portfolio page served by GitHub Pages (497 lines, self-contained with inline CSS and JavaScript)

**Configuration:**
- `CNAME`: GitHub Pages custom domain configuration (empty or contains domain name)
- `README.md`: GitHub profile README displayed in repository view
- `GITHUB_PROFILE_README.md`: Profile information file

**Core Logic:**
- `index.html` lines 471-495: Inline JavaScript for theme toggle and scroll detection
- `js/script.js`: Legacy theme toggle logic and document.ready() polyfill (not actively used, older version)

**Core Styling:**
- `css/style.css`: All component styles including Bento grid, cards, hero section, navbar, theme variables (1292 lines)
- `css/normalize.css`: Cross-browser CSS reset (269 lines)
- `css/media.css`: Responsive design breakpoints, mobile layout adjustments (153 lines)

**Typography & Fonts:**
- `css/base.css`: Base typography, heading sizes, line heights (61 lines)
- `css/font.css`: Google Fonts imports and font-family declarations (6 lines)
- `fonts/iconfont/`: Custom SVG icon font system for social icons and section icons

**Testing:**
- No test files present in codebase

## Naming Conventions

**Files:**
- HTML files: `index.html` (standard) or kebab-case for archives (`demo_index.html`)
- CSS files: kebab-case (e.g., `normalize.css`, `media.css`, `layout.css`)
- JavaScript files: kebab-case (e.g., `tocbot.min.js`)
- Image files: kebab-case (e.g., `avatar.jpg`)

**Directories:**
- lowercase with hyphens: `css/`, `js/`, `fonts/`, `image/`
- archive folders by year: `2019/`, with month subfolders `10/`, and day subfolders `13/`

**CSS Classes:**
- kebab-case for all classes: `.bento-card`, `.hero-title`, `.card-header`, `.social-btn`, `.theme-toggle`
- Semantic naming reflecting component purpose: `.nav-links`, `.hero-badge`, `.insight-card`
- Modifier classes with hyphens: `.col-span-2`, `.row-span-2`, `.animate-delay-1`, `.dark-theme`

**HTML IDs:**
- kebab-case: `#navbar`, `#themeToggle`, `#about`, `#expertise`, `#impact`, `#insights`
- Used for section anchors and interactive element targets

**CSS Custom Properties:**
- double-dash prefix with descriptive names: `--bg-primary`, `--text-secondary`, `--accent-primary`, `--gradient-primary`, `--radius-md`
- semantic naming for design tokens: `--border-glow`, `--btn-glow-shadow`, `--nav-scrolled-bg`

## Where to Add New Code

**New Section/Card:**
- Add HTML: `index.html` inside `<div class="bento-grid">` (lines 282-462)
- Add CSS: `css/style.css` with new class styles (follow existing pattern)
- Styling guidance: Use `.bento-card` base class, apply grid span modifiers (`.col-span-2`, `.row-span-2`), use CSS variables for colors

**New Interactive Feature:**
- Add JavaScript: Inline `<script>` tag at end of `index.html` (lines 471-495)
- Pattern: Use vanilla JavaScript with DOM API, no frameworks
- State management: Use `localStorage` API for persistence (follow theme toggle pattern)

**New Stylesheet:**
- Don't create new files - add to existing `css/style.css` (all component styles centralized)
- If organizing by concern: Extend `css/layout.css` or `css/media.css`
- Keep variable definitions in HTML `<style>` tag (lines 14-235) for easy access

**New Font/Icon:**
- Add to `fonts/` directory, import in `css/font.css`
- For icons: Reference the existing `fonts/iconfont/` system

**New Image Asset:**
- Place in `image/` directory
- Reference in HTML with relative path: `<img src="/image/filename.jpg">`

## Special Directories

**archives/ and 2019/:**
- Purpose: Old blog post structure (legacy, not active)
- Generated: No, manually maintained for backward compatibility
- Committed: Yes, archived for SEO and historical links

**.planning/:**
- Purpose: GSD planning and analysis documents
- Generated: Yes, by GSD command tools
- Committed: Yes, tracked in git
- Note: Not deployed to GitHub Pages (not in web-accessible directories)

**.git/:**
- Purpose: Git repository metadata and history
- Generated: Yes, by git
- Committed: N/A (git internal)
- Note: Not deployed to GitHub Pages

## Current HTML Structure

**Main Sections in `index.html`:**
1. Ambient background orbs (decorative, fixed position)
2. Navigation bar (sticky, responsive, theme toggle)
3. Hero section (title, badge, CTA button)
4. Bento grid with cards:
   - About / Background (col-span-2, row-span-2)
   - Expertise cards (AI & R&D, Regulatory Science)
   - Impact Journey / Milestones
   - Selected Works / Publications
   - Recent Talks & Thoughts / Insights (col-span-3)
   - Connect & Socials (col-span-3)
5. Footer

---

*Structure analysis: 2026-02-22*
