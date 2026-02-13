# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

Personal website for Mike Privette (mikeprivette.com). Single-file static site hosted on GitHub Pages. Zero build dependencies — no framework, no bundler, no Jekyll.

## Architecture

- **Single `index.html`** — all HTML, CSS, and JS in one file
- **Terminal aesthetic**: JetBrains Mono font, scanline overlay, cursor blink, `##` / `~/` prompt motifs
- **Dark/light theme toggle**: persists via localStorage, respects `prefers-color-scheme`
- **CSS custom properties** for theming — all colors defined in `:root` / `[data-theme]` selectors
- **`.nojekyll`** — tells GitHub Pages to skip Jekyll and serve files as-is
- **Custom domain**: `CNAME` file maps to `mikeprivette.com`

## Deployment

Push to `master` branch. GitHub Pages serves `index.html` directly (no build step). Deploys in seconds.

## Local Development

Open `index.html` in a browser. No server needed. For live reload:

```bash
# Any static file server works
python3 -m http.server 8000
```

## Key Files

| File | Role |
|------|------|
| `index.html` | The entire site — HTML, CSS, JS, no dependencies |
| `CNAME` | Custom domain (`mikeprivette.com`) |
| `.nojekyll` | Disables Jekyll processing on GitHub Pages |
| `_archive/` | Previous Jekyll-based site (preserved for reference) |

## Editing Content

All content lives in the `<body>` of `index.html`. Sections: About, Work, Now, Previously, Elsewhere. The "Now" section should be updated when priorities shift.

## Theme System

Colors are CSS custom properties. To modify the palette, edit the `[data-theme="dark"]` and `[data-theme="light"]` blocks in the `<style>` tag. The `theme-color` meta tag is updated via JS on toggle.

## Future: Hugo Blog

When blogging is needed, migrate to Hugo (see `REBUILD-PLAN.md` for the migration path). The current `index.html` becomes a Hugo layout template.
