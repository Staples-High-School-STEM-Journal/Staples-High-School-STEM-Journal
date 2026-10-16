# Design and architecture decisions

One entry per independent decision: the decision, alternatives considered,
rationale, and how to reverse it.

## 1. Baseline = "current site with YAML repaired"

`_config.yml` was invalid YAML (`*key:` entries), so the repo could not
build at all and no true "before" snapshot could be produced. Baseline
snapshots (kept outside the repo) were taken from the tree with only the
YAML syntax repaired (values unchanged, per owner direction).
Alternatives: guessing the last-good config on GitHub Pages (unverifiable).
Reverse: n/a — historical note only. Side effect: the footer now renders
the configured author name/email and the Issuu social icon; the owner
approved this when choosing "fix syntax, keep values".

## 2. Existing date string moved inside the byline

Issue pages rendered a `<time>` date. Rather than duplicating the date
(bylines must include it), the byline markup now *contains* that same
`<time>` element with the identical format string
(`site.minima.date_format | default: "%b %-d, %Y"`).
Alternatives: rendering date twice (rejected: visual noise); dropping the
byline date (rejected: spec requires the full byline string).
Reverse: restore the standalone `<time>` in `_layouts/issue.html` and drop
it from `_includes/byline.html`.

## 3. Author lookup by collection document `id`

Bylines/profiles resolve a slug to an author via
`site.authors | where_exp: "item", "item.id == aid"` where
`aid = "/authors/" + slug`. Alternatives: matching on a front-matter
`slug:` field (redundant — filename already is the slug) or on
`basename_without_ext` (not exposed on the document drop in all Jekyll
versions). Reverse: change the lookup in `_includes/byline.html` and
`_layouts/author.html` together.

## 4. Rank sorting in pure Liquid

Directory/year pages build one sorted array by concatenating, for each
rank in `_data/ranks.yml`, the alphabetically sorted authors with that
role; unranked roles sort alphabetically after. No plugin (GitHub Pages
safe mode forbids them) — Liquid sorting was practical, so no alphabetical
fallback was needed. Alternative considered: a custom plugin (rejected:
not supported on Pages). Reverse: reorder or delete lines in
`_data/ranks.yml`; sorting logic lives in `_layouts/authors.html` and
`_layouts/authors-year.html`.

## 5. Per-year staff pages are stub page files

Each year page is a tiny stub (`layout: authors-year` + `year:` + permalink)
instead of generated pages. Tradeoff: one file per year to create/forget,
in exchange for zero plugins and full Pages compatibility. Recorded per
the build guide. Reverse: replace stubs with a generator plugin if the
site ever leaves GitHub Pages safe mode.

## 6. Years are metadata-only; year pages are headed "Staff of {year}"

Years appear only in year navigation (newest-first) and year-page
headings — never on cards, profiles, or bylines. Year navigation is a
plain row of links rather than grouped per-author year labels.
Reverse: display `years` in `_includes/author-list.html` if ever wanted.

## 7. Year-navigation dropdown: not implemented (zero JS)

Judgment call permitted by the guide: with a handful of years, a plain row
of links is simpler, fully functional without JS, and keeps the live site
at zero JavaScript. A dropdown adds an enhancement nobody needs at this
scale. Revisit if the year count grows large; the enhancement would be a
small inline script in `_includes/year-nav.html`, justified here first.

## 8. Issue file convention (owner-directed)

Per the owner, each issue is a folder: `assets/Issues/<slug>/<slug>.pdf`
plus `thumbnail.jpg` (posts reference it via `image:`). This changed PDF
URLs — an explicit owner amendment to the guide's URL-preservation
constraint. Author avatars live in the pre-existing `assets/Images/`.
Reverse: move files back and revert `issueLoc:`/`image:` in the 7 posts.

## 9. Font Awesome CDN request kept (constraint conflict)

The repaired config activates the footer's social icon, which renders via
Font Awesome classes. Removing the CDN stylesheet would make the icon
invisible — a visible-output change violating content preservation, which
outranks the zero-external-requests rule. The stylesheet was moved from an
invalid `<body>` position to `_includes/custom-head.html`. Flagged for the
owner: replacing the icon with a plain text link would remove the site's
only external request. Reverse: remove the `social_links` config and the
`custom-head.html` link together.

## 10. Generic author profile choices

The single author `staples-stem-journal` has no `role:` (a journal has no
rank; layouts omit the role line when blank) and `years:` spanning the
journal's active period (metadata only — it exists so the offline year-page
scaffolding has meaningful membership). Bio is the guide-suggested
boilerplate. Reverse: edit `_authors/staples-stem-journal.md`.

## 11. `_layouts/post.html` kept though unused

All posts use `layout: issue`. `post.html` is minima's standard post
layout and the expected fallback for any future non-issue post, so it
stays (minus dead comments wiring — see the changelog's dead-code
section). Reverse: delete the file.

## 12. Only `templates/` is excluded from the build

`docs/`, `CHANGELOG.md`, etc. are copied to `_site` as static files (like
the pre-existing `Readme.txt`). Harmless and keeps `_config.yml` minimal;
add them to `exclude:` if you'd rather not serve them.

## 13. Palette enforced in `custom-variables.scss`, skins intact

Minima's `classic` skin assigns plain (non-`!default`) globals before the
custom-variables hook runs, so reassignments there win without touching
any skin file — preserving all five skins for future dark mode (see
`docs/design-notes.md`). Reverse: delete the palette block.
