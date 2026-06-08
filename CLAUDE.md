# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single-page personal résumé/CV site for Michelangelo Riccobene, served at `mriccobene.github.io`. It is a Jekyll site built on the third-party remote theme [`sproogen/resume-theme`](https://github.com/sproogen/resume-theme) (the "modern-resume-theme") and deployed automatically by GitHub Pages on push to `master`.

## Commands

```bash
bundle install        # install gems (also: install.bat)
bundle exec jekyll serve   # local dev server with live reload (also: run.bat)
```

Output builds to `_site/` (gitignored). There are no tests, linters, or a build step beyond Jekyll itself — GitHub Pages runs `jekyll build` on the server when you push.

## Architecture — where the content lives

This is the one thing that is not obvious and matters most: **essentially the entire site content lives in `_config.yml`, not in markdown/HTML files.** The theme reads structured data from the `content:` key in `_config.yml` to render every résumé section (Projects, Experience, Education, etc.). There is no `_posts/`, no `_layouts/`, no `_includes/` in this repo — all layouts and includes come from the remote theme.

Consequences when editing:

- **To change résumé text, sections, projects, or jobs → edit `_config.yml`.** Each entry under `content:` is a section with a `layout` (`list`/`text`) and nested `content` items (`layout: left`/`top-middle`, with `title`/`link`/`quote`/`description` fields). `description` blocks use Markdown and the theme's `<mark>` highlighting convention.
- `_config.yml` also holds personal info, social links, the about section, and theme settings (`darkmode`, `remote_theme`, SEO plugin).
- **Changing `_config.yml` requires restarting `jekyll serve`** — config changes are not picked up by live reload.
- `index.md` is an almost-empty stub (just `layout: default` front matter); the theme's home layout renders the config-driven content.
- `assets/main.scss` only imports the theme's stylesheet (`@import 'modern-resume-theme';`) — add custom CSS overrides here, after the import.
- Images live in `images/` and are referenced from `_config.yml` (e.g. `about_profile_image`, `favicon`).

To customize layout/styling beyond config, you must override theme files locally by recreating the corresponding path (e.g. `_layouts/home.html`) — the local copy shadows the remote theme's version.
