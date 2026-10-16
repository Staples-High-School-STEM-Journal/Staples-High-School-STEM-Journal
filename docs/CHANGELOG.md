# Changelog

All notable changes to this site are documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [Unreleased] — 2026-08-01 modernization

Owner-directed cosmetic modernization (see `docs/decisions.md` for rationale
and `docs/changes/SESSION-2026-08-01.md` for the exhaustive per-file log).

### Added

- Author system: `_authors/` collection (output at `/authors/<slug>/`),
  `_layouts/author.html` profile pages, and linked bylines
  ("By Staples STEM Journal · {date}") on all 7 issue pages and all
  front-page entries. One generic author: `staples-stem-journal`.
- Post thumbnails: new `image:` front-matter variable on all 7 posts
  (convention: `/assets/Issues/<issue-slug>/thumbnail.jpg`; images not yet
  supplied — broken-image state fails quietly by design).
- Shared includes: `entry-list.html` (parameterized entry rows),
  `byline.html`, `author-list.html`, `year-nav.html`.
- Masthead restyle: centered nameplate, newspaper rule, centered uppercase
  nav; hairline rules throughout; warm paper palette; serif system font
  stack (all via minima's `custom-variables.scss` / `custom-styles.scss`
  override hooks).
- Accessibility: skip link, one `h1` per page, landmark labels, visible
  focus outlines, WCAG-AA-verified palette.
- Offline scaffolding (excluded from build via `templates/`): authors
  directory + per-year staff pages (`_layouts/authors.html`,
  `_layouts/authors-year.html`, `_data/ranks.yml`), section pages
  (`_layouts/section.html`, `_data/sections.yml`), issues archive
  (`_layouts/issues-archive.html`), and ready page stubs in `templates/`.
- Documentation in `docs/` (adding-authors, sections, design-notes,
  decisions, change logs).
- `.gitignore` for `_site/`.

### Changed

- `_config.yml`: repaired invalid YAML (`author:` map; `social_links:`
  nested under `minima:` as a list — values unchanged). Additive-only
  otherwise: `collections:`, `defaults:`, `exclude:`.
- Issue PDFs moved from `assets/Issues/<name>.pdf` to
  `assets/Issues/<name>/<name>.pdf` (owner-directed reorganization;
  `issueLoc:` in each post updated to match).
- Front page now renders hairline-separated entry rows (thumbnail, linked
  serif title, byline-dateline) instead of the default minima post list.
- `staff.md`: markup cleanup only (markdown headings instead of inline
  HTML); rendered visible text is byte-identical to before.
- Footer markup tidied (invalid `h5 > p` nesting removed); visible text
  unchanged. Font Awesome stylesheet moved from the footer to
  `_includes/custom-head.html` (valid `<head>` placement).
- Generated `_site/` files previously committed to the repo are removed
  from version control.

### Minima dead-code removals

Removed carried-over minima features that rendered nothing (verified
against baseline output; details in `docs/changes/SESSION-2026-08-01.md`):

- Disqus comments wiring: `_includes/comments.html` and its invocation
  blocks in `_layouts/issue.html` / `_layouts/post.html` (`site.disqus`
  was never configured).
- Pagination markup in `_layouts/home.html` (`site.paginate` unset;
  jekyll-paginate not installed) and the `.pagination`,
  `.comments-disabled-message`, `.feed-subscribe`, `.post-list`, and
  `.post-link` Sass rules (the only core-partial edits — individually
  logged).

NOT removed (flagged): the empty feed-link `<li>` in
`_includes/social.html` (removal would alter footer layout) and the
Font Awesome CDN stylesheet (required by the live Issuu icon — the site's
only external request; see `docs/decisions.md` #9).

