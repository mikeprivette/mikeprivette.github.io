# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

Personal website and blog for Mike Privette (mikeprivette.com). Built with Hugo, deployed via GitHub Actions to GitHub Pages. Terminal-aesthetic design, zero JS dependencies.

## Build & Dev Commands

```bash
# Local development (live reload on http://localhost:1313)
hugo server -D

# Production build
hugo --gc --minify

# Create a new blog post
hugo new writing/my-post-title.md
```

## Architecture

- **Hugo static site generator** with custom layouts (no external theme)
- **Terminal aesthetic**: JetBrains Mono font, scanline overlay, cursor blink, `##` / `~/` prompt motifs
- **Dark/light theme toggle**: persists via localStorage, respects `prefers-color-scheme`
- **CSS custom properties** for theming — all colors defined in `:root` / `[data-theme]` selectors
- **GitHub Actions** builds Hugo and deploys to Pages on push to `master`

## Key Files

| Path | Role |
|------|------|
| `hugo.toml` | Site config (title, params, menus, outputs) |
| `layouts/_default/baseof.html` | Base HTML shell (head, toggle, footer, JS) |
| `layouts/index.html` | Homepage template |
| `layouts/writing/list.html` | Blog listing page |
| `layouts/writing/single.html` | Individual blog post |
| `layouts/404.html` | Custom 404 page |
| `layouts/partials/head.html` | `<head>` meta tags, OG, JSON-LD, FOUC script |
| `layouts/partials/css.html` | All CSS (inline `<style>`) |
| `layouts/partials/theme-js.html` | Theme toggle JS |
| `layouts/partials/analytics.html` | GA4 snippet (enabled via `hugo.toml`) |
| `content/writing/` | Blog posts (Markdown) |
| `static/CNAME` | Custom domain config |
| `static/images/og-image.png` | Social sharing image |
| `.github/workflows/hugo.yml` | CI/CD: build Hugo → deploy to Pages |

## Editing Content

- **Homepage sections** (About, Work, Now, Previously, Elsewhere): edit `layouts/index.html`
- **Blog posts**: add `.md` files to `content/writing/`. Front matter: `title`, `date`, `description`, `draft`
- **"Now" section**: update when priorities shift

## Theme System

Colors are CSS custom properties in `layouts/partials/css.html`. Edit `[data-theme="dark"]` and `[data-theme="light"]` blocks. The `theme-color` meta tag syncs via JS.

## Deployment

Push to `master`. GitHub Actions builds Hugo and deploys to Pages. CNAME is in `static/` so it copies to the build output automatically.

## Google Analytics

GA4 is configured via `hugo.toml` → `params.googleAnalytics`. Uncomment and set the `G-XXXXXXXXXX` measurement ID. The old UA-166194158-1 (Universal Analytics) is defunct.
