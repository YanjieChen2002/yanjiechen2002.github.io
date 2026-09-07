# Agent guidelines for this site

This repository is **Yanjie Chen's personal academic homepage**, not the upstream [al-folio](https://github.com/alshedivat/al-folio) starter. Read this file before changing anything.

Live site: https://yanjiechen2002.github.io/  
Stack: Jekyll + `theme: al_folio_core` (al-folio v1.x plugin gems).

## What belongs here

| Change | Location |
| --- | --- |
| Page copy, nav, permalinks | `_pages/` |
| News items | `_news/` |
| Publications | `_bibliography/papers.bib`, `_data/in_preparation.yml`, `_data/venues.yml`, `_data/coauthors.yml` |
| CV data | `_data/cv.yml` |
| Socials, GitHub cards | `_data/socials.yml`, `_data/repositories.yml` |
| Images, PDF | `assets/` |
| Plugin pins and feature flags | `Gemfile` **and** `_config.yml` (both) |
| Local theme override | `_includes/footer.liquid` only, unless the user asks for another override |

Runtime layouts, includes, Sass, Liquid tags, and feature JS live in the gems. Do not add `_layouts/`, `_sass/`, `_scripts/`, or a local Tailwind pipeline unless the user explicitly wants a gem override.

## Site facts that the starter README gets wrong

1. **`baseurl` is empty.** This is a `username.github.io` user site. `bundle exec jekyll build` / `serve` with no extra `--baseurl` is correct. `/al-folio` will break every asset and link.
2. **There is no Projects page.** The `_projects` collection was removed. Do not recreate `_projects/` or `_pages/projects.md`. Course/side work that should be public goes on Repositories (`_data/repositories.yml`) or in the CV YAML.
3. **There is no blog, books, teachings, or plugin catalog.** Do not restore `_posts/`, `_books/`, `_teachings/`, `_pages/plugins.md`, or `_data/featured_plugins.yml`.
4. **CV format is RenderCV YAML**, not the Einstein `resume.json` demo. The downloadable file is `assets/pdf/CV_202609.pdf`. Leave `.github/workflows/render-cv.txt` disabled unless the user asks to generate the PDF in CI.
5. **The footer is intentionally blank.** `_includes/footer.liquid` overrides the gem footer. Delete that file only if the user wants the stock footer back.

## Local commands

From the repo root:

```bash
bundle install
npm ci
npm run lint:prettier
bundle exec jekyll build
bundle exec jekyll serve
```

Docker (this machine already uses it):

```bash
docker compose up -d
curl -fsS http://127.0.0.1:8080/ >/dev/null
```

Dev URLs: `http://localhost:4000/` (Jekyll) or `http://127.0.0.1:8080/` (Docker).

## Features fail silently

A gem feature renders only when the gem is in `Gemfile`, the plugin is listed in `_config.yml`, the flag is on, and the page opts in. Otherwise the Liquid tag emits an empty string.

## Before you finish a content change

- Keep names, dates, and links consistent across `_pages/about.md`, `_news/`, `_data/cv.yml`, and `_bibliography/papers.bib`.
- Publication thumbnails live in `assets/img/publication_preview/` and are referenced by filename in BibTeX `preview` or `in_preparation.yml`.
- Do not commit secrets. `helper/` is gitignored local scratch.
