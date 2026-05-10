# AGENTS.md

This file gives AI coding agents the context they need to work effectively in this repository. It follows the [agents.md](https://agents.md/) open standard.

## Project Overview

This is a Jekyll-based documentation site that uses the [`just-the-docs`](https://github.com/pmarsceill/just-the-docs) theme. The repo was originally scaffolded as a Forestry.io starter (Forestry was a git-based CMS that has since shut down), so some configuration in `.forestry/` is legacy and unused by the live site.

The site is plain Jekyll: content is authored as Markdown with YAML front matter, navigation is driven by front matter (`nav_order`, `parent`, `has_children`), and the theme is consumed as a gem.

## Tech Stack

- **Static site generator:** Jekyll `~> 3.8`
- **Theme:** `just-the-docs` (gem)
- **Language:** Ruby (build), Markdown + Liquid (content)
- **Plugins:** `jekyll-seo-tag`, `jekyll-sitemap`
- **Ruby:** `> 2.5`
- **Hosting:** Netlify or Vercel (see `README.md`)

## Setup

```bash
bundle install
```

If `bundle install` fails on a fresh machine, you likely need a Ruby toolchain (`ruby`, `ruby-dev`, `build-essential` on Debian/Ubuntu) before retrying.

## Build & Serve

```bash
# Local dev server with live reload at http://localhost:4000
bundle exec jekyll serve

# One-off production build into ./_site
bundle exec jekyll build
```

Production deploys (Netlify/Vercel) run `jekyll build` and publish `_site/`.

## Repository Layout

```
.
├── _config.yml           # Jekyll site config (title, theme, plugins, search)
├── Gemfile               # Ruby dependencies
├── index.md              # Homepage
├── 404.html              # 404 page
├── docs/                 # All documentation pages live here
│   ├── index.md
│   ├── navigation-structure.md
│   └── configuration/
├── assets/               # Images and other static assets
├── .forestry/            # Legacy Forestry CMS config — do not rely on
└── README.md
```

- **New doc pages** belong under `docs/` (or a nested subdirectory) with appropriate front matter.
- **Images** go in `assets/images/` and are referenced with site-relative paths.
- **Site-wide settings** (title, search, footer, plugins) live in `_config.yml`.

## Content Conventions

Every documentation page should start with Jekyll front matter, e.g.:

```yaml
---
layout: default
title: Page Title
nav_order: 2
parent: Section Name      # omit for top-level pages
has_children: true        # only for section index pages
permalink: /section/page/ # optional, otherwise derived from path
---
```

Notes:
- Use sentence case for page titles.
- `nav_order` controls sidebar ordering; keep values stable when adding pages between existing ones.
- Headings inside a page should start at `##` (the page `title` becomes the `<h1>`).
- Prefer relative links between docs (`[link](../other-page)`) over absolute URLs.

## Code Style

- **Markdown:** ATX-style headings (`#`), fenced code blocks with a language tag, no hard line wrapping inside paragraphs.
- **YAML / front matter:** two-space indentation, no trailing whitespace, keys in lowercase snake_case.
- **HTML / Liquid in includes:** two-space indentation; keep logic minimal — prefer config-driven data over inline branching.

## Testing & Verification

There is no automated test suite. Before considering a change done:

1. Run `bundle exec jekyll build` and confirm it completes without warnings or errors.
2. Run `bundle exec jekyll serve` and visually verify any pages you touched in the browser.
3. Check that sidebar navigation, page titles, and internal links still resolve.

Do not mark a documentation change complete without at least a successful local build.

## Git Workflow

- Default branch: `master`.
- Create feature branches off `master`; keep them focused (one logical change per PR).
- Commit messages: short imperative subject (≤ 72 chars); add a body when the "why" isn't obvious.
- Do not commit `_site/`, `Gemfile.lock` is intentionally git-ignored, and never commit anything from `vendor/`, `node_modules/`, or `.bundle/`.

## Boundaries — Do Not Touch Without Reason

- `.forestry/` — legacy CMS config, kept for historical reference. Do not modify unless the user explicitly re-introduces Forestry tooling.
- `_config.yml` — changes here affect every page; only edit when the request is genuinely site-wide.
- `Gemfile` — pinning or upgrading gems can break the Netlify/Vercel build; coordinate any version bumps with the user first.
- `LICENSE` — never modify.

## Security

- Never commit secrets, API keys, deploy hooks, or analytics IDs that aren't already public.
- The `ga_tracking` field in `_config.yml` is intentionally blank; only fill it in when explicitly asked.
- Treat any user-supplied HTML in Markdown as untrusted — prefer Markdown constructs over raw `<script>` or `<iframe>`.
