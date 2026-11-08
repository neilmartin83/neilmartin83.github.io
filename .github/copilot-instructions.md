# Copilot instructions for this repository

This repository is a Jekyll-powered blog for the Sound Mac Guy site. The site is content-driven rather than an application with a test suite: the main work is writing and formatting posts, updating layout/content files, and keeping the generated site buildable on GitHub Pages.

## Build, test, and validation
- Install Ruby dependencies: `bundle install`
- Run a local dev site: `bundle exec jekyll serve` (add `--livereload` if needed)
- Validate a production build: `bundle exec jekyll build`
- GitHub Pages CI is defined in `.github/workflows/jekyll.yml` and runs:
  `bundle exec jekyll build --baseurl "${{ steps.pages.outputs.base_path }}"` with `JEKYLL_ENV=production`
- There is no automated unit test or lint target in this repo. For focused validation of content/layout changes, use `bundle exec jekyll build` after editing; it is the effective “single-check” equivalent.

## High-level architecture
- `Gemfile` and `_config.yml` define the Jekyll site, theme (`minima`), plugins, pagination, and site metadata.
- Content lives primarily in `_posts/` as Markdown posts with YAML front matter; the filename pattern is `YYYY-MM-DD-slug.md`.
- Layout overrides live in `_layouts/` and `index.html`; these customize the home page and archive pages beyond the default `minima` theme.
- Top-level pages such as `about.md`, `archives.md`, and the root `index.html` are part of the site content and often drive navigation or page structure.
- Static assets live under `images/`, `assets/`, and theme/image files at the repo root; generated output is in `_site/` and should not be edited manually.
- `.scripts/` contains migration and maintenance utilities for copying post assets into date-based asset paths and rewriting links; these are repo-specific workflows, not generic app tooling.

## Key conventions
- Prefer simple Markdown with YAML front matter over custom HTML unless a post or template genuinely needs markup beyond standard Jekyll/Liquid usage.
- Writing conventions are content-oriented: posts usually begin with front matter such as `layout: post`, `title:`, and `date:`.
- Keep templates and Liquid logic compatible with the site’s pagination setup in `index.html` and `_config.yml` (`jekyll-paginate`, `paginator.posts`).
- When you add or move images/assets referenced from a post, prefer repo-managed asset paths under `/assets/YYYY/MM/DD/<filename>` and use the helper scripts in `.scripts/` rather than editing generated files.
- Keep customizations in source files (`_layouts/`, `_includes/`, top-level pages, `_posts/`) and avoid editing `_site/` or `.jekyll-cache/`.
- Treat this repo as a GitHub Pages/Jekyll site: changes should remain compatible with the default Jekyll build, `minima` theme, and Pages deployment flow.
- Do not commit secrets or environment-specific tokens into posts, configs, or workflows.

## Editing guidance
- New posts belong in `_posts/` and should use the existing date-based naming and front matter conventions.
- If you change homepage or archive behavior, check both `index.html` and the corresponding layout files in `_layouts/` before editing.
- If a change affects assets or links, review `.scripts/*.py` helpers and keep the links consistent with the date-based asset layout used by the blog.
- Keep the content authoritative in source files; `_site/` is generated output.
