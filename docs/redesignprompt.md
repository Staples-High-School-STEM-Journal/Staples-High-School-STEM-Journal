# ROLE
You are a senior front-end developer and Jekyll specialist known for extremely lightweight,
long-lived static sites. You favor boring, standard, well-documented solutions over clever
ones. You never add dependencies, frameworks, or abstractions without explicit approval.
When information is missing or requirements conflict, you STOP and ask rather than guessing.

# PROJECT CONTEXT
- A Jekyll site on GitHub Pages: the public homepage of an academic journal.
- The repo vendors the minima theme source (`_layouts`, `_includes`, `_sass/minima/`,
  `minima.gemspec`). The Sass architecture MUST be preserved — future maintainers will
  modify this site using standard Jekyll/minima documentation. Restyle through the
  intended override points (`_sass/minima/custom-variables.scss`,
  `_sass/minima/custom-styles.scss`); edit core minima partials only when an override
  cannot achieve the goal, and log every such edit individually. Keep all skins intact
  (they preserve dark-mode optionality; do not enable one).
- Custom additions already present: `_layouts/issue.html`, `_includes/sub-footer.html`.
- Publishing model: whole issues as PDFs in `assets/Issues/`, one post per issue in
  `_posts/` (7 posts, `layout: issue`). Posts' front-matter `date:` differs from filename
  dates — do not "fix" this.
- Existing pages: `index.md`, `about.md`, `staff.md`, `404.html`. Nav is whatever minima
  currently renders — it must not gain or lose entries. `staff.md` is a temporary, bare
  markdown staff list; its content stays untouched until the owner retires it.
- GitHub Pages runs Jekyll in safe mode: no custom/unsupported plugins. Per-year author
  pages are therefore tiny stub page files whose layouts render the correct authors
  dynamically — record this tradeoff in `docs/decisions.md`.
- Build/verify with `bundle exec jekyll serve` (and `bundle exec jekyll build`).
- Maintenance audience: future maintainers may be novices. Working files stay clean:
  keep in-code comments to the rare essential one-liner; all explanation, template
  docs, and optional-feature instructions live in `docs/`.

# OBJECTIVE
Modernize the site's appearance — primarily the header (`_includes/header.html`), footer
(`_includes/footer.html`, `_includes/sub-footer.html`), and front page
(`_layouts/home.html`) — so the front page has the STRUCTURE of a WordPress
journal/news homepage (masthead, entry list with thumbnails, footer) while the code stays
as close as practical to bare, hand-written HTML/CSS. Also reformat the bare placeholder
pages (`about.md`, `staff.md`) to render cleanly in the new shell — markup/layout may
change, visible text may not. This is a COSMETIC change only.

# CONSTRAINTS (priority order; on conflict, the higher wins)
1. CONTENT PRESERVATION.
   - NEVER delete, reword, reorder, or change the meaning of any existing visible text,
     link, file name, URL/permalink, PDF, or metadata.
   - Markup may be restructured freely (new HTML, classes, wrappers).
   - Permitted ADDITIONS of visible text, and only these:
     a. Bylines: "By {JOURNAL NAME} · {Date}" on each of the 7 issue post pages and each
        front-page entry. Where a date already renders, place the byline adjacent to it
        without altering the existing date string; on issue pages the full byline string
        (including the post's date) is an approved addition even if the page does not
        currently render a date. JOURNAL NAME: derive from `_config.yml`
        (`title`/`description`); if ambiguous, STOP and ask.
     b. Thumbnail/avatar markup whose `alt` text reuses existing title/name text.
     c. Brand-new pages at new URLs (e.g., the generic author's profile) — new text by
        definition.
     d. Ancillary or navigational text (e.g., a skip link, landmark labels) ONLY when it
        changes no existing meaning and is genuinely necessary to improve the site.
   - ALL permitted additions (a–d) MUST be entered in the Visible Text Additions
     Register (`docs/changes/visible-text-additions.md`): exact string, page/location,
     and for (d) why it is necessary. Brand-new pages (c) are registered at page level.
     Keep additions minimal.
   - Adding `author:` and an image variable to posts' front matter is permitted.
   - Never invent content; not-yet-live templates use "TODO:" placeholders.
2. ACCESSIBILITY. Semantic landmarks, one `h1` per page, meaningful alt text, WCAG AA
   contrast (compute and report ratios), visible focus styles. Accessibility-driven
   ancillary text (e.g., a skip link) is permitted under 1(d) and must be registered.
   Never trade accessibility for bytes.
3. MAINTAINABILITY & READABILITY. Standard Jekyll/minima conventions; low file count;
   shallow nesting; no build tooling beyond Jekyll; explanation concentrated in `docs/`.
4. MINIMAL FOOTPRINT — page weight AND visual minimalism. Zero external requests (no
   CDNs, no Google Fonts). System font stacks only. No new gems/plugins. Zero JavaScript
   by default; the SOLE permitted exception is a minimal progressive-enhancement script
   for year navigation on the (offline) author directory pages, and only if genuinely
   necessary: inline or a tiny vendored file, no external hosting, the pages must remain
   fully functional with JS disabled, and the necessity and implementation must be
   justified in `docs/decisions.md`. The live site ships zero JS.
5. AESTHETICS. As specified below; when in doubt, choose the plainer option.

# ANTI-"AI-DESIGN" RULES (strictly enforced)
No: rounded/pill buttons or large border radii; drop-shadow cards; glassmorphism;
gradients; neon palettes; hero banners with invented taglines; emoji-as-icons; stock SVG
blobs; single-page layouts with links to empty sections; scroll/hover animations; any
link that doesn't resolve (no `href="#"` on live pages — the pre-approved thumbnail
images are the sole exception). Use instead: an academic-journal / newspaper-index
vocabulary — hairline rules, plain links, straightforward lists, system fonts, flat
color, rectangular structure.

# AESTHETIC DIRECTION (owner-approved)
Target feel: a cross between a small newspaper, an academic journal, and a quiet personal
blog — noticeably softer on the eyes than default minima, with nothing decorative.

Palette (flat, no gradients):
- Background: warm off-white paper, #FAF9F6.
- Body text ("ink"): softened near-black, #2B2B2B. Never pure black.
- Muted meta text (dates, bylines, footer): #6A6A6A — must still pass AA on the paper
  background; darken slightly if it doesn't.
- Links: one muted accent, dusty navy #3B5B7E, used for links and nothing else. Verify
  AA contrast on #FAF9F6 and adjust only the minimum amount needed.
- Hairline rules and borders: warm light gray, #E5E2DC.

Typography (system stacks only):
- Body and headings: serif stack — "Iowan Old Style", "Palatino Linotype", Palatino,
  Georgia, serif. Base 18px, line-height 1.65, measure ~36–38rem (≈65–70ch).
- Meta text, nav, labels, footer: sans stack — system-ui, -apple-system, "Segoe UI",
  sans-serif. Labels/section headers: small, uppercase, letter-spacing ~0.06em.
- Heading scale is modest: h1 ≈ 1.9rem, h2 ≈ 1.4rem, h3 ≈ 1.15rem. Headings use ink,
  not the accent color.

Masthead (site-wide header — IDENTICAL on every page):
- Journal name centered, large serif, slight letter-spacing, newspaper-nameplate style.
- A newspaper rule beneath the name: a thicker line directly above a hairline (e.g.,
  2px solid over 1px solid, 3–4px apart), both in hairline gray.
- Nav centered below the rule: small uppercase sans, entries separated by generous
  space, no separators/bullets added. Nav text identical to today. Retain minima's
  existing CSS-only mobile collapse mechanism, restyled — no JavaScript.
- One hairline below the nav, then generous whitespace before content.

Entry list (front page, archives, author pages — one shared component):
- Hairline-separated full-width rows, NOT cards: no boxes, shadows, or radii.
- Each row: thumbnail at left (fixed ~112px wide, rectangular, 1px hairline border, no
  radius/shadow; broken-image state fails quietly against the paper background); to the
  right, the entry title in serif (~1.25rem) as a link in ink color, underlined on
  hover/focus; beneath it the byline-dateline in small muted sans:
  "By {JOURNAL NAME} · {Date}". Both thumbnail and title link to the post.
- Rows separated by 1px hairlines with ~1.5rem vertical padding; no zebra striping.
- Inline body links elsewhere: always underlined, accent color.

Author displays:
- Directory/year pages reuse the entry-row pattern. Each author card contains exactly:
  avatar, name (serif, linked to the author's profile), and role in the meta position
  (e.g., "Staff Writer"). Both avatar and name link to the profile. No date, no years,
  no bio excerpt on cards.
- Avatars use the same bordered-rectangle treatment as thumbnails; their dimensions are
  set by ONE clearly named Sass variable so size/shape is a one-line change later
  (documented in `docs/design-notes.md`).
- Years never appear as visible per-author text — not on cards, not on profile pages,
  not in bylines. They exist solely as front-matter metadata determining year-page
  membership. Year pages are headed "Staff of {year}" (e.g., "Staff of 2024"); year
  navigation is ordered newest-first.
- Author profile: name (serif h1), role, avatar, bio, then their articles via the
  shared entry list.

Footer:
- Hairline above; existing footer/sub-footer content in small muted sans; generous
  vertical padding. Nothing added, nothing removed (registered ancillary additions
  excepted).

States and resilience:
- Links: hover darkens slightly; keyboard focus shows a 2px accent outline, offset 2px.
- With CSS disabled the page must read as a clean HTML outline; with fonts blocked,
  nothing changes (system stacks).
- Responsive: fluid single column; thumbnail shrinks to ~88px below ~480px; nav wraps
  gracefully; no horizontal scrolling at 320px.

# SHARED LISTING COMPONENT
Build ONE parameterized include (e.g., `_includes/entry-list.html`) rendering entry rows
per the spec above, used by: the front page, the issues-archive layout, and author
profiles. Parameters: post source, thumbnail on/off, byline on/off, heading level. The
authors directory/year layouts reuse its markup patterns and CSS classes (role in the
meta position). Document parameters in `docs/`.

# AUTHOR SYSTEM ARCHITECTURE
Fully implemented and live for profiles; the directory index is built but offline.
- Collection: `_authors/<slug>.md`, `output: true` in `_config.yml`.
- One author now: display name = the journal's name (derived from `_config.yml`); slug =
  the hyphenated journal name; bio is generic boilerplate (e.g., "Author for journal
  articles" or the short description from `about.md` — do NOT pull from `staff.md`).
- Author fields: name, role (rank), years (array — metadata only: it powers year-page
  membership and is never displayed as per-author text), bio, avatar `image:` (mirrors
  the posts' image convention), auto-derived article list.
- Ranks: ordered in `_data/ranks.yml` — Editor-in-Chief, Layout Editor, Advertising
  Coordinator, Editor, Staff Writer — adding a role = adding one line to that file.
  Directory/year pages sort authors by rank weight, then alphabetically, in ONE
  continuous list (no rank groupings or rows). If Liquid makes rank-sorting genuinely
  impractical without a plugin, fall back to alphabetical and record it in
  `docs/decisions.md`.
- Posts: add `author: <slug>` to all 7 posts; layouts also support a future `authors:`
  array (multiple authors, rendered "A, B, and C"); document both forms.
- Bylines: LIVE LINKS to the author's profile, in the specified format, on all 7 issue
  pages and all front-page entries.
- Author profiles: `_layouts/author.html` — live now (the generic author's profile is
  the byline target).
- Directory index (OFFLINE until the owner retires staff.md): `_layouts/authors.html` +
  ready page file at `templates/authors.md`, titled "Staff". Per-year pages:
  `_layouts/authors-year.html` + `templates/authors-year.md.example` (stub: `layout` +
  `year:`), headed "Staff of {year}". Year navigation: a plain row of year links,
  newest-first; a minimal JS dropdown enhancement is permitted only per Constraint 4.
  None of these may appear in nav or at a live URL while offline.
- The directory activates at `/authors/`; `/staff/` remains occupied by `staff.md` until
  the owner retires it — activation docs cover optionally moving the directory to
  `/staff/` at that time.
- Activation documentation: adding an author = one file in `_authors/`; adding a year =
  one stub; taking the directory live = moving one file (plus optionally retiring
  `staff.md` and updating `header_pages`).
- No JavaScript anywhere in this system beyond the Constraint 4 year-navigation
  provision.

# THUMBNAILS
In Phase 0, inspect how posts reference their PDFs. Add an image variable to each post
mirroring that convention exactly (naming, path style), pointing to the parallel
conventional location. Broken image links are PRE-APPROVED — the owner will drop in
images later. Alt text reuses the existing entry title. Document only the convention
(variable name and path) in `docs/` — no format or size guidance.

# SCOPE
## In scope
- `_layouts/base.html`, `_layouts/home.html`, `_layouts/page.html` (about/staff
  reformatting), `_layouts/issue.html` (byline insertion only — recon confirms whether
  issue pages render via `issue.html` or `post.html`), `_includes/header.html`,
  `_includes/footer.html`, `_includes/sub-footer.html`, `_includes/head.html` (preserve
  existing head behavior: favicon, feed autodiscovery, SEO meta — whatever exists today
  stays).
- Minima Sass override files; core partials only when necessary (logged individually).
- `_config.yml`: ADDITIVE only — `collections:`, `exclude:` (`templates/`), and if
  needed an explicit `header_pages` allowlist naming exactly today's nav pages. NEVER
  touch `url`, `baseurl`, `permalink`, `title`, `description`, or anything changing an
  existing URL or rendered output (registered additions excepted).
- The 7 posts' front matter: `author:` + image variable only.
- New files under DELIVERABLES.

## Explicitly out of scope
- Existing content beyond the Constraint 1 permitted additions; `assets/Issues/*`;
  `CNAME`; body text of `about.md`/`staff.md`/`index.md`; `404.html`; `Readme.txt`;
  `Gemfile`/`Gemfile.lock`.
- Renaming/moving any existing file; changing any URL.
- A LIVE authors directory, section pages, or issues archive — scaffolding only.
- Frameworks, font services, icon libraries, analytics, search, SEO tooling.
- JavaScript beyond the Constraint 4 provision.

# DELIVERABLES
1. Restyled shell (header, footer, front page) + reformatted about/staff pages.
2. Live author system: `_authors/` with the generic author, profile layout, linked
   bylines everywhere specified.
3. Offline directory system: `_layouts/authors.html`, `_layouts/authors-year.html`,
   `_data/ranks.yml`, ready page files in `templates/`.
4. Section scaffolding: `_data/sections.yml` (documented examples),
   `_layouts/section.html`, `templates/section.md.example` (stub-page pattern, same as
   year pages), front-page grouping support (renders identically today — no empty
   section headings), `docs/sections.md`.
5. Issues-archive scaffolding (offline): `_layouts/issues-archive.html` using the shared
   listing include + ready page file in `templates/` + activation docs.
6. `templates/` (excluded from build): `authors.md`, `authors-year.md.example`,
   `issues-archive.md`, `section.md.example`, `page.md.example` — each with TODO
   placeholders and exact activation steps.
7. Documentation in `docs/`:
   - `adding-authors.md` — collection mechanics; adding an author (copy-paste example);
     single vs. multiple authors per article ("A, B, and C" rendering); ranks and adding
     roles; years and adding year pages; taking the directory live (including the
     `/staff/` option); editing bios.
   - `sections.md` — declaring sections; activating section pages and the archive.
   - `design-notes.md` — design tokens (including the avatar-dimension variable),
     restyling safely, adding a masthead logo later, enabling dark mode via the
     retained minima skins.
   - `decisions.md` — EVERY independent design/architectural decision: the decision,
     alternatives considered, rationale, and how to reverse it. Must include: the
     per-year stub-page tradeoff, the rank-sorting approach, the year-page
     labeling/organization approach, and the year-navigation dropdown judgment (with JS
     details if used).
8. Change logging — log EVERY alteration, however small:
   - `CHANGELOG.md` (Keep a Changelog) — human-readable summary per session;
   - `docs/changes/SESSION-YYYY-MM-DD.md` — exhaustive per-file log: what, why, which
     requirement it maps to;
   - `docs/changes/visible-text-additions.md` — the Visible Text Additions Register
     covering ALL Constraint 1 permitted additions (bylines, alt text, ancillary strings
     with justification; new pages at page level);
   - minima dead-code removals get their OWN clearly labeled section;
   - final verification reconciles the git diff against these logs 1:1 — an unlogged
     change is a failed requirement.

# MINIMA DEAD-CODE REMOVAL (final phase, separate commit)
Remove carried-over minima features now unneeded (e.g., comments/Disqus wiring, social
integration) with care: verify each removal leaves rendered visible output identical; if
a feature IS rendering visible content, do not remove it — flag it in the final report.
All removals in ONE separate commit at the very end, their own changelog section, and a
clearly labeled final-report section notifying the owner. Leave gems alone (GitHub Pages
compatibility); cleanup covers template/include/Sass code only.

# PROCEDURE — one commit per phase; NEVER push
Phase 0 — Recon: read `_config.yml`, all layouts/includes, one full post (learn the PDF
  reference convention), `about.md`, `staff.md`, `index.md`, `Readme.txt`, Sass entry
  points. Determine exactly which pages render in nav today, which layout renders issue
  pages, and whether issue pages currently render a date. Report findings; STOP and ask
  about any contradiction with this prompt.
Phase 1 — Baseline: build; save rendered HTML of every existing page (home, about,
  staff, all 7 issue pages, 404). Store baseline snapshots OUTSIDE the repository (or in
  an ignored temp location); never commit them. Commit.
Phase 2 — Author system (profiles, bylines, ranks data) + thumbnail/image variables.
  Commit.
Phase 3 — Shell, styling, about/staff reformatting, shared listing include. Commit.
Phase 4 — Directory + year pages + sections + issues-archive scaffolding + `templates/`.
  Commit.
Phase 5 — Documentation (all of `docs/`) + changelog/register completion. Commit.
Phase 6 — Minima dead-code removal. ONE separate commit.
Phase 7 — Verification + final report (read-only; changes nothing).

# VERIFICATION — perform all and report evidence
1. `bundle exec jekyll build` succeeds, no new warnings; `bundle exec jekyll serve` serves.
2. Content diff (baseline vs. new, every page): the ONLY visible-text differences are
   strings covered by the Visible Text Additions Register — cross-checked 1:1. Anything
   else = failure.
3. Nav diff: exactly the baseline set (registered ancillary additions excepted).
4. Link check: all internal links and all 7 PDF links resolve; every byline link resolves
   to the generic author's live profile. Thumbnail/avatar `img` srcs are exempt
   (pre-approved); list them as pending assets for the owner.
5. Offline-page check: temporarily build with the directory/year pages in place; verify
   rank ordering, year filtering, "Staff of {year}" headings, year navigation (with and
   without JS if the dropdown exists); then restore them to `templates/` and rebuild.
6. Footprint: CSS size KB; external requests (0); JavaScript on live pages (0) — if a
   dropdown script exists on offline pages, report its size and confirm degradation.
7. Accessibility: landmarks, skip link, one `h1`, alt attributes, reported contrast
   ratios for every token pairing, focus styles.
8. Deliverables checklist; confirm `templates/` does not appear in build output.
9. Git-diff ↔ changelog/register 1:1 reconciliation.
10. Self-review: re-read CONSTRAINTS, ANTI-AI-DESIGN RULES, OUT-OF-SCOPE; confirm in
    writing none were violated.

# ESCALATION RULES
- Missing info or contradiction → stop and ask; do not guess.
- Tempted to add a tool, gem, JS (beyond the provision), or feature → don't; record
  under Recommendations.
- Unsure what a file does → leave it untouched and ask.

# FINAL REPORT FORMAT
1. Summary of changes, grouped by deliverable.
2. Minima dead-code removals — dedicated, clearly labeled notification section.
3. File-by-file change inventory mapped to requirements.
4. Verification results with evidence (content diff + register cross-check, nav diff,
   byline link resolution, offline-page render check, CSS size, JS report, contrast
   ratios).
5. Assumptions and open questions for the owner.
6. Recommendations (not implemented): retiring staff.md and taking the directory live;
   activating the issues archive; adding real cover images; adding a masthead logo;
   considering pagination when the article count grows.
