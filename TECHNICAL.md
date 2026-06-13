# Technical Documentation — Blue Lagoon Consulting Services Website

## Overview

The site is a **single-file static website** (`index.html`) serving as a professional company page for Blue Lagoon Consulting Services. It has no server-side rendering, no build step, and no JavaScript framework — it runs entirely in the browser from a local or hosted HTML file.

---

## File Structure

```
Blue Lagoon/
├── index.html                           # The entire website (single file)
├── blue_lagoon_logo_version_c_revised.svg  # Company logo (local asset)
├── CLAUDE.md                            # Claude Code project instructions
├── AGENTS.md                            # Next.js agent compatibility notes
├── .claude/
│   └── rules/
│       ├── design-recreation-workflow.md
│       ├── design-fidelity.md
│       └── technical-defaults.md
│
│   # Next.js scaffold (not used by index.html — retained for future app development)
├── package.json
├── next.config.ts
├── tsconfig.json
├── postcss.config.mjs
├── eslint.config.mjs
├── next-env.d.ts
└── src/
    └── app/
        ├── layout.tsx
        ├── page.tsx
        ├── globals.css
        └── favicon.ico
```

---

## Stack

### Runtime (what the browser loads)

| Layer | Technology | Version / Source |
|---|---|---|
| Markup | HTML5 | — |
| Styling | Tailwind CSS | CDN (`https://cdn.tailwindcss.com`) |
| Typography | Inter (Google Fonts) | CDN (`https://fonts.googleapis.com`) |
| Icons | Heroicons-style inline SVG | Embedded in HTML, no external dependency |
| Scripting | None | No JavaScript runtime or framework |

### Development Environment (local tooling only)

| Tool | Version | Purpose |
|---|---|---|
| Node.js | v24.x | Runtime for dev tooling |
| Next.js | 16.2.7 | Scaffolded for future app development (unused by `index.html`) |
| React | 19.2.4 | Bundled with Next.js scaffold |
| Tailwind CSS (npm) | ^4 | Available for future Next.js use; `index.html` uses CDN instead |
| Puppeteer | ^25.1.0 | Used during development to screenshot and compare rendered pages |
| TypeScript | ^5 | Next.js scaffold typing |
| ESLint | ^9 | Linting (flat config format via `eslint.config.mjs`) |

---

## External Dependencies

### CDN Dependencies (required at runtime)

Both of these must be reachable when the page loads. If hosting offline or in a restricted network, they must be self-hosted.

#### 1. Tailwind CSS CDN
```html
<script src="https://cdn.tailwindcss.com"></script>
```
- Loads the full Tailwind CSS v3-compatible build with JIT compiler
- Configured inline via `tailwind.config` in a `<script>` block
- Custom theme tokens defined:

| Token | Value | Usage |
|---|---|---|
| `navy` | `#032D60` | Nav, footer, credential card backgrounds |
| `navy-deep` | `#021E42` | Hero gradient start, contact section |
| `bl-blue` | `#1589EE` | Primary brand blue |
| `bl-cyan` | `#00A1E0` | Secondary cyan accent |
| `bl-light` | `#5EB8FF` | Light blue for skill tags and subtitles |
| Font family | `Inter` | Body font, overrides Tailwind default |

#### 2. Google Fonts — Inter
```html
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800;900&display=swap" rel="stylesheet" />
```
- Weights loaded: 400, 500, 600, 700, 800, 900
- Applied globally via `body { font-family: 'Inter', sans-serif; }`

### Local Assets

| File | Type | Used in |
|---|---|---|
| `blue_lagoon_logo_version_c_revised.svg` | SVG vector graphic | Navbar, footer |

**Logo display note:** The SVG has a `viewBox="0 0 690 330"` with content (cloud mark + text) occupying approximately the top 55% of the viewBox. To display it proportionally in the fixed navbar, the image is rendered at `300×143px` inside a `300×80px` clipping container with `overflow: hidden` and `margin-top: -6px` to center the content region within the nav bar height.

---

## External Link Integrations

These are navigational links only — no API calls or data exchange occurs.

| Label | URL | Opens |
|---|---|---|
| LinkedIn | `https://www.linkedin.com/in/mbrybag/` | New tab (`target="_blank"`) |
| GitHub | `https://github.com/mikeoak79` | New tab (`target="_blank"`) |
| Email | `mailto:mbrybag@gmail.com` | Default mail client |

All external links include `rel="noopener noreferrer"` for security.

---

## Page Sections

| Section ID | Element | Background | Purpose |
|---|---|---|---|
| *(nav)* | `<nav>` fixed | `#032D60` navy | Site navigation + logo |
| *(hero)* | `<section>` | `linear-gradient(160deg, #021E42, #032D60, #0a3a7a)` | Hero headline + stats bar |
| `#about` | `<section>` | White | Company intro + credentials card |
| `#clients` | `<section>` | `gray-50` | KIPP Foundation + Chronosphere client cards |
| `#services` | `<section>` | `#032D60` navy | 6-card services grid |
| `#skills` | `<section>` | White | 4-category technical skills grid |
| `#contact` | `<section>` | `#021E42` | LinkedIn, GitHub, email cards |
| *(footer)* | `<footer>` | `#032D60` navy | Logo, copyright, social icons |

---

## CSS Architecture

All styles are in two places within `index.html`:

### Tailwind utility classes (inline)
Used for all layout, spacing, color, and typography. The CDN JIT compiler generates only the classes referenced in the HTML.

### Custom `<style>` block (8 rules)

| Class | Purpose |
|---|---|
| `.nav-bg` | Solid navy navbar background |
| `.hero-bg` | Multi-stop linear gradient for the hero section |
| `.wave-divider` | Removes line-height gap under the SVG wave element |
| `.bl-gradient-text` | Blue→cyan gradient applied as text fill |
| `.bl-gradient-bg` | Blue→cyan gradient applied as element background |
| `.skill-tag` | Semi-transparent blue pill style for skill badges |
| `.card-hover` | Lift + shadow transition on hover for client/feature cards |
| `.section-rule` | 48px gradient rule decorating section headings |

---

## Deployment

The site is currently opened as a local file (`file:///Users/piaosmacbookair/Blue%20Lagoon/index.html`). To deploy:

- **Static hosting (recommended):** Drop `index.html` and `blue_lagoon_logo_version_c_revised.svg` into any static host (GitHub Pages, Netlify, Vercel static, S3 + CloudFront). No build step required.
- **Next.js app (future):** The scaffolded Next.js 16 app in `src/` can be developed separately and replace or coexist with the static site. Run with `npm run dev` (port 3000).

### Hosting requirements
- No server-side runtime needed
- CDN access required at page load (Tailwind CDN + Google Fonts); self-host both if deploying to air-gapped or CDN-restricted environments
- Both files must be served from the same directory (or `src` attribute updated) for the logo to load

---

## Browser Compatibility

Uses modern CSS features (CSS custom properties, `backdrop-filter`, gradient text via `-webkit-background-clip`). Targets evergreen browsers (Chrome 90+, Firefox 90+, Safari 14+, Edge 90+). No polyfills are included.
