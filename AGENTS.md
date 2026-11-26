# Portfolio Site Architecture

This is a Jekyll static site with a light, design-document-style single-page portfolio. Here's how it works:

## File Structure
```
_data/profile.yml   ←  all personal content lives here (this template uses it, but optional)
index.html          ←  single-page template with Liquid loops — replaceable with any Jekyll/static page
assets/             ←  static files (logos, badges, resume PDF, fonts)
_site/              ←  Jekyll output (auto-generated, gitignored)
```

## Data-Driven Design
The template (`index.html`) has sections — hero, experience, skills, projects, credentials (education + certifications), contact — each pulling from `profile.yml` via `{% for %}` and `{{ }}` Liquid tags. No duplicate HTML. One new portfolio = one new YAML.

But `profile.yml` is optional. Someone can drop in a completely different `index.html` that doesn't read from it — hardcoded HTML, different YAML files, multiple pages. Jekyll doesn't care.

## Universal Stack (applies regardless of template)
- **Jekyll** processes everything in the repo (`.html`, `.md`, `_data/`, `_layouts/`, `_includes/`) into `_site/`
- **Tailwind CSS** via CDN (`<script src="https://cdn.tailwindcss.com">`) — runs in-browser, no build step
- **Google Fonts** (IBM Plex Sans, IBM Plex Sans Condensed) loaded via CDN `<link>`
- Icons are small inline SVGs (no icon font). Skill icons live in one `<svg>` sprite at the top of
  `<body>`: 24px grid, 1.5 stroke, square caps, `currentColor` only (monochrome, so they invert on pressed chips)

## YAML Schema
```yaml
name: ""
title: ""
role: ""
nav: []              # [{label, href}]
meta: {description: "", favicon: "", resume_label: "", theme_color: ""}
hero:
  headline: ""
  description: ""
  resume_url: ""
  github_url: ""
  facts:            # margin column beside the headline
    - label: ""
      value: ""
      groups: []     # optional; instead of value, labelled rows: [{label, items: []}]
      live: false    # true → accent status dot
  note:             # highlighted note under the actions
    title: ""
    text: ""         # supports **bold**, rendered as a highlight
experience:
  heading: ""
  description: ""
  jobs:
    - year: ""
      title: ""
      company: ""
      company_logo: ""   # path to assets/logos/...
      current: false     # mint dot + mint year
      focus: []          # optional tag chips
      groups:            # title may be "" for an untitled group
        - title: ""
          bullets: []    # first 3 shown; the rest behind "Show N more"
skills:
  heading: ""
  description: ""
  items:
    - name: ""
      category: ""       # skills are grouped by category, in first-seen order
      icon: ""           # optional; name of an inline SVG symbol (#i-<name>) in index.html:
                         # agents, workflow, metadata, function, spark, database, stream, schema,
                         # terminal, table, code, api, cloud, schedule, layers, container
      keys: []           # optional; lowercase substrings matched against project
                         # subindustry + tags. With keys the chip filters projects.
projects:
  heading: ""
  description: ""
  filter_prefix: ""
  filter_clear: ""
  items:
    - category: ""
      title: ""
      flow: []           # 3–4 short stage names drawn as an architecture sketch
      description: ""    # supports **bold** via markdownify filter
      subindustry: []    # brighter outlined chips
      tags: []           # muted outlined chips
      url: ""
contact:
  heading: ""
  availability: ""
  email: ""
  email_label: ""
  copy_label: ""
  copied_label: ""
  location: ""
  github_url: ""
  linkedin_url: ""
credentials:
  heading: ""            # one section renders education + certifications
education:
  items:
    - degree: ""
      school: ""
      year: ""
certifications:
  items:
    - name: ""
      issuer: ""
      year: ""
      badge_url: ""      # local path or external URL
      expired: false     # true → grayscale badge + reduced opacity
footer:
  brand: ""
  copyright: ""
```

## Key Styling Conventions
- Light "design document" look, defined in `<script id="tailwind-config">`:
  `paper #F6F7F9` (page), `ink #14181F` (text, primary buttons), `sub #4E5663` (secondary text),
  `rule #D8DCE2` (hairlines, borders), `accent #0F766E` (deep teal) (the one accent: links, active nav, current/live state,
  highlights, focus ring). White (`bg-white`) for chips, inputs and secondary buttons
- **Square corners everywhere.** A global `border-radius: 0` rule enforces it; never add `rounded-*`.
  The only round shape is the small status dot (`.live-dot`)
- Layout: every section uses the `.doc` grid — a 240px margin column (dates, facts, categories, in
  `font-cond`) beside a reading column capped at `.measure` (66ch). Sections and entries are separated by
  `border-t border-rule` hairlines; one paper background throughout, no alternating bands
- Type: IBM Plex Sans for everything; IBM Plex Sans Condensed (`font-cond`) for margin metadata and chips.
  Sentence case labels, no all-caps eyebrows
- Motion: one orchestrated moment (the opening reads in once). Everything else responds to the visitor
  (show more, skill filter, copy email). All motion respects `prefers-reduced-motion`
- Expired certs get `grayscale` on the badge + `opacity-60` on the row + muted year
- Company logos sit in fixed-size white badges (`h-10 w-28`) so different logo shapes match

## How to Run
```bash
jekyll serve --host 0.0.0.0 --port 4000
# Site at http://localhost:4000
```

---

# Conversion Task

Given any hardcoded HTML portfolio page (in `index.html` or similar), convert it to follow this architecture:

1. Extract all personal content (names, titles, descriptions, links, job entries, skills, projects, certifications, contact info) into `_data/profile.yml` following the schema above.

2. Replace all hardcoded content in the HTML with Jekyll Liquid tags (`{{ site.data.profile.name }}`, `{% for job in site.data.profile.experience.jobs %}`, `{% if cert.expired %}`, etc.).

3. Keep the original visual design — colors, fonts, layout, Tailwind classes. Only replace content, never styling. If the page uses different fonts or colors, preserve them.

4. If the page has sections not in the schema above (e.g. education, testimonials, blog feed, interactive widgets), add them to both the YAML and the template. Use descriptive key names.

5. If a section doesn't fit cleanly into a data loop (e.g. embedded media, custom layout, one-off components), leave it in the HTML but parameterize it with YAML flags like `section_style: "alt"` or boolean toggles so future templates can branch on them.

6. After conversion, the output must:
   - Serve correctly at `http://localhost:4000` with `jekyll serve`
   - Render identically to the original in both content and appearance
   - Use the same CSS/font CDNs as the original (don't force our Tailwind config unless adopting the full design)

7. If the original has no existing `_data/` directory, create it. If `index.html` already has front matter (`---`), preserve it. Add `---\n---` at the top if missing (tells Jekyll to process the file).
