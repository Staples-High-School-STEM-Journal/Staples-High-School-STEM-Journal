# Sections and the issues archive

Scaffolding only — nothing here is live until activated.

## Declaring sections

`_data/sections.yml` holds one entry per section (examples are commented
out in the file):

```yaml
- slug: reviews
  name: Reviews
```

Then add `section: reviews` to a post's front matter.

- **Front page:** as long as no post declares a `section:`, the front page
  renders one flat entry list (today's behavior). The moment any post does,
  the front page groups: one "section-heading" per section that actually
  has posts (empty sections never render a heading), followed by one
  unheaded list of posts with no (or an unknown) section.
- **Section pages:** copy `templates/section.md.example` to the repository
  root, fill in the TODO values (`title:`, `section:` slug, permalink).
  The page lists the section's posts via the shared entry-list include.

## Activating the issues archive

Move `templates/issues-archive.md` to the repository root. It goes live at
`/issues/`, listing every issue newest-first via the shared entry-list
include. Link it from prose or nav as desired (see `templates/page.md.example`
for the nav behavior of new pages).

## The shared listing component

`_includes/entry-list.html` renders the hairline-separated entry rows used
by the front page, section pages, the archive, and author profiles.
Parameters (pass as strings where noted):

| Parameter   | Meaning                                             |
| ----------- | --------------------------------------------------- |
| `posts`     | array of post documents (required)                  |
| `thumbnail` | `"true"`/`"false"` — render `post.image` thumbnails |
| `byline`    | `"true"`/`"false"` — render the byline-dateline     |
| `heading`   | `2` or `3` — heading level of entry titles          |

Author cards use the sibling include `_includes/author-list.html`
(parameters `authors`, `heading`) with the same markup pattern and CSS
classes, role in the meta position.
