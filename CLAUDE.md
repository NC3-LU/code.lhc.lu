# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**Code @ LHC** is a Hugo-based static site serving as the official open source software catalog for the Luxembourg House of Cybersecurity (LHC), showcasing cybersecurity projects from CIRCL and NC3-LU. Deployed to GitHub Pages from the `theme_kode` branch.

## Commands

```bash
# Local development (port 1313, includes drafts, auto-reload)
hugo server -D --disableFastRender

# Production build (output to public/)
hugo --minify --gc

# After cloning or switching branches
git submodule update --init --recursive
```

There are no automated tests — verify changes locally via `hugo server`, checking the console for warnings about missing resources or broken links.

## Architecture

**Stack:** Hugo v0.108.0, `kode` theme (git submodule), Dart Sass Embedded for SCSS compilation via Hugo Pipes.

**Content model:**
- `content/projects/*.md` — One file per project with TOML front matter: `title`, `author` (references `data/authors/*.toml`), `description`, `logo`, `weight` (sort order), `repository` (GitHub org/repo path). Body typically uses `{{< readme >}}` shortcode.
- `layouts/shortcodes/readme.html` — Fetches each project's `README.md` from GitHub at **build time** using Hugo's `resources.GetRemote`. Requires the project's default branch to be `master`.
- `data/homepage.yml` — Drives homepage sections (banner, highlights, CTA).
- `data/authors/` — Author metadata (CIRCL.toml, NC3.toml) referenced by project front matter.
- `config/_default/menus.toml` — Navigation config.
- `assets/sass/custom.scss` — Site-specific SCSS compiled by Hugo Pipes.
- `static/img/` — Project logos referenced by filename in project front matter.

**Theme customization:** Override theme templates in the root `layouts/` directory. Do not edit `themes/kode/` directly — it is a git submodule.

**Deployment:** GitHub Actions (`.github/workflows/hugo.yml`) triggers on push to `theme_kode` branch, builds with `HUGO_ENVIRONMENT=production`, and deploys `public/` to GitHub Pages.

## Conventions

**Commit messages:** `type: [scope] message`
- Examples: `new: [project] Add Range42`, `fix: [cicd] Fixed out of date actions`, `new: [pic] Added scandale logo`
- Common types: `new`, `fix`, `upd`, `del`

**Files:** snake_case filenames, Markdown with TOML front matter, ~100 char line wrap, SCSS with 2-space indent.

**Adding a project:** Create `content/projects/<name>.md` with TOML front matter, add logo to `static/img/`, ensure the GitHub repo's default branch is `master` for the readme shortcode to work.

**Branch targeting:** All work destined for production must merge to `theme_kode` (not `main`/`master`).
