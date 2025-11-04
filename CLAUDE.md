# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Code @ LHC is a Hugo-based static site showcasing open source cybersecurity projects from the Luxembourg House of Cybersecurity (LHC), including projects from CIRCL and NC3. The site is deployed automatically to GitHub Pages when pushing to the `theme_kode` branch.

## Essential Commands

### Setup
```bash
# Clone with theme submodules
git submodule update --init --recursive

# Install optional Node.js dependencies (if needed by theme)
npm ci
```

### Development
```bash
# Run local dev server with drafts enabled
hugo server -D --disableFastRender

# Access at http://localhost:1313
```

### Build & Testing
```bash
# Production build (matches CI pipeline)
hugo --cleanDestinationDir --gc --minify

# Build with strict error checking
hugo --panicOnWarning
```

## Architecture

### Content Management
- **Project pages**: Each project is a Markdown file in `content/projects/` with TOML front matter
- **Dynamic README fetching**: The `{{< readme >}}` shortcode (`layouts/shortcodes/readme.html`) fetches and renders the project's README.md directly from GitHub using the `repository` front matter field
- **Homepage configuration**: `data/homepage.yml` controls the homepage layout and content sections

### Required Front Matter for Projects
```toml
+++
title = "Project Name"
author = "CIRCL" # or "NC3"
description = "Brief description"
logo = "logo-project.png"  # Must exist in static/img/
weight = 10  # Controls display order
draft = false
repository = "org/repo"  # GitHub repo path for {{< readme >}} shortcode
tags = ["projects", "tag1", "tag2"]
+++
```

### Theme System
- Primary theme: `kode` (located in `themes/kode/`)
- Themes are **git submodules** - always sync before editing theme assets
- Override theme templates by placing files in root `layouts/` directory
- Custom shortcodes: `layouts/shortcodes/` (readme.html, osm.html, rawhtml.html)

### Static Assets
- `static/img/` - Project logos and images (served as-is)
- `static/images/` - Banner and site images
- `assets/sass/` - Source styles (processed by Hugo's asset pipeline)
- `public/` - Generated build output (never edit manually, excluded from git)

### Configuration
- `config.toml` - Main Hugo configuration
  - Base URL: https://code.lhc.lu
  - Theme: kode
  - Hugo version used in CI: 0.108.0 (specified in `.github/workflows/hugo.yml`)
  - Social links and menu configuration

## Code Style (from .editorconfig)

- **Default**: 4-space indentation
- **HTML/CSS/JS/YAML**: 2-space indentation
- **Makefiles**: Tab indentation
- **Encoding**: UTF-8 with LF line endings
- **Markdown**: May retain intentional double trailing spaces (for line breaks)
- Always include trailing newline

## Commit Message Format

Follow existing pattern: `type: [scope] message`

Examples:
- `new: [project] Added Range42`
- `fix: [md] Fixing content regression`
- `new: [doc] Added LICENCE and README`
- `new: [pic] Added scandale logo`

Use imperative verbs and keep unrelated changes in separate commits.

## Git Workflow

- **Main branch**: `theme_kode` (this is the default deployment branch)
- Development branches are merged via pull requests
- GitHub Actions automatically deploys to Pages on push to `theme_kode`
- Always run `hugo --cleanDestinationDir --minify` before submitting PR

## Adding a New Project

1. Create `content/projects/project-name.md` with required front matter
2. Add project logo to `static/img/logo-project-name.png`
3. Ensure the `repository` field points to the correct GitHub repo (org/repo format)
4. Set `draft = false` when ready to publish
5. The `{{< readme >}}` shortcode will automatically fetch and display the project's README from GitHub

## Testing Checklist

- Use `draft = true` while developing new content
- Run `hugo server -D` to preview drafts locally
- Run `hugo --panicOnWarning` to catch errors (broken front matter, missing assets, shortcode errors)
- Verify project README renders correctly via the `{{< readme >}}` shortcode
- Check for browser console warnings after Sass changes
- Ensure all project logos exist in `static/img/`
