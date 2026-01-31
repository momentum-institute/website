Momentum Institute — Hugo content guide

This README explains how to add long-form newsletters (central index + individual pages) and static pages (About, Contact, Programs, subpages, and a private mentorship application draft) to this Hugo site.

Quick Start

- Prereq: Install Hugo (use the extended build if using SCSS). Verify with `hugo version`.
- Preview locally: `hugo server -D`
- Build for publish: `hugo`

Project layout (important paths)

- Content folder: [content](content)
- Newsletters: [content/letters](content/letters)
- Layouts/partials: [layouts/partials](layouts/partials)
- Assets: [assets](assets) and [static](static) (if present)

Add a New Long-Form Newsletter
Follow these concise steps to add a newsletter that appears both in a central index and as its own page.

- Create the file: Add a markdown file to [content/letters](content/letters) (example: [content/letters/05.md](content/letters/05.md)).
- Filename: Use a readable slug or `YYYY-MM-DD-title.md` to keep dates obvious.
- Front matter: Use YAML (example below). The `summary` is used for index listings; `draft: false` publishes it.

Example front matter for a newsletter:

```yaml
---
title: "Your Newsletter Title"
date: 2026-01-31
draft: false
summary: "One-sentence summary shown on the index"
tags: ["newsletter"]
slug: "newsletter-slug"
---
```

- Central index (section list): By default Hugo will list content placed under `content/letters` at the section URL `/letters/`. If you want a custom index page (title, intro, ordering), create [content/letters/_index.md](content/letters/_index.md) with front matter like:

```yaml
---
title: "Newsletters"
layout: "list"
summary: "Archive of long-form newsletters"
---
```

- Ordering & visibility: Use the `date` field to control chronological order. For manual order, add `weight: 10` to front matter (lower weights appear earlier). Ensure `draft: false` to show on production.

- Custom index templates: The theme (hugo-PaperMod) likely provides list templates. If you need custom list behavior, add/override templates in [layouts/letters/list.html](layouts/letters/list.html) or appropriate partials.

Static pages (About, Contact, Programs, subpages)
Use top-level or nested content pages for static site sections.

- About: Create [content/about.md](content/about.md)

Example front matter:

```yaml
---
title: "About"
draft: false
menu:
  main:
    weight: 10
---
```

- Contact: Create [content/contact/_index.md](content/contact/_index.md) for a contact landing page (or `content/contact.md`).

- Programs: Use a folder for program landing page and subpages:
  - [content/program/_index.md](content/program/_index.md) — programs overview
  - [content/program/ignition.md](content/program/ignition.md) — Ignition program page
  - [content/program/crucible.md](content/program/crucible.md) — Crucible program page

Example program subpage front matter:

```yaml
---
title: "Ignition"
draft: false
menu:
  main:
    parent: "Programs"
---
```

- Private 1:1 Mentorship application (placeholder draft): Add a draft page so it doesn't go live until ready. Example path: [content/mentorship/application.md](content/mentorship/application.md)

Example placeholder front matter:

```yaml
---
title: "Private 1:1 Mentorship — Application"
draft: true
summary: "Private application (draft)."
---
```

Note: Hugo itself is static and won't enforce access control. Host-level or app-level auth (Netlify Identity, server-side) is required to restrict pages.

Authoring tips & templates

- Use `summary` in front matter for index excerpts; Hugo will also auto-generate a summary from the content if omitted.
- Use `weight` to force custom ordering on list pages.
- If you commonly add newsletters, consider creating `archetypes/letters.md` with default front matter so `hugo new letters/my-title.md` pre-populates the file.

Example minimal archetype (put in `archetypes/letters.md` if you want):

```yaml
---
title: "{{ replace .Name "-" " " | title }}"
date: {{ .Date }}
draft: true
summary: ""
tags: ["newsletter"]
---

Write your newsletter here.
```

Previewing & Publishing workflow

- Create the file and keep `draft: true` while writing. Preview drafts locally with: `hugo server -D`.
- When ready, set `draft: false`, run `hugo server` to double-check, then build with `hugo` and deploy.
- Typical Git steps:

```sh
git add content/letters/your-newsletter.md
git commit -m "Add newsletter: Your Newsletter Title"
git push
```

Troubleshooting

- If a new newsletter doesn't appear on the index: ensure `draft` is `false` (or preview with `-D`) and that it lives under `content/letters`.
- If ordering seems wrong: check `date` and `weight` front matter values; list templates may sort by `.Date` or `.Params.weight` depending on implementation.

References

- Example newsletter in this repo: [content/letters/05.md](content/letters/05.md)
- Theme docs: check hugo-PaperMod theme docs for list & single templates.

If you'd like, I can add `archetypes/letters.md` and a sample `content/letters/_index.md` next. Want me to create those files now?
