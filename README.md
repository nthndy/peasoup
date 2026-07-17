# peasoup

A GitHub Pages site for the pond. Blog updates, a plant inventory, water
chemistry, a wishlist, and a "sponsor a plant" page that pulls live special
offers from Wetland Plants.

Built with Jekyll, which GitHub Pages compiles for you on every push. Day to
day you only ever edit small YAML files or drop a markdown post. No build step.

## One-time setup

1. Create a repo on GitHub named `peasoup` under your account (`nthndy`), then
   push this folder to it:
   ```bash
   cd peasoup
   git init
   git add .
   git commit -m "Initial pond"
   git branch -M main
   git remote add origin https://github.com/nthndy/peasoup.git
   git push -u origin main
   ```
2. On GitHub: **Settings → Pages**. Under *Build and deployment*, set
   *Source* to **Deploy from a branch**, branch **main**, folder **/ (root)**.
   The site appears at `https://nthndy.github.io/peasoup/`.
3. On GitHub: **Settings → Actions → General → Workflow permissions**, select
   **Read and write permissions**. This lets the price scraper commit updates.
4. Run the scraper once by hand: **Actions → Refresh plant offers → Run
   workflow**. After that it runs every Monday.

If you ever rename the repo or use a custom domain, update `url` and `baseurl`
in `_config.yml`.

## Updating content

Everything below is a plain file edit, then `git commit` and `git push`.

### New blog post
Add a file to `_posts/` named `YYYY-MM-DD-a-short-slug.md`:
```markdown
---
title: "Your title"
date: 2026-08-01 18:00:00 +0100
tags: [water, plants]
excerpt: "One line shown in the blog list."
# cover: /assets/img/your-photo.jpg   # optional header image
---

Write the update here in markdown.
```
Photos: drop them in `assets/img/` and reference them as
`![alt text](/assets/img/your-photo.jpg)`.

### Plants (`_data/plants.yml`)
Duplicate a block to add a new plant. Each plant carries its history as
`checks`, a list of dated health checks, newest first — the first entry is
the plant's current state. Add a new block at the **top** of a plant's
`checks:` each time you look at it. `status` is one of `planted`, `thriving`,
`steady`, `struggling`, `lost`. `zone` is one of `marginal`, `oxygenator`,
`floating`, `deep-water`, `bog`. `note` and `photos` (paths under
`/assets/img/`) on a check are both optional — leave them out rather than
leaving them blank.

### Water chemistry (`_data/water.yml`)
Add a new block at the **top** of `log:` each time you test. Colours are worked
out automatically from the `targets:` safe/warn bands, so you never set them by
hand; `range` sets the axis scale for the charts on the water page. `photos` on
a log entry is optional.

### Wishlist (`_data/wishlist.yml`)
Two lists, `plants` and `materials`. Set `got: true` once you have something and
it shows struck through. `priority` is `high`, `medium`, or `low`.

### Sponsor / offers
The offers grid updates itself from the scraper. The Monzo handle lives in
`_config.yml` as `monzo:`.

## How the offers scraper works

`.github/workflows/scrape-offers.yml` runs `scripts/scrape_offers.py` weekly.
It reads the supplier's special-offers page, writes `_data/offers.yml`, and
commits it. The site rebuilds automatically. If the scrape fails, it keeps the
last good list and records an error the Sponsor page shows quietly.

To change frequency, edit the `cron` line in the workflow.

## Local preview (optional)

You do not need this to publish, but to preview before pushing:
```bash
mamba create -n peasoup ruby -y
mamba activate peasoup
bundle install
bundle exec jekyll serve
```
Then open `http://localhost:4000/peasoup/`.
