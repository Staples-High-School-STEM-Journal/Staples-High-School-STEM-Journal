# Design notes

All styling lives in minima's two override hooks —
`_sass/minima/custom-variables.scss` (tokens) and
`_sass/minima/custom-styles.scss` (rules). Core minima partials are
untouched except where logged in `docs/changes/SESSION-2026-08-01.md`.

## Design tokens

| Token | Value | Use |
| ----- | ----- | --- |
| `$background-color` | `#FAF9F6` | warm off-white paper |
| `$text-color`, `$heading-color` | `#2B2B2B` | ink (never pure black) |
| `$brand-color-dimmed` | `#6A6A6A` | muted meta text (AA 5.14:1 on paper) |
| `$link-base-color` | `#3B5B7E` | dusty navy accent — links only (AA 6.69:1) |
| `$link-hover-color` | `#324F6E` | accent darkened slightly |
| `$border-color-01` | `#E5E2DC` | hairline rules |
| `$base-font-family` | Iowan Old Style / Palatino / Georgia serif stack | body + headings |
| `$sans-font-family` | system-ui sans stack | meta text, nav, labels, footer |
| `$base-font-size` / `$base-line-height` | 18px / 1.65 | measure ≈38rem via `$content-width: 684px` |
| `$author-avatar-size` | 112px | **the single knob for avatar dimensions** — profile avatars and directory-card avatars read this one variable (height follows a fixed 4:5 ratio in `custom-styles.scss`), so resizing/resshaping avatars later is a one-line change |

## Restyling safely

- Change colors/typography in `custom-variables.scss`; change layout rules
  in `custom-styles.scss`. Both are imported after every skin, so they win
  in any skin mode.
- Don't edit `_sass/minima/_base.scss`, `_layout.scss`, or `skins/*` — if an
  override truly can't achieve a goal, log the core edit individually in
  `docs/changes/`.
- The masthead rule is `.masthead-rule` (2px line over 1px line, 3px apart);
  entry rows are `.entry-list`/`.entry-row` (see `docs/sections.md` for the
  include parameters).

## Adding a masthead logo later

Put the image in `assets/Images/`, then in `_includes/header.html` add an
`<img>` inside the `.site-title` link (keep the text name for screen
readers, or give the image that alt text). Size it in
`custom-styles.scss`. No other changes needed — the header is identical on
every page by construction.

## Enabling dark mode later

All five minima skins are intact in `_sass/minima/skins/`. Set
`minima: skin: auto` (or `dark`, `solarized`, …) in `_config.yml`.
Caveat: our paper palette is enforced by plain reassignments in
`custom-variables.scss`, which override every skin — to defer to a skin's
colors, remove the corresponding lines there (keep the typography tokens).
Verify contrast again afterwards.
