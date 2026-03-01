# Christina Andrea Putri — Portfolio

> Personal portfolio site for Christina Andrea Putri, SAP Consultant & Developer.
> Live at **https://christinandrea.github.io**

## Overview

Soft-pink themed portfolio showcasing SAP Fiori/UI5, SAP Cloud Integration, and machine learning projects. Single-page scroll with dedicated project detail pages, deployed to GitHub Pages via GitHub Actions.

## Workflow

```
  ┌─────────────────────────────────────────────────┐
  │  LOCAL                                          │
  │                                                 │
  │  src/ ──▶ npm run build ──▶ dist/               │
  │                                                 │
  └──────────────────────┬──────────────────────────┘
                         │  git push origin main
  ┌──────────────────────▼──────────────────────────┐
  │  GITHUB ACTIONS                                 │
  │                                                 │
  │  checkout ──▶ npm ci ──▶ npm run build          │
  │           ──▶ upload artifact ──▶ deploy pages  │
  │                                                 │
  └──────────────────────┬──────────────────────────┘
                         │
                         ▼
             https://christinandrea.github.io
```

## Tech Stack

- **Astro 5.x** — static site generator
- **Vanilla CSS** with CSS custom properties (pink palette)
- **Web3Forms** — contact form backend
- **Google Fonts** — Inter, Poppins, JetBrains Mono
- **astro-icon** + Iconify — icon sets
- **GitHub Actions** — CI/CD deploy to GitHub Pages

## Quick Start

```bash
npm install
npm run dev       # localhost:4321
npm run build     # production build to dist/
npm run preview   # preview production build
```

## Project Structure

```
christinandrea.github.io/
├── .github/workflows/deploy.yml   # GitHub Actions CI/CD
├── public/
│   ├── favicon.svg
│   └── images/                    # profile + project screenshots
├── src/
│   ├── components/                # Navbar, Hero, About, Experience,
│   │                              # Skills, Certifications, Projects,
│   │                              # Contact, Footer, ProjectCard
│   ├── data/
│   │   ├── projects.json
│   │   ├── skills.json
│   │   └── experience.json
│   ├── layouts/
│   │   └── BaseLayout.astro
│   ├── pages/
│   │   ├── index.astro
│   │   └── projects/
│   │       ├── fiori-training.astro
│   │       ├── groovys-script.astro
│   │       └── carbon-prediction.astro
│   └── styles/
│       └── global.css             # design tokens + utilities
├── astro.config.mjs
└── package.json
```

## Customization

| What | Where |
|------|-------|
| Projects | `src/data/projects.json` |
| Skills | `src/data/skills.json` |
| Experience | `src/data/experience.json` |
| Colors / tokens | `src/styles/global.css` |
| Contact email | `src/components/Contact.astro`, `src/components/Footer.astro` |
| Web3Forms key | `src/components/Contact.astro` — replace `YOUR_ACCESS_KEY` |
| Profile photo | Replace `public/images/profile.svg` with `profile.jpg` |
| Project images | Replace `public/images/placeholder.svg` per project |
