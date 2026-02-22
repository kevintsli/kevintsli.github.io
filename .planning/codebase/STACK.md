# Technology Stack

**Analysis Date:** 2026-02-22

## Languages

**Primary:**
- HTML5 - Markup for portfolio pages (`index.html`)
- CSS3 - Styling and animations
- JavaScript (ES5+) - Client-side interactivity (`js/script.js`)

**Secondary:**
- Markdown - Documentation (README.md, GITHUB_PROFILE_README.md)

## Runtime

**Environment:**
- Browser-based (static site)
- No server runtime required

**Deployment Target:**
- GitHub Pages (kevintsli.github.io)
- Static file hosting

## Frameworks & Libraries

**Frontend:**
- No framework used - vanilla HTML/CSS/JavaScript
- Pure DOM manipulation for interactivity

**Build/Dev:**
- No build tool required
- Direct file serving via GitHub Pages

## Key Dependencies

**External Resources:**
- Google Fonts API - Typography
  - Font families: Inter (300-700 weight), JetBrains Mono (400-500 weight)
  - Link: `https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&family=JetBrains+Mono:wght@400;500&display=swap`
  - CDN delivery via `https://fonts.googleapis.com` and `https://fonts.gstatic.com`

**Icons:**
- Custom iconfont implementation
  - Files: `fonts/iconfont/iconfont.js`, `fonts/iconfont/iconfont.css`
  - SVG inline icons in HTML

**Libraries:**
- tocbot.min.js - Table of contents generation
  - Location: `js/tocbot.min.js`
- mathjax2.7.5.js - Mathematical rendering (present but not actively used in current site)
  - Location: `js/mathjax2.7.5.js`

## CSS Architecture

**Stylesheet Organization:**
- `css/normalize.css` - CSS reset (normalize.css v8.0.0)
- `css/base.css` - Base element styling (61 lines)
- `css/variable.css` - CSS custom properties (2 lines)
- `css/layout.css` - Layout patterns (35 lines)
- `css/media.css` - Responsive media queries (153 lines)
- `css/style.css` - Main component styles (1292 lines)
- `css/font.css` - Font face declarations (6 lines)
- `css/custom.css` - Empty placeholder (0 lines)

**CSS Features Used:**
- CSS Variables (custom properties) for theming
- CSS Grid - Bento grid layout (`grid-template-columns: repeat(3, 1fr)`)
- CSS Flexbox - Navigation and card layouts
- CSS Animations - Fade-up animations, orb drift effects
- CSS Transitions - Smooth hover and scroll effects
- Backdrop filters - Glassmorphism effects (`backdrop-filter: blur()`)
- Gradient backgrounds - Red to orange gradient (`linear-gradient(135deg, #ef4444 0%, #f97316 100%)`)

## Configuration

**Theme System:**
- Dark/Light theme toggle
- Storage: `localStorage` browser API
- Key: `theme` (values: "dark" or "light")
- Default theme: Dark (`data-theme="dark"`)
- CSS variable theming via `:root` selector

**Responsive Design:**
- Mobile-first breakpoint at `768px` (tablet/desktop)
- Fluid typography using `clamp()` function
- Grid collapses to single column on mobile

**Favicon:**
- Location: `/favicon.ico`
- Format: ICO file (15,086 bytes)

## Platform Requirements

**Development:**
- Text editor or IDE
- Git (for version control)
- No npm/package.json - vanilla HTML/CSS/JS

**Production:**
- HTTP/HTTPS web server (GitHub Pages provides this)
- Modern browser with ES5+ support
- Support for CSS Grid, Flexbox, custom properties, backdrop-filter

**Browser Compatibility:**
- Target: Modern browsers (Chrome, Firefox, Safari, Edge)
- Features: CSS Grid, Backdrop-filter, localStorage
- Graceful degradation for older browsers

## Static Assets

**Images:**
- Avatar images: `image/` directory
- Favicon: `/favicon.ico` (15,086 bytes)

**Fonts:**
- System fonts + Google Fonts fallback
- Iconfont in `/fonts/iconfont/`

**Archive Files:**
- Backup HTML: `index_backup.html` (66,640 bytes)
- Previous blog posts in `/2019/` and `/archives/`

---

*Stack analysis: 2026-02-22*
