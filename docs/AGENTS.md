# AGENTS.md

Jekyll site for the Staples High School STEM Journal. Deployed via GitHub Pages
to `shsstemjournal.org` (see `CNAME`). No CI workflows — Pages builds on push
to the default branch. Pages runs Jekyll in safe mode: no custom plugins.

## Structure

- The git repo root is this directory (`Staples-High-School-STEM-Journal/`);
  its parent only contains this folder and the build guide (`stemj-prompt.md`).
- The repo is both the site AND the theme: a vendored fork of the minima theme
  (`minima.gemspec`, pulled in via `gemspec` in the `Gemfile`). Restyle only
  through the override hooks `_sass/minima/custom-variables.scss` and
  `_sass/minima/custom-styles.scss`; core partial edits must be logged in
  `docs/changes/`. Keep all skins in `_sass/minima/skins/` intact.
- Journal issues are posts in `_posts/` named `YYYY-M-D-<season>-<year>.md`
  (month/day NOT zero-padded). Front matter: `layout: issue`, `issueLoc:`,
  `author:` (slug from `_authors/`), `image:` (thumbnail). Never touch the
  filename dates — they are deliberate estimates.
- Issue assets live one folder per issue: `assets/Issues/<slug>/<slug>.pdf`
  plus `thumbnail.jpg` (thumbnails are placeholders until the owner adds
  images — broken images are expected). Author avatars go in `assets/Images/`.
- `templates/` is OFFLINE scaffolding (excluded from the build): authors
  directory, year pages, section pages, issues archive. Activation
  instructions are in each file and in `docs/`.
- Maintainer docs live in `docs/` (adding-authors, sections, design-notes,
  decisions, change logs). Log every visible-text addition in
  `docs/changes/visible-text-additions.md` and every session's changes in
  `docs/changes/`.

## Commands

- Local dev: `bundle install` then `bundle exec jekyll serve`
  (`bundle exec jekyll build` to verify). No tests/lint.
- Pre-existing noise to ignore: Ruby "logger … default gems" warning and
  minima Sass `@import`/`lighten()` deprecation warnings. Any OTHER warning
  is new and must be investigated.

## Gotchas

- `_config.yml` is intentionally minimal; keep changes additive. Never touch
  `title`/`permalink`/anything that changes an existing URL.
- kramdown smart-quotes apostrophes in markdown but not in inline HTML —
  that's why one `<h4>` in `staff.md` stays raw HTML (visible text must not
  change). `staff.md` content is frozen until the owner retires it in favor
  of the authors directory.
- Never commit `_site/`.
- Style per `.editorconfig`: 2-space indent, LF, final newline; trailing
  whitespace allowed only in `*.md`.
