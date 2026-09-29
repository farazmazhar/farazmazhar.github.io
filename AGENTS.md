# Portfolio Site Architecture

This is a Jekyll static site with a dark-themed single-page portfolio. Here's how it works:

## File Structure
```
_data/profile.yml   ←  all personal content lives here (this template uses it, but optional)
index.html          ←  single-page template with Liquid loops — replaceable with any Jekyll/static page
assets/             ←  static files (logos, badges, resume PDF, fonts)
_site/              ←  Jekyll output (auto-generated, gitignored)
```

## Data-Driven Design
The template (`index.html`) has sections — hero, experience, skills, projects, certifications, contact — each pulling from `profile.yml` via `{% for %}` and `{{ }}` Liquid tags. No duplicate HTML. One new portfolio = one new YAML.

But `profile.yml` is optional. Someone can drop in a completely different `index.html` that doesn't read from it — hardcoded HTML, different YAML files, multiple pages. Jekyll doesn't care.

## Universal Stack (applies regardless of template)
- **Jekyll** processes everything in the repo (`.html`, `.md`, `_data/`, `_layouts/`, `_includes/`) into `_site/`
- **Tailwind CSS** via CDN (`<script src="https://cdn.tailwindcss.com">`) — runs in-browser, no build step
- **Google Fonts** (Space Grotesk, Inter, Manrope) loaded via CDN `<link>`
- **Material Symbols** for icons via Google CDN

## YAML Schema
```yaml
name: ""
title: ""
role: ""
hero:
  title_line_1: ""
  title_line_2: ""
  highlight: ""
  description: ""
  resume_url: ""
  github_url: ""
experience:
  description: ""
  jobs:
    - year: ""
      title: ""
      company: ""
      company_logo: ""   # path to assets/logos/...
      current: false
      bullets: []
skills:
  description: ""
  items:
    - name: ""
      icon: ""           # Material Symbol name (e.g. "code_blocks", "bolt", "cloud")
projects:
  description: ""
  items:
    - category: ""       # shown as label badge
      title: ""
      icon: ""           # Material Symbol name
      description: ""    # supports **bold** via markdownify filter
      subindustry: []    # blue outlined tags
      tags: []           # gray solid tech stack tags
      url: ""
      mt_class: "mt-0"   # or "mt-0 lg:mt-16" for staggered 2-col layout
certifications:
  description: ""
  items:
    - name: ""
      issuer: ""
      year: ""
      badge_url: ""      # local path or external URL
      expired: false     # true → grayscale + reduced opacity
contact:
  availability: ""
  email: ""
  location: ""
  github_url: ""
  linkedin_url: ""
footer:
  brand: ""
  copyright: ""
```

## Key Styling Conventions
- Dark theme: `class="dark"` on `<html>`, background `#0e0e0e`
- Custom Tailwind theme defined inline in `<script id="tailwind-config">`
- Primary color: `#007ACC`, surface colors: various grays
- Font classes: `font-headline` (Space Grotesk), `font-body` (Inter), `font-label` (Manrope)
- Icons use `material-symbols-outlined` class with `font-variation-settings`
- Sections alternate background between default and `bg-surface-container-low` for tonal separation
- Project cards use alternating `mt_class` for staggered 2-column layout
- Expired certs get `grayscale` filter on badge + `opacity-60` on card + muted year color
- Company logos in white rounded badges (`h-8 bg-white rounded-lg p-1 shadow-sm`)

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
