# Yanjie Chen

Personal academic homepage: [yanjiechen2002.github.io](https://yanjiechen2002.github.io/).

I am a Research Staff Associate at Columbia's Irving Institute for Cancer Dynamics. This site is a customized [al-folio](https://github.com/alshedivat/al-folio) v1.x Jekyll site. Runtime layouts and features come from the `al_folio_*` plugin gems; this repository holds content, configuration, and a few local overrides.

## Pages

| Path | Source |
| --- | --- |
| Home | [`_pages/about.md`](_pages/about.md) |
| Publications | [`_pages/publications.md`](_pages/publications.md), [`_bibliography/papers.bib`](_bibliography/papers.bib), [`_data/in_preparation.yml`](_data/in_preparation.yml) |
| Repositories | [`_pages/repositories.md`](_pages/repositories.md), [`_data/repositories.yml`](_data/repositories.yml) |
| CV | [`_pages/cv.md`](_pages/cv.md), [`_data/cv.yml`](_data/cv.yml), PDF at [`assets/pdf/CV_202609.pdf`](assets/pdf/CV_202609.pdf) |
| News | [`_news/`](_news/), listed on the home page |

The Projects collection is retired. GitHub work is listed on Repositories instead.

## Local preview

This is a user site (`username.github.io`), so `baseurl` in [`_config.yml`](_config.yml) is empty. Do not build with `--baseurl /al-folio`.

With Ruby and Bundler:

```bash
bundle install
bundle exec jekyll serve
```

Then open [http://localhost:4000/](http://localhost:4000/).

With Docker:

```bash
docker compose up -d
```

Then open [http://127.0.0.1:8080/](http://127.0.0.1:8080/).

Pushing to `main` deploys via [`.github/workflows/deploy.yml`](.github/workflows/deploy.yml).

## Editing content

- **About and news:** `_pages/about.md` and `_news/*.md`
- **Publications:** add BibTeX to `_bibliography/papers.bib`; drafts go in `_data/in_preparation.yml`; previews in `assets/img/publication_preview/`
- **CV webpage:** `_data/cv.yml` (`cv_format: rendercv` on the CV page). The downloadable PDF is maintained by hand; the RenderCV GitHub Action is disabled (`.github/workflows/render-cv.txt`)
- **Social links:** `_data/socials.yml`
- **Footer:** [`_includes/footer.liquid`](_includes/footer.liquid) is an empty local override that hides the theme footer

If you add or remove an `al-folio` plugin, update both `Gemfile` and `_config.yml`.

## License

Site content is mine. The underlying al-folio starter is MIT-licensed; see [LICENSE](LICENSE).
