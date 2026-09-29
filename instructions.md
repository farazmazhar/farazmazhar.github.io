# Portfolio Maintenance Guide

A quick-reference for editing your live portfolio without touching HTML or CSS. For the full architecture breakdown, see [AGENTS.md](AGENTS.md).

## Where Everything Lives

```
_data/profile.yml   ←  edit this file for all content changes
index.html          ←  template (rarely touched)
assets/logos/       ←  company logos and certification badges
assets/             ←  resume PDF, favicon, fonts
```

## Editing Sections

### Hero
```yaml
hero:
  title_line_1: "Building Scalable"
  title_line_2: "Data Ecosystems"
  highlight: "for 8+ Years."
  description: "Data engineer with 8+ years..."
  resume_url: "assets/Resume_Faraz-Mazhar.pdf"
  github_url: "https://github.com/farazmazhar"
```

### Experience
Add a new job by copying an existing block under `jobs:` and updating the fields:
```yaml
- year: "Jan 2024 — PRESENT"
  title: "Staff Engineer"
  company: "Company Name - City, Country"
  company_logo: "assets/logos/company.svg"
  current: true
  bullets:
    - "Led migration of..."
    - "Designed and built..."
```

### Skills
Icons are [Google Material Symbols](https://fonts.google.com/icons) names. Browse, pick one, paste it:
```yaml
skills:
  items:
    - name: "Kubernetes"
      icon: "deployed_code"
```

### Projects
Tags are split into two rows: `subindustry` (blue outlined) and `tags` (gray solid tech stack). Use `**bold**` in descriptions for emphasis.
```yaml
projects:
  items:
    - category: "Data Engineering"
      title: "Project Name"
      icon: "dataset"
      description: "A **metadata-driven** pipeline..."
      subindustry:
        - "Lakehouse"
        - "MLOps"
      tags:
        - "Python"
        - "Spark"
      mt_class: "mt-0"
```

### Certifications
Set `expired: true` to auto-grayscale the badge and dim the card:
```yaml
certifications:
  items:
    - name: "AWS Certified Developer"
      issuer: "Amazon Web Services"
      year: "2018 — 2021"
      badge_url: "assets/logos/aws-dev-associate.png"
      expired: true
```

### Contact & Footer
```yaml
contact:
  email: "you@email.com"
  github_url: "https://github.com/yourusername"
  linkedin_url: "https://linkedin.com/in/yourusername"
```

## Updating Your Resume PDF

1. Save the new PDF to `assets/`
2. Update `resume_url:` under `hero` in `profile.yml`

## Updating Cert Badges

1. Place the badge image in `assets/logos/`
2. Point `badge_url` to the local path (e.g. `"assets/logos/az-900.svg"`)

## Deploying

```bash
git add _data/profile.yml assets/
git commit -m "Updated portfolio"
git push
```

GitHub Pages rebuilds the site automatically within minutes.
