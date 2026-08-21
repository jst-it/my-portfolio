# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A personal portfolio/blog site (Joe Stebbings) built with [Hugo](https://gohugo.io) using the [PaperMod](https://github.com/adityatelange/hugo-PaperMod) theme, deployed to Cloudflare (via `wrangler`).

## Commands

- `hugo server -D` — run the local dev server with drafts included (live reload, default at http://localhost:1313)
- `hugo` — build the production site into `public/`
- `hugo --gc --minify` — production build as used by the deploy pipeline (see `wrangler.jsonc`)
- `hugo new posts/my-post.md` — create a new post from `archetypes/default.md`
- `npx wrangler deploy` — deploy `public/` to Cloudflare (runs the build command from `wrangler.jsonc` first)

There is no package.json, linter, or test suite in this repo — it's Hugo + theme only.

## Architecture

- **Content** lives in `content/` as Markdown/TOML front matter (`.md` files use `+++` TOML front matter, e.g. `content/posts/hello-world.md`; `content/_index.md` is the home page body). Blog posts go under `content/posts/`.
- **Theme** is the `PaperMod` git submodule at `themes/PaperMod` (do not edit directly; it's pinned via `.gitmodules`/submodule commit). Site-specific overrides would go in `layouts/`, `assets/`, `static/`, `i18n/`, or `data/` at the repo root (currently all empty — no overrides yet), which Hugo merges over the theme.
- **Site config** is `hugo.toml` — this is the single source of truth for site params (title, description, PaperMod-specific `params.*` flags like `ShowReadingTime`) and the main menu (`[[menu.main]]` entries).
- **`public/`** is the built site output and **is committed to git** (no `.gitignore` exists in this repo). When content or theme changes, regenerate it with `hugo --gc --minify` before committing if the build output needs to stay in sync.
- **Deployment**: `wrangler.jsonc` configures a Cloudflare Workers/Pages deployment that serves static assets from `./public`, building via `hugo --gc --minify`.
