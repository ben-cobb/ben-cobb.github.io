# AGENTS.md - Project Guide

## Overview
Personal academic website for Benjamin M. Cobb. Built with Jekyll using the **al-folio** theme. Hosted on GitHub Pages at `ben-cobb.github.io`.

## Git Branches
- `test` - Canonical source **and** the auto-deploy branch. Pushing here triggers GitHub Actions (`.github/workflows/deploy.yml`) to build and publish the site to www.ben-cobb.com.
- Legacy/unused: `source`, `main`, `jekyll_source` (older snapshots — do not use).

## Development & Deployment
Deployment is automated via GitHub Actions — just edit source files and push to `test`:
```bash
git add .
git commit -m "description of changes"
git push origin test     # GitHub Actions builds + deploys -> live in ~3-4 min
```
No manual build or `docs/` copying is needed. To preview locally before pushing (optional):
```bash
bundle exec jekyll serve   # serves at http://localhost:4000
```
**Deployment details:** GitHub Pages "Source" is set to **GitHub Actions**. The deploy workflow installs gems serially (avoids a Bundler parallel-installer deadlock), builds with `JEKYLL_ENV=production`, and publishes `_site` via `actions/deploy-pages`. The custom domain (www.ben-cobb.com) is preserved by the root `CNAME` file. The build output is no longer committed — do not re-create a `docs/` folder.

## Directory Structure
```
_bibliography/    # papers.bib (publications)
_books/           # Book review markdown files
_data/            # YAML data (coauthors, venues, cv, socials, repositories)
_includes/        # Reusable Liquid template partials
_layouts/         # Page layout templates (.liquid)
_pages/           # Site pages (about, publications, cv, books, teaching, profiles)
_plugins/         # Custom Jekyll plugins
_sass/            # Sass stylesheets
_site/            # Generated output (do not edit)
assets/
  img/            # Images (auto-generates responsive WebP at 480/800/1400px)
  pdf/            # Publication PDFs and resume
  json/           # resume.json
  jupyter/        # Rendered Jupyter notebooks
```

## Adding a Publication

1. Place the PDF in `assets/pdf/`
2. Add a BibTeX entry to `_bibliography/papers.bib` (newest first)

### BibTeX Entry Template
```bibtex
@INPROCEEDINGS{uniqueID,
  author={Last, First and Last2, First2},
  booktitle={Conference Name},
  title={Paper Title},
  year={2025},
  pages={},
  keywords={keyword1;keyword2},
  doi={10.xxxx/xxxxx},
  abstract={Full abstract text.},
  selected={true},
  bibtex_show={true},
  pdf={filename.pdf},
  code={https://github.com/...}
}
```

### Key BibTeX Fields
| Field | Effect |
|-------|--------|
| `abstract` | Enables ABS button (collapsible abstract) |
| `doi` | Enables DOI link button |
| `bibtex_show={true}` | Enables BIB button (collapsible citation) |
| `pdf={filename.pdf}` | Enables PDF button (file from `assets/pdf/`) |
| `code={url}` | Enables CODE button |
| `selected={true}` | Shows on homepage "selected publications" |
| `video={youtube_embed_url}` | Enables VIDEO button |
| `slides={url}` | Enables SLIDES button |
| `poster={url}` | Enables POSTER button |
| `website={url}` | Enables WEBSITE button |
| `arxiv={id}` | Enables arXiv link |
| `award={text}` | Shows award badge |
| `preview={image}` | Thumbnail image |

## Adding a Book Review

Create a markdown file in `_books/` with this front matter:
```yaml
---
layout: book-review
title: Book Title
author: Author Name
cover: assets/img/book_covers/cover.jpg
categories: genre1 genre2
tags: tag1
buy_link: https://...
started: YYYY-MM-DD
finished: YYYY-MM-DD
released: YYYY
stars: 5
goodreads_review: ID
status: Finished
---
Review content here.
```

## Key Data Files
- `_data/coauthors.yml` - Author name variants and profile URLs (for linking in publications)
- `_data/venues.yml` - Conference/journal abbreviations, URLs, and badge colors
- `_data/cv.yml` - Structured CV data
- `_data/repositories.yml` - GitHub repos shown on the site

## Page Layouts
- `about` - Homepage with profile, selected papers, news
- `page` - Generic content page
- `cv` - CV/resume page
- `bib` - Individual publication rendering (used by jekyll-scholar)
- `book-review` - Single book review
- `book-shelf` - Book collection grid
- `post` - Blog post
- `distill` - Scientific article format

## Key Config (_config.yml)
- Scholar plugin processes `_bibliography/papers.bib`
- ImageMagick auto-generates responsive images from `assets/img/`
- Homepage selected papers: controlled by `selected_papers: true` in `_pages/about.md`
- Publications page at `/publications/` renders all entries via `{% bibliography %}`
