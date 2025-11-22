<!--
Repository: HelloBlake.Jekyll
Purpose: Provide concise, actionable guidance for AI coding agents working on this Jekyll site.
Do not replace this file without consulting the repo owner.
-->

# Copilot / AI Agent Instructions

This repository is a Jekyll-based personal site using the Scriptor theme. The guidance below is focused on codebase-specific patterns, developer workflows, and concrete examples so an AI agent can be immediately productive.

**Big Picture:**
- **Type:** Static site built with Jekyll (theme: Scriptor-derived).
- **Source folders:** editable content and templates live at the repo root (e.g. `/_posts`, `/_includes`, `/_layouts`, `/_sass`, `/assets`, `/images`).
- **Generated output:** Jekyll builds go to `_site/` (and there are `site/` and `raft/` directories present in the repo — treat these as generated copies and avoid editing them directly).

**Key files & where to look**
- **`_config.yml`**: single source for site metadata (title, analytics id, disqus shortname), pagination, permalinks (`permalink: /articles/:title/`), Markdown & Sass settings. Use it for global variable lookups.
- **`Gemfile`**: pins `jekyll ~> 4.4.0`, includes `jekyll-paginate` and `jekyll-sitemap`. Respect versions here when running or updating Jekyll.
- **Layouts & includes:** change structure/HTML in `/_layouts/*.html` and `/_includes/*.html`. Example: `/_layouts/post.html` and `/_includes/header.html` control post HTML and header markup.
- **Styles:** Sass lives in `/_sass` (see `sass_dir` in `_config.yml`). Edit `_sass/_site-header.scss` or `_sass/_site-footer.scss` for global style changes.
- **Content:** posts are standard Jekyll posts in `/_posts/YYYY-MM-DD-title.md` and pages like `about.md`. Front matter follows Jekyll norms.
- **Site data:** JSON files in `/_data/` (like `author.json`, `social.json`) are used by includes (example: `_includes/social.html`).

**Local dev & build commands (concrete)**
- **Install deps:** `bundle install` (use the Ruby environment the repo expects).
- **Serve locally:** `bundle exec jekyll serve` (default).
- **Serve including drafts:** `bundle exec jekyll serve --drafts` (note: `-draft` is incorrect — use `--drafts`).
- **Build:** `bundle exec jekyll build` produces `_site/`.
- **Important:** `_config.yml` is not reloaded automatically by `jekyll serve` — restart the server after changes to `_config.yml`.

**Project-specific conventions & patterns**
- **Permalinks:** posts and articles are routed under `/articles/:title/` (see `_config.yml`). When generating links or testing, expect article URLs to follow that pattern.
- **Pagination:** configured via `jekyll-paginate` with `paginate: 3` and `paginate_path: '/page:num/'`. When changing list templates, respect the paginate variables.
- **Assets pipeline:** Sass sources are in `/_sass` and compiled by Jekyll. CSS files are expected under `/assets/css/` in the built site.
- **Third-party integrations:** `google_analytics` id and `disqus` shortname live in `_config.yml` and are injected via `_includes/google_analytics.html` and `_includes/disqus.html`.
- **Do not edit generated dirs:** avoid changing files under `/site`, `/raft`, and `_site` — those appear to be generated outputs or export copies.

**Patterns for making changes**
- To change HTML structure: update `/_layouts/*.html` then test with a local `jekyll serve` build.
- To add a snippet used across pages: create or update an include in `/_includes/` and reference it with `{% include my_snippet.html %}`.
- To change site config or metadata: edit `_config.yml` and restart `jekyll serve`.

**Debugging tips / gotchas**
- If `bundle exec jekyll serve` fails, confirm `bundle install` completed and that the `jekyll` version matches the `Gemfile` (`~> 4.4.0`).
- For Windows (PowerShell) use the same `bundle` commands in `pwsh.exe` — ensure Ruby is on PATH and `gem` dependencies installed.
- Use `--drafts` (two dashes) to include drafts; missing this flag is a common cause of missing content locally.

**Useful examples from this repo**
- Global config & variables: `_config.yml` (title, description, analytics, disqus, permalinks).
- Header include: `_includes/header.html` — site header and nav logic.
- Post layout: `_layouts/post.html` — canonical post rendering and Disqus injection.
- Site data usage: `_includes/social.html` reads `/_data/social.json`.

**When editing, prefer small, testable changes**
- Make one template or Sass change, run `bundle exec jekyll serve --drafts`, and verify output in the browser at `http://127.0.0.1:4000`.

If anything in this instruction is unclear or you'd like more examples (e.g., a walkthrough changing the header, or where to add a site-wide meta tag), ask and I will expand with step-by-step edits and exact file locations.
