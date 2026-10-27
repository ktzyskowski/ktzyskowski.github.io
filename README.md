# ktzyskowski.github.io

Personal site at <https://www.ktzyskowski.com>, built with [Jekyll](https://jekyllrb.com/) and the [al-folio](https://github.com/alshedivat/al-folio) theme. Theme documentation lives in [`docs/`](docs/).

## Structure

- `_pages/about.md` — landing page at `/` (bio, photo, latest posts)
- `_pages/blog.md` — post listing at `/blog/`
- `_pages/publications.md` — rendered from `_bibliography/papers.bib`
- `_pages/cv.md` — rendered from `_data/cv.yml`
- `_posts/` — blog posts (`YYYY-MM-DD-title.md`)
- `_drafts/` — unpublished posts
- `_data/socials.yml` — email, GitHub, LinkedIn, etc.
- `_config.yml` — site config

## Local development

Requires [Docker](https://docs.docker.com/get-docker/) (on WSL, Docker Desktop with WSL integration enabled).

```bash
docker compose pull   # first time only, ~400MB
docker compose up
```

Site served at <http://localhost:8080> and rebuilds on save. Changes to `_config.yml` need a restart: `docker compose restart`.

## Adding a blog post

Create `_posts/YYYY-MM-DD-your-title.md`:

```yaml
---
layout: post
title: Your Title
date: YYYY-MM-DD HH:MM:SS-0500
description: one-line summary
tags: ml
---
```

Math (`$$ ... $$`), code blocks, and Jupyter notebooks are supported; see `docs/CUSTOMIZE.md`.

## Adding a publication

Append a BibTeX entry to `_bibliography/papers.bib` (below the `---` lines). al-folio adds optional fields such as `abbr`, `selected={true}` (shows on the about page when `selected_papers: true`), `pdf`, `code`, `arxiv`, and `bibtex_show={true}`.

Note: `%` lines are not treated as comments by the BibTeX parser, so don't leave commented-out entries in the file.

## Deployment

Pushing to `main` runs `.github/workflows/deploy.yml`, which builds the site and pushes the output to the `gh-pages` branch. In the repo's **Settings → Pages**, the source must be set to **Deploy from a branch → `gh-pages` / root**.
