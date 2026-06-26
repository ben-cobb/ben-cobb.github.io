# Website Maintenance Guide

A practical guide to updating and maintaining **www.ben-cobb.com** — the personal
academic site for Benjamin M. Cobb, built with the [al-folio](https://github.com/alshedivat/al-folio)
Jekyll theme and hosted on GitHub Pages.

> **TL;DR for everyday edits:** make your change on the `main` branch, then
> `git add . && git commit -m "..." && git push origin main`. GitHub Actions
> builds and publishes the site to www.ben-cobb.com in ~3–4 minutes. That's it —
> no local build, no copying files around.

---

## 1. How the site is built and deployed

```
edit source on `main`  ──push──►  GitHub Actions (.github/workflows/deploy.yml)
                                         │  build with Jekyll (production)
                                         ▼
                                   GitHub Pages  ──►  https://www.ben-cobb.com
```

- **One branch — `main`** — is both where you edit and what gets deployed. It is
  the repository's default branch. There are no other working/deploy branches.
- On every push to `main`, **`.github/workflows/deploy.yml`** runs: it installs
  Ruby gems + ImageMagick + nbconvert, runs `jekyll build` with
  `JEKYLL_ENV=production`, purges unused CSS, and publishes the generated `_site/`
  to GitHub Pages via `actions/deploy-pages`.
- **GitHub Pages settings:** Source = **"GitHub Actions"**; the `github-pages`
  environment has deployment branches set to **No restriction**; the custom
  domain **www.ben-cobb.com** is preserved by the root **`CNAME`** file; **Enforce
  HTTPS** is on.
- **Nothing is hand-built.** The old model (build locally → `cp -r _site/ docs/` →
  commit the built output) is gone. **Do not re-create a `docs/` folder** — it is
  git-ignored and excluded from the build on purpose.

### Watching a deploy
Go to the repo's **Actions** tab → **"Deploy site"** → click the latest run. A run
has two jobs: **build** (compiles the site) and **deploy** (publishes it). Both
should be green. The run takes ~3–4 minutes.

---

## 2. The everyday workflow

```bash
# 1. (first time only) clone the repo
git clone https://github.com/ben-cobb/ben-cobb.github.io.git
cd ben-cobb.github.io

# 2. make sure you're on main and up to date
git checkout main
git pull origin main

# 3. edit files (see Section 3 for what to edit)

# 4. publish
git add .
git commit -m "describe your change"
git push origin main          # → auto-builds & deploys in ~3–4 min
```

### Preview before publishing (optional but recommended for big changes)
- **Locally:** `bundle exec jekyll serve`, then open <http://localhost:4000>.
  (Requires Ruby + `bundle install` once. Note: local `serve` runs in *development*
  mode, which skips CSS/JS minification — see Troubleshooting if something works
  locally but fails in CI.)
- **Via a pull request:** push your change to a short-lived branch and open a PR
  into `main`. The workflow runs the **build** job as a check (no deploy) so you
  can confirm it compiles before merging. You don't need permanent dev branches —
  feature-branch PRs are the way to stage risky changes.

---

## 3. Common content updates

All paths below are relative to the repo root. Edit on `main` and push.

### 3.1 Add a publication
1. Drop the PDF into **`assets/pdf/`** (e.g. `My_Paper.pdf`).
2. Add a BibTeX entry to the **top** of **`_bibliography/papers.bib`** (newest
   first):
   ```bibtex
   @INPROCEEDINGS{uniqueKey2025,
     author={Cobb, Benjamin and Coauthor, Name},
     booktitle={Conference Name},
     title={Paper Title},
     year={2025},
     doi={10.xxxx/xxxxx},
     abstract={Full abstract text.},
     selected={true},        % shows on the homepage "selected publications"
     bibtex_show={true},     % adds the "BIB" button
     pdf={My_Paper.pdf},     % filename in assets/pdf/
     code={https://github.com/...}
   }
   ```
   Useful optional fields: `arxiv={id}`, `slides={url}`, `video={url}`,
   `poster={url}`, `website={url}`, `award={text}`, `preview={image_in_assets_img}`.
   The publications page is generated automatically by `jekyll-scholar` — you do
   **not** edit `_pages/publications.md`.

### 3.2 Edit an existing page
Pages live in **`_pages/*.md`** (e.g. `about.md`, `cv.md`, `teaching.md`,
`books.md`, `profiles.md`). Edit the Markdown/HTML below the `---` front-matter
block. The homepage bio is in **`_pages/about.md`**.

### 3.3 Add a NEW page (and put it in the top nav)
Create **`_pages/<name>.md`** with front matter like this:
```markdown
---
layout: page
title: research            # appears as the nav label and page title
permalink: /research/      # the URL: www.ben-cobb.com/research/
nav: true                  # show it in the top navigation bar
nav_order: 3               # position in the nav (lower = further left)
description: Optional subtitle shown under the page title.
---

Your content here, in **Markdown** or HTML.
```
- `nav: true` is what puts it in the menu; `nav_order` controls position. Existing
  pages use orders like `publications: 2`, `cv: 5`, so pick an unused number.
- For a page that should **not** appear in the nav (e.g. a standalone URL), omit
  `nav: true`.
- Available `layout:` values: `page` (generic), `about` (homepage style), `cv`,
  `book-shelf`, `bib`, `post`, `distill` (scientific article). Use `page` for most
  new pages.

### 3.4 Update the CV
Two parts, in **`_pages/cv.md`**:
- The **"Download PDF" button** points at `cv_pdf: ben_cobb_resume.pdf` (a file in
  `assets/pdf/`). To update the PDF, replace that file (keep the name, or update
  the `cv_pdf:` value).
- The **on-page CV content** is driven by structured data in **`_data/cv.yml`**
  (education, experience, skills, etc.). Edit that YAML to change the rendered CV.

### 3.5 Add a book review
Create **`_books/<slug>.md`**:
```markdown
---
layout: book-review
title: Book Title
author: Author Name
cover: assets/img/book_covers/cover.jpg   # or use `olid:`/`isbn:` to auto-fetch
categories: genre1 genre2
tags: tag1
buy_link: https://...
started: 2025-01-01
finished: 2025-01-20
released: 2024
stars: 5
goodreads_review: 1234567890
status: Finished
---

Your review text here.
```
Put cover images in **`assets/img/book_covers/`**. The bookshelf grid at
`/books/` updates automatically.

### 3.6 Add news / announcements (optional — not currently enabled)
The theme supports a news feed, but it isn't turned on right now. To enable it:
1. Create a **`_news/`** folder with short Markdown files, e.g.
   `_news/2025-01-01-announcement.md`:
   ```markdown
   ---
   layout: post
   date: 2025-01-01 10:00:00-0400
   inline: true
   ---
   Your one-line announcement, with **Markdown** and [links](https://...).
   ```
2. In **`_pages/about.md`**, un-comment the `announcements:` block in the front
   matter (`enabled: true`).

### 3.7 Homepage tweaks (`_pages/about.md`)
- **Bio text:** edit the Markdown body.
- **Subtitle/affiliation:** the `subtitle:` front-matter field.
- **Profile photo:** replace **`assets/img/prof_pic.jpg`**.
- **Selected publications list:** controlled by `selected_papers: true`; a paper
  appears here when its BibTeX entry has `selected={true}`.

### 3.8 Images and other assets
- Images: **`assets/img/`**. The build auto-generates responsive WebP versions at
  480/800/1400px widths (via ImageMagick), so just add a normal `.jpg`/`.png`.
- PDFs: **`assets/pdf/`**.
- Profile/social links and other structured data live in **`_data/`**
  (`socials.yml`, `coauthors.yml`, `venues.yml`, `repositories.yml`, `cv.yml`).

---

## 4. Troubleshooting

**Where to look first:** repo **Actions** tab → **"Deploy site"** → the failed run
→ open the red job → expand the failing step to read the error.

**If something renders locally but the deploy fails:** local `jekyll serve` runs in
*development* mode (minification off). The deploy runs in *production*
(`JEKYLL_ENV=production`), which minifies HTML/CSS/JS and is stricter. To reproduce
locally: `JEKYLL_ENV=production bundle exec jekyll build`.

**Known issues that were fixed (and could recur):**
- **Never commit a `docs/` folder.** It's git-ignored and excluded from the build.
  Re-introducing it causes Jekyll to ingest its own output recursively and can
  break the build.
- **`Gemfile.lock` must stay Linux-resolved.** The committed lock was generated on
  the CI runner (Linux). If you run `bundle update`/`bundle install` on macOS and
  commit the resulting lock, the deploy can fail (the precompiled Linux gems get
  treated as "yanked," forcing a broken native build). If you must update gems,
  regenerate the lock on Linux — e.g. let CI produce it, or run inside the repo's
  Docker/devcontainer — and commit that.
- **Deploy job "skipped" or "not allowed":** the deploy only runs for pushes to
  `main` and manual **Run workflow** (not PRs). If it fails instantly at
  "environment setup," check **Settings → Environments → `github-pages` →
  Deployment branches** is set to allow `main` (currently "No restriction").
- **Gem install hangs/deadlocks in CI:** the workflow installs gems serially
  (`bundle install --jobs 1`) on purpose to avoid a Bundler parallel-installer
  deadlock with this Gemfile. Keep that flag.

**Manually trigger a deploy** (without a content change): Actions → "Deploy site" →
**Run workflow** → branch `main`. (Or re-run a previous run.)

---

## 5. What changed in the 2026 migration (summary)

The site was migrated from a fragile manual process to automated CI deployment:

| Before | After |
|--------|-------|
| Build locally, `cp -r _site/ docs/`, commit the built output | Push source to `main`; GitHub Actions builds + deploys |
| Pages served from `test` branch `/docs` folder | Pages Source = "GitHub Actions" |
| Confusing branch sprawl (`source`, `master`, `main`, `test`, `jekyll_source`) | Single branch: **`main`** (default) |
| Built output (`docs/`) committed; recursively nested 2,400+ stale files | No built output in git; `docs/` removed and ignored |
| `Gemfile.lock` generated on macOS (broke CI) | Linux-resolved `Gemfile.lock` committed |
| `url` was `https://ben-cobb.github.io` (wrong) | `url: https://www.ben-cobb.com`; custom domain via root `CNAME` |

---

## 6. Key files & directories

| Path | Purpose |
|------|---------|
| `.github/workflows/deploy.yml` | The build + deploy pipeline (push to `main` → live) |
| `_config.yml` | Jekyll/site configuration (`url`, plugins, excludes, etc.) |
| `CNAME` | Custom domain (`www.ben-cobb.com`); published into `_site` |
| `Gemfile` / `Gemfile.lock` | Ruby gem dependencies (lock is Linux-resolved — see §4) |
| `_pages/` | Site pages (about/home, cv, publications, teaching, books, …) |
| `_bibliography/papers.bib` | Publications (BibTeX, newest first) |
| `_books/` | Book reviews |
| `_data/` | Structured data: `cv.yml`, `coauthors.yml`, `venues.yml`, `socials.yml`, `repositories.yml` |
| `assets/img/`, `assets/pdf/` | Images (auto-responsive) and PDFs |
| `_layouts/`, `_includes/`, `_sass/`, `_plugins/` | Theme internals (rarely edited) |
| `CLAUDE.md` / `AGENTS.md` | Short orientation notes for AI coding assistants |

For deeper theme customization, see the upstream al-folio docs (`README.md`,
`CUSTOMIZE.md`, `FAQ.md` in this repo, and <https://github.com/alshedivat/al-folio>).
