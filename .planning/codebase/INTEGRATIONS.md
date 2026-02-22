# External Integrations

**Analysis Date:** 2026-02-22

## APIs & External Services

**Typography & Fonts:**
- Google Fonts API
  - What it's used for: Loads Inter and JetBrains Mono fonts
  - URLs: `https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&family=JetBrains+Mono:wght@400;500&display=swap`
  - Preconnect domains: `https://fonts.googleapis.com`, `https://fonts.gstatic.com`
  - Integration point: `index.html` lines 11-13
  - Attributes: Crossorigin with preconnect hints for performance

**Social/Professional Links:**
- LinkedIn
  - URL: `https://linkedin.com/` (generic) and `https://linkedin.com/in/tjkevin` (specific profile)
  - Integration: Social button in hero section (`index.html` line 452)
  - No API integration - external link only

- Google Scholar
  - URL: `https://scholar.google.com/citations?user=8Y_Rjk4AAAAJ&hl=zh-TW`
  - Integration: Social button in hero section (`index.html` line 455)
  - No API integration - external link only

- GitHub
  - URL: `https://github.com/kevintsli`
  - Integration: Social button in hero section (`index.html` line 458)
  - No API integration - external link only

## Data Storage

**Databases:**
- None - static site only

**Browser Storage:**
- localStorage API
  - Purpose: Theme preference persistence
  - Key: `theme`
  - Values: `"dark"` or `"light"`
  - Implementation: `js/script.js` lines 37-81, `index.html` lines 476-483

**File Storage:**
- GitHub Pages static file hosting
- Repository: `kevintsli.github.io`
- No dynamic file upload capability

**Caching:**
- Browser cache (default HTTP caching headers from GitHub Pages)
- Font caching via Google Fonts CDN
- No custom caching layer

## Authentication & Identity

**Auth Provider:**
- None - static public site
- No login system, user accounts, or protected content

**Public Access:**
- All content publicly accessible
- No authorization checks required

## Monitoring & Observability

**Error Tracking:**
- None detected
- No integration with Sentry, LogRocket, or similar services

**Analytics:**
- None detected in current code
- No Google Analytics, Mixpanel, or tracking scripts

**Logs:**
- Browser console logging only (development reference in `js/script.js`)
- No server-side logging capability (static site)

## CI/CD & Deployment

**Hosting:**
- GitHub Pages (kevintsli.github.io)
- Custom domain: CNAME file present (empty, not actively configured)
- Direct push to master branch triggers deployment

**CI Pipeline:**
- GitHub Pages automatic deployment
- No custom CI/CD configuration detected (no `.github/workflows/`)
- No build step - files served as-is

**Version Control:**
- Git repository: `.git/` directory
- Default branch: `master`
- Recent commits visible in git history

## Environment Configuration

**Required env vars:**
- None - static site with no backend

**Secrets location:**
- No secrets in codebase
- CNAME file is empty (no domain secrets)

**Configuration Files:**
- None detected
- All configuration hardcoded in HTML/CSS/JavaScript

## Webhooks & Callbacks

**Incoming:**
- None - static site cannot receive webhooks

**Outgoing:**
- None - client-side only, no outbound requests to webhooks

**Form Submissions:**
- No form processing - site is read-only

## External Assets & CDNs

**Content Delivery:**
- Google Fonts CDN
  - Domains: `fonts.googleapis.com`, `fonts.gstatic.com`
  - Protocol: HTTPS
  - Preconnect hints in place for optimization

**Performance Headers:**
- Preconnect established for font resources
- No other CDN integrations detected

## Client-Side Dependencies

**Third-party JavaScript:**
- `tocbot.min.js` - Table of contents generation (present but unused in current portfolio)
- `mathjax2.7.5.js` - MathJax 2.7.5 for mathematical rendering (present but unused)
- All other JavaScript is inline or custom (`script.js`)

**No npm dependencies:**
- Pure vanilla implementation
- No node_modules or package manager configuration

## Legal & Tracking

**Meta Information:**
- OpenGraph-style content in meta tags
- Description: "Kevin Li, Ph.D. — Assistant Department Manager at Grape King Bio..."
- Language: English (lang="en")

**CNAME/Domain:**
- CNAME file present: `/CNAME` (empty file, 0 bytes)
- Currently serving from GitHub Pages default domain

---

*Integration audit: 2026-02-22*
