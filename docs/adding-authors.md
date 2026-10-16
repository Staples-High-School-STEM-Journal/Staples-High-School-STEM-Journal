# Adding and managing authors

Authors are a Jekyll collection (`_authors/`), configured in `_config.yml`
with `output: true`, `permalink: /authors/:name/`, and a front-matter
default of `layout: author`. Profiles are live pages; the authors *directory*
is offline scaffolding until activated (see "Taking the directory live").

## Adding an author

Create one file, `_authors/<slug>.md` — the slug is the filename without
`.md`. Copy-paste example:

```yaml
---
name: Jane Doe
role: Staff Writer        # optional; must match _data/ranks.yml to rank-sort
years: [2026]             # metadata only; never shown as per-author text
image: /assets/Images/jane-doe.jpg   # optional avatar
bio: TODO one-sentence bio.
---
```

That's it — the profile appears at `/authors/jane-doe/`, listing every post
that names the slug. Edit the bio by editing the `bio:` field (or the file
body, which renders beneath it).

## Crediting posts

Single author (what the 7 issue posts use):

```yaml
author: jane-doe
```

Multiple authors render "A, B, and C" in bylines:

```yaml
authors: [jane-doe, john-smith, alex-lee]
```

Bylines ("By {names} · {date}") are rendered by `_includes/byline.html`;
each name links to that author's profile. An unknown slug falls back to
plain text, so a typo never breaks the build.

## Ranks and roles

`_data/ranks.yml` lists roles from highest to lowest:

```yaml
- Editor-in-Chief
- Layout Editor
- Advertising Coordinator
- Editor
- Staff Writer
```

Adding a role = adding one line. Directory/year pages sort authors by rank
order, then alphabetically by name, in one continuous list; authors whose
`role:` is blank or unlisted sort alphabetically after all ranked roles.

## Years and year pages

`years:` is front-matter metadata only — it determines year-page membership
and is never displayed on cards, profiles, or bylines. To add a year page,
copy `templates/authors-year.md.example` to the repository root (e.g.
`authors-2026.md`), set `year:` and the permalink to the same year, done.
The page heads itself "Staff of {year}" and lists that year's members in
rank order. Year navigation (plain links, newest-first) appears on the
directory and all year pages automatically.

## Taking the directory live

1. Move `templates/authors.md` to the repository root — the directory goes
   live at `/authors/`.
2. Optionally retire `staff.md` and serve the directory at `/staff/`
   instead: change the moved file's permalink to `/staff/`, delete
   `staff.md`, and check the nav (a page with a `title:` joins the
   navigation automatically; pin the exact set with `nav_pages:` under
   `minima:` in `_config.yml` if needed).

Until then, nothing in the directory system appears in nav or at a live
URL besides individual author profiles (which are live now, because
bylines link to them).
