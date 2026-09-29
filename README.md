# Faraz Mazhar — Portfolio Site

[![Jekyll](https://img.shields.io/badge/jekyll-4.x-CC0000?logo=jekyll)](https://jekyllrb.com)
[![GitHub Pages](https://img.shields.io/badge/github%20pages-active-222?logo=github)](https://farazmazhar.com)
[![Tailwind CSS](https://img.shields.io/badge/tailwind-css-06B6D4?logo=tailwindcss)](https://tailwindcss.com)
[![Content](https://img.shields.io/badge/content-via%20YAML-blue?logo=yaml)](_data/profile.yml)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)

Source for my personal portfolio at **[farazmazhar.com](https://farazmazhar.com)** — a dark-themed single-page Jekyll site powered entirely by a YAML data file.

## Quick Start

```bash
jekyll serve --host 0.0.0.0 --port 4000
# Visit http://localhost:4000
```

## Editing Content

All personal content lives in **`_data/profile.yml`**. Change your name, add a job, update skills, or swap projects — no HTML needed.

```yaml
# Example: adding a new skill
skills:
  items:
    - name: "Kubernetes"
      icon: "deployed_code"
```

Icons use [Google Material Symbols](https://fonts.google.com/icons). Browse the library and paste the icon name.

## Architecture

For a full breakdown of how this site works — file structure, YAML schema, styling conventions, and how to convert any hardcoded HTML portfolio into this data-driven setup — see **[AGENTS.md](AGENTS.md)**.

## Deploying

Push to `master` or `dev`. GitHub Pages builds via Jekyll automatically:

```bash
git add _data/profile.yml
git commit -m "Updated portfolio"
git push
```
