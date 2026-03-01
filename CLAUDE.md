# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Personal portfolio site for Christina Andrea Putri (SAP Consultant & Developer). Deployed to GitHub Pages at `https://christinandrea.github.io`.

## Tech Stack

- **Astro 5.x** — static site generator, ships zero JS by default
- **Vanilla CSS** with CSS custom properties (design tokens in `src/styles/global.css`)
- **No React/Vue islands** — pure `.astro` components only
- **Web3Forms** — contact form backend (access key in hidden input)
- **Google Fonts** — Inter (body), Poppins (headings), JetBrains Mono (code/tags)
- **astro-icon** + `@iconify-json/simple-icons` + `@iconify-json/mdi`

## Commands

```bash
npm run dev        # Dev server on localhost:4321
npm run build      # Static build to dist/
npm run preview    # Preview production build
```

## Architecture

Hybrid single-page scroll main page + dedicated project detail pages.

**Main page** (`src/pages/index.astro`): Hero → About → Experience → Skills → Certifications → Projects → Contact. Each section is a standalone `.astro` component in `src/components/`.

**Project pages** (`src/pages/projects/*.astro`): 3 detail pages with prev/next navigation. Each page is self-contained with its own data in the frontmatter.

**Data-driven content**: `src/data/projects.json`, `skills.json`, `experience.json` drive the main page sections. Edit these files to update content without touching components.

**Layout**: `src/layouts/BaseLayout.astro` wraps all pages with Navbar, Footer, scroll reveal script, fonts, and meta tags.

## Design System

**Color palette** (CSS custom properties in `:root`):
- `--color-primary: #F4A0B5` (Soft Rose) — buttons, links, accents
- `--color-primary-light: #FDE2E8` (Blush) — card backgrounds, hover
- `--color-primary-dark: #D4708F` (Deep Rose) — active states
- `--color-bg: #FFF8F9` (Pearl) — page background
- `--color-accent: #C9A0DC` (Soft Mauve) — badges, secondary highlights

**Responsive breakpoints**: Mobile `<768px`, Tablet `768–1024px`, Desktop `>1024px` (max-width `1200px`).

**Animations**: CSS transitions (`300ms ease`) + Intersection Observer scroll reveal (`.reveal` → `.visible`). No animation libraries.

## Key Conventions

- All styling uses CSS custom properties from `src/styles/global.css` — never hardcode colors or spacing
- Components use Astro scoped `<style>` blocks, global utilities (`.container`, `.section`, `.btn`, `.tag`) are in `global.css`
- Placeholder images in `public/images/` — replace with real screenshots/photos
- Contact form has `YOUR_ACCESS_KEY` placeholder — replace with real Web3Forms key before deploying
- Footer social links have `#` placeholders for LinkedIn and email

## Deployment

GitHub Actions (`.github/workflows/deploy.yml`) auto-deploys on push to `main`. The workflow runs `npm ci` → `npm run build` → deploys `dist/` to GitHub Pages.

## Plans

Implementation plan with full component code: `docs/plans/2026-03-01-portfolio-implementation.md`
Design decisions and rationale: `docs/plans/2026-03-01-portfolio-design.md`
