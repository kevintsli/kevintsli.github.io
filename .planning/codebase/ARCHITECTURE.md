# Architecture

**Analysis Date:** 2026-02-22

## Pattern Overview

**Overall:** Single-Page Static Site Architecture

**Key Characteristics:**
- Pure HTML/CSS/JavaScript static site (no build process, no framework)
- Client-side theme persistence and navigation
- Component-based visual design using CSS Grid (Bento layout)
- Inline CSS styling within HTML document
- Self-contained, no external dependencies or API integrations

## Layers

**Presentation Layer:**
- Purpose: Render visual components and handle user interactions
- Location: `index.html` (inline styles and scripts)
- Contains: HTML markup, CSS styling, JavaScript interactivity
- Depends on: Browser DOM API, localStorage API
- Used by: End users visiting the portfolio site

**Styling Layer:**
- Purpose: Define visual design system, themes, and responsive layouts
- Location: `css/` directory (style.css, normalize.css, media.css, layout.css, base.css, font.css)
- Contains: CSS rule sets organized by purpose (reset, typography, layout, responsive breakpoints)
- Depends on: CSS variables defined in HTML head
- Used by: Presentation layer for visual rendering

**Interaction Layer:**
- Purpose: Handle user input and browser state management
- Location: Inline `<script>` tag in `index.html` (lines 471-495) and `js/script.js`
- Contains: Theme toggle logic, navbar scroll detection, localStorage persistence
- Depends on: DOM API, localStorage API
- Used by: User interactions (theme switching, navigation)

## Data Flow

**Theme Persistence Flow:**

1. Page loads → JavaScript checks `localStorage` for saved theme preference
2. If theme exists → Apply to `data-theme` attribute on `<html>`
3. User clicks theme toggle button → Event listener captures click
4. Toggle switches theme (dark ↔ light) → Updates `data-theme` attribute
5. New theme saved to `localStorage` → Persists across sessions

**Navigation & Scroll Flow:**

1. User scrolls page → `scroll` event fires on window
2. If `scrollY > 50px` → Add `.scrolled` class to navbar
3. Navbar CSS applies glass-morphism effect (backdrop blur, background color)
4. User clicks nav link → Smooth scroll to target section ID

**Visual Component Rendering:**

1. HTML defines Bento Grid layout (3-column on desktop, 1-column on mobile)
2. CSS applies responsive breakpoints at `768px` media query
3. Cards use CSS Grid subgrid for internal layouts (stats-grid, insights-grid)
4. Animations apply on page load (fade-up, pulse-dot, orb-drift)

## Key Abstractions

**Bento Card Component:**
- Purpose: Reusable container for content sections
- Examples: `<div class="bento-card col-span-2 row-span-2">` (About section), `<div class="bento-card">` (Expertise cards)
- Pattern: CSS class-based composition with grid span modifiers (col-span-2, row-span-2)
- Styling: Semi-transparent background, subtle border, hover effects, backdrop blur

**Theme System:**
- Purpose: Support light/dark mode switching
- Examples: CSS variables in `:root` and `[data-theme="light"]` selectors
- Pattern: CSS custom properties for colors, transitions based on `data-theme` attribute
- Data storage: Browser localStorage under key `theme`

**Card Header Component:**
- Purpose: Label sections with icon and category text
- Examples: `.card-header` with nested SVG and text
- Pattern: Monospace font, uppercase transform, accent color gradient text

**Insight Card:**
- Purpose: Display recent talks and thoughts in structured grid
- Examples: `.insight-card` inside `.insights-grid` (2-column layout)
- Pattern: Meta information (year, venue), title, description with consistent typography

## Entry Points

**HTML Document:**
- Location: `/index.html`
- Triggers: Browser loads URL (GitHub Pages serves this as index)
- Responsibilities: Define entire page structure, include all styles and scripts, render sections (hero, bento grid, footer)

**Theme Toggle Button:**
- Location: `index.html` line 255 `#themeToggle`
- Triggers: User click event
- Responsibilities: Toggle between dark/light themes, persist choice to localStorage

**Scroll Event Listener:**
- Location: Inline script line 488-494
- Triggers: User scrolls page
- Responsibilities: Detect scroll position, apply visual effects to navbar (glass effect, border)

## Error Handling

**Strategy:** Defensive programming with fallback checks

**Patterns:**
- localStorage access wrapped in condition: `window.localStorage && window.localStorage.getItem('theme')`
- Theme attribute defaults to 'dark' if nothing in localStorage: `localStorage.getItem('theme') || 'dark'`
- DOM element selection assumes elements exist (no null checks); potential issue if HTML structure changes
- JavaScript uses legacy document.ready() pattern for cross-browser compatibility (even IE 6 support)

## Cross-Cutting Concerns

**Logging:** No logging framework; uses `console` API if debugging needed (not present in current code)

**Validation:** No client-side validation; form interactions not present

**Authentication:** Not applicable; public portfolio site

**Styling Consistency:** CSS variables define entire design system:
- Colors: Primary accent (#ef4444 red), secondary (#f97316 orange), tertiary (#f59e0b amber)
- Typography: Inter font for body, JetBrains Mono for code/labels
- Spacing: Base grid of 4px (padding values: 4px, 8px, 12px, 16px, 20px, 24px, 28px, 32px)
- Borders: Subtle semi-transparent borders with hover glow effect

---

*Architecture analysis: 2026-02-22*
