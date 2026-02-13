# mikeprivette.com — Rebuild Plan

## Summary

Replace current site with a single-file, terminal-aesthetic personal page. Dark/light mode toggle, retro-clean design, trivially maintainable. Hosted on GitHub Pages as-is.

---

## What's Included (index.html)

- **Dark + light theme** with toggle (persists via localStorage, respects system preference)
- **Terminal aesthetic**: JetBrains Mono, scanline overlay, cursor blink, `##` and `~/` prompt motifs
- **Sections**: About, Work, Now, Previously, Elsewhere (links)
- **Zero dependencies**: No build step, no framework, no JS libraries
- **Mobile responsive**
- **Light mode palette**: Warm off-white (`#f6f5f0`) with muted greens — reads like aged paper on a terminal

---

## Deployment (GitHub Pages)

### Step 1: Update your repo

Your existing repo is likely `mikeprivette/mikeprivette.github.io` or has a custom domain configured.

```bash
# Clone your existing repo (if not already local)
git clone https://github.com/mikeprivette/mikeprivette.github.io.git
cd mikeprivette.github.io

# Back up current site
mkdir _archive
mv index.html _archive/index_old.html
# Move any other current files to _archive as needed

# Drop in the new index.html
# (copy the provided index.html to the repo root)

# Ensure CNAME file exists for custom domain
echo "mikeprivette.com" > CNAME

# Commit and push
git add .
git commit -m "Rebuild: terminal-aesthetic single-page site"
git push origin main
```

### Step 2: Verify GitHub Pages settings

1. Go to repo → Settings → Pages
2. Source: Deploy from branch (`main`, root `/`)
3. Custom domain: `mikeprivette.com` (should already be set)
4. Enforce HTTPS: ✅

### Step 3: DNS (skip if already working)

Your DNS should already point to GitHub Pages. No changes needed unless you're migrating from another host.

---

## Adding a Blog (Future)

### Recommended: Hugo on GitHub Pages

When you're ready to add lightweight blogging, migrate from a single HTML file to Hugo. This preserves the single-file simplicity while adding markdown-based posts.

**Why Hugo over other options:**
- Fastest static site generator (sub-second builds)
- Single binary, no Node/Ruby dependency chain
- GitHub Actions handles the build — you just push markdown
- You can replicate the exact current design as a Hugo theme
- Posts are just `.md` files in a folder

**Migration path:**

```
mikeprivette.github.io/
├── config.toml           # Site config
├── content/
│   └── writing/          # Blog posts as .md files
│       ├── _index.md
│       └── 2026-02-some-post.md
├── layouts/
│   ├── index.html        # Your current homepage (adapted as template)
│   ├── _default/
│   │   ├── list.html     # Blog listing page
│   │   └── single.html   # Individual post template
│   └── partials/
│       ├── head.html
│       └── footer.html
├── static/
│   └── CNAME
└── .github/
    └── workflows/
        └── hugo.yml      # Auto-deploy on push
```

**GitHub Actions workflow** (`.github/workflows/hugo.yml`):

```yaml
name: Deploy Hugo
on:
  push:
    branches: [main]
jobs:
  build-deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: peaceiris/actions-hugo@v3
        with:
          hugo-version: 'latest'
      - run: hugo --minify
      - uses: peaceiris/actions-gh-pages@v4
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```

**Writing a post:**

```bash
# Create new post
hugo new writing/my-take-on-something.md

# Edit the markdown file, then push
git add . && git commit -m "New post" && git push
```

**Blog aesthetic guidance**: The `/writing` page should mirror the main site — same monospace type, same section-label style, minimal metadata per post (title, date, maybe one tag). No categories, no sidebar, no pagination until you have 20+ posts. Each post page is just the text with a link back to the listing.

---

## Content to Review Before Publishing

Items in the current `index.html` you should verify/update:

1. **X/Twitter handle**: Currently set to `@mikepsecuritee` — confirm this is current
2. **"Now" section items**: These should reflect what's actually top-of-mind right now. Update quarterly or whenever priorities shift.
3. **Government briefings**: Confirm you're comfortable with GCHQ/NCSC being named publicly. If not, soften to "UK and allied government security organizations."
4. **AI Security SRM**: Decide if this still merits a top-level work item or should be mentioned elsewhere.
5. **PE advisory line in "Now"**: This is a signal to potential clients — make sure the framing matches how you want inbound to arrive.

---

## Optional Enhancements (Low Priority)

These aren't needed for launch but are worth considering later:

- **`/uses` page**: Tools, stack, setup. Terminal-aesthetic sites often have these. Could be a second HTML file linked from footer.
- **Structured data / JSON-LD**: Add schema markup for better search presence as a person/professional.
- **Open Graph meta tags**: For social sharing previews. Add `og:title`, `og:description`, `og:image` to `<head>`.
- **Favicon**: A simple monochrome favicon. Could be a `>_` cursor icon.
- **Analytics**: If you want basic traffic data, add Plausible or Fathom (privacy-respecting, single script tag). Skip Google Analytics.
- **RSS for blog**: Hugo generates this automatically when you add the blog.

---

## File Manifest

| File | Purpose |
|------|---------|
| `index.html` | The entire site — single file, no dependencies |
| `CNAME` | Custom domain config for GitHub Pages |

That's it. Two files to deploy.
