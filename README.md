# ktzyskowski.github.io

Personal site built with [Jekyll](https://jekyllrb.com/) and the [Minima](https://github.com/jekyll/minima) theme, deployed via GitHub Pages.

## Structure

- `index.markdown` — landing page at `/`
- `blog.md` — post listing at `/blog/` (uses minima's `home` layout)
- `about.markdown` — about page at `/about/`
- `_posts/` — blog posts (filename format: `YYYY-MM-DD-title.markdown`)
- `_config.yml` — site config
- `_layouts/`, `_includes/`, `_sass/` — theme overrides

Minima auto-populates the site nav from any page with a `title` in its front matter.

## Local development

Requires Ruby (tested with 3.3.5) and Bundler.

```bash
bundle install
bundle exec jekyll serve
```

Site served locally at <http://localhost:4000>.

## Adding a blog post

Create a file in `_posts/` named `YYYY-MM-DD-your-title.markdown` with front matter:

```yaml
---
layout: post
title: "Your Title"
date: YYYY-MM-DD HH:MM:SS -0500
---
```

### Drafting a blog post

Create a file in `_drafts/` and once you're ready, follow the steps above to make it a published post.

## Troubleshooting

**Bundler version mismatch:** If bundler complains the lockfile was generated with a different version, delete `Gemfile.lock` and re-run `bundle install`.
