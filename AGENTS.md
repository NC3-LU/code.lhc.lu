# Repository Guidelines

## Project Structure & Module Organization
- `content/` houses Markdown with TOML front matter; subfolders mirror site navigation and service areas.
- `layouts/` overrides theme templates; keep page partials grouped by section to avoid collisions.
- `assets/` collects SCSS and pipeline assets compiled by Hugo Extended.
- `static/` serves images and downloads verbatim; store heavy media externally and link here.
- `data/` exposes YAML/TOML snippets consumed by shortcodes, ideal for project inventories.
- `themes/` includes `kode` (active) and `hugo-arcana`; treat them as git submodules and avoid direct edits without upstream PRs.

## Build, Test, and Development Commands
- `hugo server -D --disableFastRender` starts a local preview with drafts and reloads assets on each request.
- `hugo --minify --gc` produces the production bundle in `public/`, pruning unused fingerprinted assets.
- `git submodule update --init --recursive` syncs theme sources after cloning or switching branches.

## Coding Style & Naming Conventions
- Author content in Markdown with TOML front matter; use snake_case filenames for multi-word slugs.
- Wrap prose near 100 characters and prefer semantic Markdown (lists, tables) over ad-hoc HTML.
- Keep SCSS indented with two spaces and rely on Hugo Pipes; do not commit compiled CSS.

## Testing Guidelines
- Verify new pages via `hugo server` and watch for console warnings about missing resources or broken links.
- Before pushing, run `hugo --minify` locally and open `public/index.html` to spot layout regressions.
- The repository has no automated unit tests; document manual verification steps in the pull request description.

## Commit & Pull Request Guidelines
- Follow the existing `type: [scope] message` pattern (e.g., `new: [project] Add Range42`) and keep messages imperative.
- Group related changes per commit, separating content edits from theme updates to ease review.
- Pull requests should link related issues, describe the change, list manual checks, and attach screenshots for UI adjustments.

## Security & Configuration Tips
- Global settings live in `config/_default/`; review `params.toml` before introducing site-wide toggles.
- The default `theme_kode` branch deploys via `.github/workflows/hugo.yml`; keep the pinned Hugo Extended (0.108.0) and Dart Sass versions updated together when bumping.
