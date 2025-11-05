# Repository Guidelines

Use this guide when contributing to Code @ LHC's Hugo site to keep content consistent and builds reliable.

## Project Structure & Module Organization
- `content/` houses Markdown with TOML front matter; subfolders mirror site navigation and service areas. Prefer Hugo shortcodes (e.g. `readme`) over raw HTML embeds.
- `layouts/` and `layouts/shortcodes/` override theme templates; keep page partials grouped by section to avoid collisions.
- `assets/sass/` contains the SCSS processed by Hugo Extended; rely on the Hugo Pipes pipeline instead of committing compiled CSS.
- `static/` serves images and downloads verbatim; store heavy media externally and link here only when needed.
- `data/` exposes YAML/TOML snippets consumed by shortcodes, ideal for project inventories and shared content blocks.
- `themes/kode` (active) and `themes/hugo-arcana` are git submodules; sync them before editing and avoid direct changes without upstream pull requests.
- `public/` is generated build output; never edit it manually.

## Build, Test, and Development Commands
- `git submodule update --init --recursive` ensures theme sources are available after cloning or switching branches.
- `hugo server -D --disableFastRender` runs a local preview with drafts enabled and reloads assets on each request (http://localhost:1313).
- `hugo --cleanDestinationDir --gc --minify` mirrors the CI build, prunes unused assets, and writes fresh output to `public/`; run before pushing.
- `hugo --panicOnWarning` catches missing resources, shortcode errors, or front matter issues during review.
- `npm ci` (optional) installs theme tooling when a `package-lock.json` is introduced or updated.

## Coding Style & Naming Conventions
- Follow `.editorconfig`: 4-space indentation by default, 2 spaces for HTML/CSS/JS/YAML, tabs only in `Makefile`; always keep UTF-8 with LF endings and a trailing newline.
- Author content in Markdown with TOML front matter; wrap prose near 100 characters and prefer semantic Markdown (lists, tables) over ad-hoc HTML.
- Name new content files in `kebab-case.md`; include standard fields like `title`, `author`, `description`, `repository`, `weight`, and `draft`. Legacy files may differ—match nearby conventions when touching existing content.
- Keep SCSS indented with two spaces and organized by component; do not commit generated CSS.
- Place reusable snippets in `layouts/shortcodes/` instead of embedding raw HTML directly in content files.

## Testing Guidelines
- Toggle `draft = true` while iterating on new pages, flipping to `false` only when ready to publish.
- Verify new or updated pages via `hugo server` and watch the terminal/browser console for warnings about missing resources or broken links.
- Run `hugo --cleanDestinationDir --gc --minify` locally and spot-check `public/index.html` (or affected pages) for layout regressions.
- Use `hugo --panicOnWarning` to fail fast on front matter or shortcode issues.
- After Sass changes, confirm compiled styles in the browser and resolve any console warnings.
- The repository has no automated unit tests; document manual verification steps in pull requests.

## Commit & Pull Request Guidelines
- Follow the existing `type: [scope] message` pattern (e.g. `new: [project] Add Range42`) using imperative verbs.
- Group related changes per commit, separating content edits from theme updates to ease review.
- Pull requests should link related issues, describe the change, list manual checks (commands run), and attach screenshots or GIFs for UI adjustments.
- Confirm `hugo --cleanDestinationDir --minify` (and other relevant checks) passes before requesting review.

## Security & Configuration Tips
- Global settings live in `config/_default/`; review `params.toml` before introducing site-wide toggles.
- The default `theme_kode` branch deploys via `.github/workflows/hugo.yml`; keep the pinned Hugo Extended (0.108.0) and Dart Sass versions updated together when bumping.
