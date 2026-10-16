# Visible Text Additions Register

Every visible-text addition permitted under Constraint 1 of the build
guide, with exact strings and locations. Baseline for comparison: the
repo with only the `_config.yml` YAML repair applied (see
`docs/decisions.md` #1).

## (a) Bylines

Format: `By Staples STEM Journal · {Date}` — the journal name is a link to
`/authors/staples-stem-journal/`; the date is the post's existing date
string, format unchanged.

| Exact string | Location |
| ------------ | -------- |
| `By Staples STEM Journal · Sep 1, 2022` | issue page `/2022/09/01/fall-2022.html`; front-page entry |
| `By Staples STEM Journal · Feb 1, 2023` | issue page `/2023/02/01/international-2023.html`; front-page entry |
| `By Staples STEM Journal · Mar 1, 2023` | issue page `/2023/03/01/winter-2023.html`; front-page entry |
| `By Staples STEM Journal · Sep 1, 2023` | issue page `/2023/09/01/fall-2023.html`; front-page entry |
| `By Staples STEM Journal · Apr 1, 2024` | issue page `/2024/04/01/spring-2024.html`; front-page entry |
| `By Staples STEM Journal · Nov 1, 2024` | issue page `/2024/11/01/fall-2024.html`; front-page entry |
| `By Staples STEM Journal · Feb 1, 2026` | issue page `/2026/02/01/winter-2026.html`; front-page entry |

The same seven bylines also appear on the author profile's article list
(a brand-new page — see (c)). JOURNAL NAME "Staples STEM Journal" was
chosen by the owner (config `title:` is "Staples Stem Journal").

## (b) Thumbnail/avatar alt text (reuses existing title/name text)

| Exact string | Location |
| ------------ | -------- |
| `alt="Fall 2022 STEM Journal Issue"` | front page; author profile (thumbnail) |
| `alt="STEM Journal 2023 International Issue"` | front page; author profile (thumbnail) |
| `alt="Winter 2023 STEM Journal Issue"` | front page; author profile (thumbnail) |
| `alt="Fall 2023 STEM Journal Issue"` | front page; author profile (thumbnail) |
| `alt="Spring 2024 STEM Journal Issue"` | front page; author profile (thumbnail) |
| `alt="Fall 2024 STEM Journal Issue"` | front page; author profile (thumbnail) |
| `alt="Winter 2026 STEM Journal Issue"` | front page; author profile (thumbnail) |
| `alt="Staples STEM Journal"` | author profile avatar; (offline) directory/author cards |

## (c) Brand-new page (registered at page level)

- `/authors/staples-stem-journal/` — the generic author's profile
  (name heading, bio, "Articles" section). New text by definition.

## (d) Ancillary/navigational text

| Exact string | Location | Why necessary |
| ------------ | -------- | ------------- |
| `Skip to content` | skip link, all pages | keyboard/screen-reader users must be able to bypass the masthead (WCAG 2.4.1) |
| `Menu` (visually hidden) | mobile nav toggle label, all pages | the CSS-only hamburger `<label>` has no text; screen readers need an accessible name |
| visually-hidden `h1` reusing existing site title `Staples Stem Journal` | front page only | every page needs exactly one `h1`; the front page previously had none. Reuses existing config text — nothing invented |
| `aria-label="Site"` (no visible text) | `<nav>` landmark | distinguishes the landmark for assistive tech |
| `aria-label="Staff by year"` (no visible text) | year navigation, offline pages only | same |

## Owner-approved additions outside (a)–(d)

Repairing `_config.yml` with values intact (owner directive) newly renders
in the footer, relative to the last unbuildable state:

- author name `Staples Stem Journal` and email link `potentional@email.com`
- the Issuu social icon link (icon title `Stem Journal Issuu Page`)

Also owner-directed: issue PDFs moved to per-issue folders, changing PDF
URLs (see `docs/decisions.md` #8).
