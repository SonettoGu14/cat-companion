# CAT Companion

Bilingual (EN / 中文) companion site for the XJTLU modules **CAT105TC**, **CAT204TC** and
**CAT207TC**. It holds the material a 13-week or 26-week module cannot fit in — the maths
under a shader, the vocabulary of VR, the references that are faster to look up than to
ask about.

The site is built with [MkDocs Material](https://squidfunk.github.io/mkdocs-material/) and
[MkDocs Static i18n](https://ultrabug.github.io/mkdocs-static-i18n/).

## Local preview

```bash
uv sync
uv run mkdocs serve        # http://127.0.0.1:8000
```

## Build

```bash
uv run mkdocs build --strict
```

`--strict` fails on any broken link or unknown nav entry, which is what CI runs.

## Deploy

Pushing to `main` triggers `.github/workflows/deploy.yml`, which builds the site and
publishes it to GitHub Pages via Actions.

Before the first deploy, set **Settings → Pages → Build and deployment → Source** to
**GitHub Actions**.

## Adding a page

Every page is a pair of files, sharing the same stem:

```
docs/foundations/vectors-and-dot-product.en.md
docs/foundations/vectors-and-dot-product.zh.md
```

Then add a line to `nav:` in `mkdocs.yml` — **without** the locale suffix
(`vectors-and-dot-product.md`, not `.en.md`) — and, if the title is new, add it to
`nav_translations` under `zh`.

## Conventions

See [AGENTS.md](AGENTS.md) for the content boundary, the bilingual rules and the
maintenance workflow.
