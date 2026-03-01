# Portfolio Design — Christina Andrea Putri

> GitHub Pages portfolio for an SAP Consultant & Developer.
> Approved: 2026-03-01

## Approach

Hybrid single-page scroll + dedicated project detail pages.
Main page scrolls through all sections; each project card links to its own detail page.

## Tech Stack

| Tool | Purpose |
|------|---------|
| Astro 5.x | Static site generator, zero JS by default |
| Vanilla CSS | Styling via CSS custom properties (design tokens) |
| CSS + Intersection Observer | Scroll-triggered fade-in animations |
| Web3Forms | Contact form backend (free, no server) |
| Google Fonts | Inter, Poppins, JetBrains Mono |
| Astro Icon + simple-icons | Tech logos, social link icons |
| GitHub Pages | Hosting via GitHub Actions deploy |

## Color Palette

| Role | Name | Hex | Usage |
|------|------|-----|-------|
| Primary | Soft Rose | `#F4A0B5` | Buttons, links, accents |
| Primary Light | Blush | `#FDE2E8` | Card backgrounds, hover states |
| Primary Dark | Deep Rose | `#D4708F` | Active states, emphasis |
| Background | Pearl | `#FFF8F9` | Page background |
| Surface | White | `#FFFFFF` | Cards, sections |
| Text Primary | Charcoal | `#2D2D2D` | Headings, body text |
| Text Secondary | Warm Gray | `#6B5B6E` | Subtitles, captions |
| Accent | Soft Mauve | `#C9A0DC` | Tags, badges, secondary highlights |

## Typography

- **Headings:** Poppins (semibold/bold)
- **Body:** Inter (regular/medium)
- **Code/Tech tags:** JetBrains Mono

## Design Tokens

- Border radius: `12px`
- Box shadow: `0 4px 20px rgba(244, 160, 181, 0.15)`
- Spacing base unit: `4px`

## Responsive Breakpoints

- Mobile: `< 768px` — single column, stacked
- Tablet: `768px – 1024px` — 2-column card grid
- Desktop: `> 1024px` — full layout, max-width `1200px` centered

## Main Page Sections (top to bottom)

### 1. Navigation Bar (sticky)
- Name: "Christina Andrea Putri"
- Links: About, Experience, Skills, Certifications, Projects, Contact
- Glass-morphism background on scroll (`backdrop-filter: blur`)

### 2. Hero (full viewport)
- Heading: "Christina Andrea Putri"
- Subtitle: "SAP Consultant & Developer"
- Brief intro (1-2 sentences)
- CTA buttons: "View Projects" + "Contact Me"
- Animated gradient background (soft pink → blush → pearl)
- Decorative geometric shapes (subtle)

### 3. About
- Circular profile photo with pink-tinted border
- Bio paragraph (3-5 sentences)
- Quick stats row (years of experience, projects, technologies)

### 4. Experience
- Vertical timeline layout
- Each entry: role, company, date range, description
- Timeline line in soft rose, dot markers
- Placeholder entries initially

### 5. Skills
- Grid grouped by category:
  - SAP: Fiori/UI5, SAP CPI, OData, ABAP
  - Frontend: JavaScript, XML Views, HTML/CSS
  - Backend: Groovy, Node.js
  - Tools: Git, AWS, Docker
  - ML/Data: Python, TensorFlow, OpenCV
- Each skill as a pill/badge

### 6. Certifications
- Card grid layout
- Each card: cert name, issuer, date, icon
- Placeholder cards initially

### 7. Projects (overview)
- 3 project cards in responsive grid
- Each card: title, short description, tech tags, thumbnail
- "View Details →" link to dedicated page
- Fade-in on scroll

### 8. Contact
- Form: name, email, message (via Web3Forms)
- Social links: Email, LinkedIn, GitHub
- Soft pink gradient background

### 9. Footer
- Copyright, social icons, "Built with Astro"

## Project Detail Pages

### Shared Layout
- Back button → main page `#projects`
- Hero banner with project title
- Prev/Next project navigation

### /projects/fiori-training
- **Title:** SAP Fiori Airlines Management
- **Subtitle:** Enterprise CRUD application built with SAP UI5
- **Tags:** SAP UI5, OData 2.0, JavaScript, XML Views, SAPUI5
- **Description:** Full-stack airline management app with filtering, CRUD, responsive Object Page layouts, mock data server
- **Features:** Dynamic filtering, carrier detail with connections/flights tables, responsive design, i18n
- **Screenshots:** Placeholder area
- **GitHub link**

### /projects/groovys-script
- **Title:** SAP CPI Authentication Scripts
- **Subtitle:** OAuth & digital signature flows for SAP Cloud Integration
- **Tags:** Groovy, SAP CPI, OAuth, JSON, Java
- **Description:** Integration scripts for token-based auth with digital signatures, secure token management, service-to-service communication
- **Features:** Pre-auth flow, token management with expiry, signature service, masked logging
- **Code snippet showcase**
- **GitHub link**

### /projects/carbon-prediction
- **Title:** Carbon Prediction from Drone Imagery
- **Subtitle:** Machine learning internship project
- **Badge:** Internship Project
- **Tags:** Python, TensorFlow, OpenCV, AWS Lambda, Docker, Scikit-learn
- **Description:** ML pipeline predicting carbon from drone NIR images using FAST/SIFT features and neural networks, deployed on AWS Lambda
- **Features:** FAST + SIFT extraction, CLAHE enhancement, serverless inference, 10% MAPE / 0.75 R²
- **Architecture diagram** (serverless_architecture.png from repo)
- **Results section:** Model metrics
- **GitHub link**

## File Structure

```
christinandrea.github.io/
├── src/
│   ├── layouts/
│   │   └── BaseLayout.astro
│   ├── pages/
│   │   ├── index.astro
│   │   └── projects/
│   │       ├── fiori-training.astro
│   │       ├── groovys-script.astro
│   │       └── carbon-prediction.astro
│   ├── components/
│   │   ├── Navbar.astro
│   │   ├── Hero.astro
│   │   ├── About.astro
│   │   ├── Experience.astro
│   │   ├── Skills.astro
│   │   ├── Certifications.astro
│   │   ├── ProjectCard.astro
│   │   ├── ProjectsSection.astro
│   │   ├── Contact.astro
│   │   ├── Footer.astro
│   │   └── ScrollReveal.astro
│   ├── styles/
│   │   ├── global.css
│   │   └── components.css
│   └── data/
│       ├── projects.json
│       ├── skills.json
│       └── experience.json
├── public/
│   ├── images/
│   └── favicon.svg
├── .github/
│   └── workflows/
│       └── deploy.yml
├── astro.config.mjs
└── package.json
```

## Astro Config

```js
import { defineConfig } from 'astro/config';

export default defineConfig({
  site: 'https://christinandrea.github.io',
  output: 'static',
});
```

## Animations

- Scroll reveal: Intersection Observer adds `.visible` class → CSS `opacity` + `translateY` transition
- Hero gradient: CSS `@keyframes` slow color shift
- Navbar: `backdrop-filter: blur(10px)` on scroll
- Hover: card lift with shadow transition
- All transitions: `300ms ease` default
