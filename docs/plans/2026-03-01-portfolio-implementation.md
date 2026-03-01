# Portfolio Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Build a soft-pink themed Astro portfolio site for Christina Andrea Putri (SAP Consultant & Developer) with a single-page scroll main page and 3 dedicated project detail pages, deployed to GitHub Pages.

**Architecture:** Astro 5.x static site with pure `.astro` components (no React/Vue islands). Vanilla CSS with custom properties for the pink theme. Intersection Observer for scroll animations. JSON data files drive dynamic content. Web3Forms handles the contact form.

**Tech Stack:** Astro 5.x, Vanilla CSS, Web3Forms, Google Fonts (Inter, Poppins, JetBrains Mono), GitHub Pages + Actions

**Design doc:** `docs/plans/2026-03-01-portfolio-design.md`

**Working directory:** `/c/christina/christinandrea.github.io`

**Color Palette Quick Reference:**
- Primary (Soft Rose): `#F4A0B5` — buttons, links, accents
- Primary Light (Blush): `#FDE2E8` — card backgrounds, hover states
- Primary Dark (Deep Rose): `#D4708F` — active states, emphasis
- Background (Pearl): `#FFF8F9` — page background
- Surface: `#FFFFFF` — cards, sections
- Text: `#2D2D2D` — headings, body
- Text Secondary: `#6B5B6E` — subtitles, captions
- Accent (Soft Mauve): `#C9A0DC` — tags, badges

---

## Phase 1: Project Scaffolding

### Task 1: Initialize Astro project

**Files:**
- Create: `package.json`
- Create: `astro.config.mjs`
- Create: `tsconfig.json`
- Create: `.gitignore`

**Step 1: Scaffold Astro**

```bash
cd /c/christina/christinandrea.github.io
npm create astro@latest . -- --template minimal --no-install --no-git --typescript strict
```

If prompted about overwriting, answer yes. The repo is empty.

**Step 2: Install dependencies**

```bash
npm install
npm install astro-icon @iconify-json/simple-icons @iconify-json/mdi
```

**Step 3: Configure Astro for GitHub Pages**

Overwrite `astro.config.mjs` with:

```js
import { defineConfig } from 'astro/config';
import icon from 'astro-icon';

export default defineConfig({
  site: 'https://christinandrea.github.io',
  output: 'static',
  integrations: [icon()],
});
```

**Step 4: Verify dev server starts**

```bash
npm run dev
```

Expected: Astro dev server running on `localhost:4321`

**Step 5: Commit**

```bash
git add package.json package-lock.json astro.config.mjs tsconfig.json .gitignore src/
git commit -m "chore: scaffold Astro project with minimal template"
```

---

### Task 2: Create design tokens and global styles

**Files:**
- Create: `src/styles/global.css`

**Step 1: Write the file**

Create `src/styles/global.css` with this exact content:

```css
/* ===== RESET ===== */
*,
*::before,
*::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html {
  scroll-behavior: smooth;
}

img {
  max-width: 100%;
  display: block;
}

a {
  text-decoration: none;
  color: inherit;
}

ul {
  list-style: none;
}

/* ===== DESIGN TOKENS ===== */
:root {
  /* Colors */
  --color-primary: #F4A0B5;
  --color-primary-light: #FDE2E8;
  --color-primary-dark: #D4708F;
  --color-bg: #FFF8F9;
  --color-surface: #FFFFFF;
  --color-text: #2D2D2D;
  --color-text-secondary: #6B5B6E;
  --color-accent: #C9A0DC;

  /* Typography */
  --font-heading: 'Poppins', sans-serif;
  --font-body: 'Inter', sans-serif;
  --font-mono: 'JetBrains Mono', monospace;

  /* Spacing (4px base) */
  --space-1: 0.25rem;
  --space-2: 0.5rem;
  --space-3: 0.75rem;
  --space-4: 1rem;
  --space-6: 1.5rem;
  --space-8: 2rem;
  --space-12: 3rem;
  --space-16: 4rem;
  --space-20: 5rem;
  --space-24: 6rem;

  /* Sizing */
  --max-width: 1200px;
  --radius: 12px;
  --radius-sm: 8px;
  --radius-full: 9999px;

  /* Shadows */
  --shadow-sm: 0 2px 8px rgba(244, 160, 181, 0.1);
  --shadow-md: 0 4px 20px rgba(244, 160, 181, 0.15);
  --shadow-lg: 0 8px 30px rgba(244, 160, 181, 0.2);

  /* Transitions */
  --transition: 300ms ease;
}

/* ===== BASE ===== */
body {
  font-family: var(--font-body);
  color: var(--color-text);
  background-color: var(--color-bg);
  line-height: 1.6;
  -webkit-font-smoothing: antialiased;
}

h1, h2, h3, h4 {
  font-family: var(--font-heading);
  line-height: 1.2;
}

/* ===== UTILITIES ===== */
.container {
  width: 100%;
  max-width: var(--max-width);
  margin: 0 auto;
  padding: 0 var(--space-6);
}

.section {
  padding: var(--space-24) 0;
}

.section-title {
  font-size: 2rem;
  font-weight: 700;
  text-align: center;
  margin-bottom: var(--space-4);
}

.section-subtitle {
  text-align: center;
  color: var(--color-text-secondary);
  margin-bottom: var(--space-12);
  max-width: 600px;
  margin-left: auto;
  margin-right: auto;
}

/* ===== SCROLL REVEAL ===== */
.reveal {
  opacity: 0;
  transform: translateY(30px);
  transition: opacity 0.6s ease, transform 0.6s ease;
}

.reveal.visible {
  opacity: 1;
  transform: translateY(0);
}

/* ===== BUTTONS ===== */
.btn {
  display: inline-flex;
  align-items: center;
  gap: var(--space-2);
  padding: var(--space-3) var(--space-6);
  border-radius: var(--radius-full);
  font-family: var(--font-body);
  font-weight: 500;
  font-size: 1rem;
  cursor: pointer;
  border: none;
  transition: all var(--transition);
}

.btn-primary {
  background: var(--color-primary);
  color: white;
}

.btn-primary:hover {
  background: var(--color-primary-dark);
  transform: translateY(-2px);
  box-shadow: var(--shadow-md);
}

.btn-outline {
  background: transparent;
  color: var(--color-primary-dark);
  border: 2px solid var(--color-primary);
}

.btn-outline:hover {
  background: var(--color-primary-light);
  transform: translateY(-2px);
}

/* ===== TECH TAGS ===== */
.tag {
  display: inline-block;
  padding: var(--space-1) var(--space-3);
  background: var(--color-primary-light);
  color: var(--color-primary-dark);
  border-radius: var(--radius-full);
  font-size: 0.8rem;
  font-family: var(--font-mono);
  font-weight: 500;
}

/* ===== RESPONSIVE ===== */
@media (max-width: 768px) {
  .section {
    padding: var(--space-16) 0;
  }

  .section-title {
    font-size: 1.6rem;
  }
}
```

**Step 2: Commit**

```bash
git add src/styles/global.css
git commit -m "feat: add design tokens and global CSS with pink palette"
```

---

### Task 3: Create data files

**Files:**
- Create: `src/data/projects.json`
- Create: `src/data/skills.json`
- Create: `src/data/experience.json`

**Step 1: Create `src/data/` directory and write all 3 files**

`src/data/projects.json`:

```json
[
  {
    "slug": "fiori-training",
    "title": "SAP Fiori Airlines Management",
    "subtitle": "Enterprise CRUD application built with SAP UI5",
    "description": "Full-stack airline management app with dynamic filtering, CRUD operations, responsive Object Page layouts, and a mock data server for local development.",
    "tags": ["SAP UI5", "OData 2.0", "JavaScript", "XML Views"],
    "features": [
      "Dynamic filtering by Carrier ID and Currency Code",
      "Carrier detail page with connections and flights tables",
      "Responsive design across desktop, tablet, and mobile",
      "Internationalization (i18n) support"
    ],
    "github": "https://github.com/christinandrea/fiori-training",
    "image": "/images/fiori-training.png"
  },
  {
    "slug": "groovys-script",
    "title": "SAP CPI Authentication Scripts",
    "subtitle": "OAuth & digital signature flows for SAP Cloud Integration",
    "description": "Collection of Groovy scripts implementing token-based authentication with digital signatures, secure token management, and service-to-service communication for SAP Cloud Platform Integration.",
    "tags": ["Groovy", "SAP CPI", "OAuth", "JSON"],
    "features": [
      "Pre-authentication flow with header extraction",
      "Token management with expiry calculation",
      "Digital signature service integration",
      "Masked logging for production security"
    ],
    "github": "https://github.com/christinandrea/groovys-script",
    "image": "/images/groovys-script.png"
  },
  {
    "slug": "carbon-prediction",
    "title": "Carbon Prediction from Drone Imagery",
    "subtitle": "Machine learning internship project",
    "badge": "Internship Project",
    "description": "ML pipeline predicting carbon values from drone-captured NIR images using FAST/SIFT feature extraction and neural networks, deployed serverlessly on AWS Lambda.",
    "tags": ["Python", "TensorFlow", "OpenCV", "AWS Lambda", "Docker"],
    "features": [
      "FAST keypoint + SIFT descriptor feature extraction",
      "CLAHE image enhancement for NIR processing",
      "AWS Lambda serverless inference pipeline",
      "10% MAPE / 0.75 R² model performance"
    ],
    "github": "https://github.com/christinandrea/isai-carbon-prediction-2024",
    "image": "/images/carbon-prediction.png",
    "metrics": {
      "mape": "10%",
      "r2": "0.75",
      "bestHeight": "25m (6.77% error)"
    }
  }
]
```

`src/data/skills.json`:

```json
[
  {
    "category": "SAP",
    "items": ["Fiori/UI5", "SAP CPI", "OData", "ABAP"]
  },
  {
    "category": "Frontend",
    "items": ["JavaScript", "XML Views", "HTML", "CSS"]
  },
  {
    "category": "Backend",
    "items": ["Groovy", "Node.js"]
  },
  {
    "category": "Tools",
    "items": ["Git", "AWS", "Docker"]
  },
  {
    "category": "ML / Data",
    "items": ["Python", "TensorFlow", "OpenCV", "Scikit-learn"]
  }
]
```

`src/data/experience.json`:

```json
[
  {
    "role": "SAP Consultant",
    "company": "Company Name",
    "period": "2024 – Present",
    "description": "Placeholder — update with real experience details."
  },
  {
    "role": "ML Intern",
    "company": "ISAI",
    "period": "2024",
    "description": "Developed a carbon prediction pipeline from drone imagery using TensorFlow and deployed on AWS Lambda."
  }
]
```

**Step 2: Commit**

```bash
git add src/data/
git commit -m "feat: add JSON data files for projects, skills, experience"
```

---

## Phase 2: Layout & Navigation

### Task 4: Create BaseLayout

**Files:**
- Create: `src/layouts/BaseLayout.astro`
- Modify: `src/pages/index.astro`

**Step 1: Create `src/layouts/` directory and write BaseLayout**

`src/layouts/BaseLayout.astro`:

```astro
---
interface Props {
  title?: string;
  description?: string;
}

const {
  title = 'Christina Andrea Putri — SAP Consultant & Developer',
  description = 'Portfolio of Christina Andrea Putri, SAP Consultant & Developer specializing in Fiori/UI5, SAP CPI, and cloud integration.',
} = Astro.props;
---

<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="description" content={description} />
    <title>{title}</title>
    <link rel="icon" type="image/svg+xml" href="/favicon.svg" />

    <!-- Google Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <link
      href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&family=Poppins:wght@600;700&family=JetBrains+Mono:wght@400;500&display=swap"
      rel="stylesheet"
    />
  </head>
  <body>
    <slot />

    <!-- Scroll Reveal Script -->
    <script>
      const observer = new IntersectionObserver(
        (entries) => {
          entries.forEach((entry) => {
            if (entry.isIntersecting) {
              entry.target.classList.add('visible');
            }
          });
        },
        { threshold: 0.1 }
      );

      document.querySelectorAll('.reveal').forEach((el) => observer.observe(el));
    </script>
  </body>
</html>

<style is:global>
  @import '../styles/global.css';
</style>
```

**Step 2: Replace `src/pages/index.astro` with:**

```astro
---
import BaseLayout from '../layouts/BaseLayout.astro';
---

<BaseLayout>
  <main>
    <h1>Coming soon</h1>
  </main>
</BaseLayout>
```

**Step 3: Verify** — run `npm run dev`, confirm fonts load and pearl background `#FFF8F9` appears.

**Step 4: Commit**

```bash
git add src/layouts/BaseLayout.astro src/pages/index.astro
git commit -m "feat: add BaseLayout with fonts, meta, scroll reveal script"
```

---

### Task 5: Create Navbar component

**Files:**
- Create: `src/components/Navbar.astro`
- Modify: `src/layouts/BaseLayout.astro` (add Navbar import)

**Step 1: Write `src/components/Navbar.astro`**

```astro
---
const navLinks = [
  { label: 'About', href: '/#about' },
  { label: 'Experience', href: '/#experience' },
  { label: 'Skills', href: '/#skills' },
  { label: 'Certifications', href: '/#certifications' },
  { label: 'Projects', href: '/#projects' },
  { label: 'Contact', href: '/#contact' },
];
---

<nav class="navbar" id="navbar">
  <div class="container navbar-inner">
    <a href="/" class="navbar-brand">Christina Andrea Putri</a>

    <button class="navbar-toggle" aria-label="Toggle menu" aria-expanded="false">
      <span class="hamburger"></span>
    </button>

    <ul class="navbar-links">
      {navLinks.map((link) => (
        <li><a href={link.href}>{link.label}</a></li>
      ))}
    </ul>
  </div>
</nav>

<style>
  .navbar {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 1000;
    padding: var(--space-4) 0;
    transition: all var(--transition);
  }

  .navbar.scrolled {
    background: rgba(255, 248, 249, 0.85);
    backdrop-filter: blur(10px);
    box-shadow: var(--shadow-sm);
    padding: var(--space-3) 0;
  }

  .navbar-inner {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  .navbar-brand {
    font-family: var(--font-heading);
    font-weight: 700;
    font-size: 1.2rem;
    color: var(--color-primary-dark);
  }

  .navbar-links {
    display: flex;
    gap: var(--space-6);
  }

  .navbar-links a {
    font-size: 0.95rem;
    font-weight: 500;
    color: var(--color-text-secondary);
    transition: color var(--transition);
    position: relative;
  }

  .navbar-links a:hover {
    color: var(--color-primary-dark);
  }

  .navbar-links a::after {
    content: '';
    position: absolute;
    bottom: -4px;
    left: 0;
    width: 0;
    height: 2px;
    background: var(--color-primary);
    transition: width var(--transition);
  }

  .navbar-links a:hover::after {
    width: 100%;
  }

  .navbar-toggle {
    display: none;
    background: none;
    border: none;
    cursor: pointer;
    padding: var(--space-2);
  }

  .hamburger {
    display: block;
    width: 24px;
    height: 2px;
    background: var(--color-text);
    position: relative;
    transition: all var(--transition);
  }

  .hamburger::before,
  .hamburger::after {
    content: '';
    position: absolute;
    width: 24px;
    height: 2px;
    background: var(--color-text);
    transition: all var(--transition);
  }

  .hamburger::before { top: -7px; }
  .hamburger::after { top: 7px; }

  @media (max-width: 768px) {
    .navbar-toggle {
      display: block;
    }

    .navbar-links {
      position: fixed;
      top: 0;
      right: -100%;
      width: 70%;
      height: 100vh;
      flex-direction: column;
      background: var(--color-surface);
      padding: var(--space-20) var(--space-8);
      gap: var(--space-8);
      box-shadow: var(--shadow-lg);
      transition: right var(--transition);
    }

    .navbar-links.open {
      right: 0;
    }

    .navbar-links a {
      font-size: 1.1rem;
    }
  }
</style>

<script>
  const navbar = document.getElementById('navbar');
  window.addEventListener('scroll', () => {
    navbar?.classList.toggle('scrolled', window.scrollY > 50);
  });

  const toggle = document.querySelector('.navbar-toggle');
  const links = document.querySelector('.navbar-links');
  toggle?.addEventListener('click', () => {
    const expanded = toggle.getAttribute('aria-expanded') === 'true';
    toggle.setAttribute('aria-expanded', String(!expanded));
    links?.classList.toggle('open');
  });

  document.querySelectorAll('.navbar-links a').forEach((link) => {
    link.addEventListener('click', () => {
      links?.classList.remove('open');
      toggle?.setAttribute('aria-expanded', 'false');
    });
  });
</script>
```

**Step 2: Update `src/layouts/BaseLayout.astro`**

Add Navbar import at top of frontmatter and render it in the `<body>` above `<slot />`:

```astro
---
import Navbar from '../components/Navbar.astro';

interface Props {
  title?: string;
  description?: string;
}

const {
  title = 'Christina Andrea Putri — SAP Consultant & Developer',
  description = 'Portfolio of Christina Andrea Putri, SAP Consultant & Developer specializing in Fiori/UI5, SAP CPI, and cloud integration.',
} = Astro.props;
---

<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="description" content={description} />
    <title>{title}</title>
    <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <link
      href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&family=Poppins:wght@600;700&family=JetBrains+Mono:wght@400;500&display=swap"
      rel="stylesheet"
    />
  </head>
  <body>
    <Navbar />
    <slot />

    <script>
      const observer = new IntersectionObserver(
        (entries) => {
          entries.forEach((entry) => {
            if (entry.isIntersecting) {
              entry.target.classList.add('visible');
            }
          });
        },
        { threshold: 0.1 }
      );
      document.querySelectorAll('.reveal').forEach((el) => observer.observe(el));
    </script>
  </body>
</html>

<style is:global>
  @import '../styles/global.css';
</style>
```

**Step 3: Commit**

```bash
git add src/components/Navbar.astro src/layouts/BaseLayout.astro
git commit -m "feat: add sticky Navbar with glass-morphism and mobile hamburger"
```

---

### Task 6: Create Footer component

**Files:**
- Create: `src/components/Footer.astro`
- Modify: `src/layouts/BaseLayout.astro` (add Footer below `<slot />`)

**Step 1: Write `src/components/Footer.astro`**

```astro
---
const currentYear = new Date().getFullYear();
const socialLinks = [
  { label: 'GitHub', href: 'https://github.com/christinandrea' },
  { label: 'LinkedIn', href: '#' },
  { label: 'Email', href: 'mailto:your@email.com' },
];
---

<footer class="footer">
  <div class="container footer-inner">
    <p class="footer-copy">&copy; {currentYear} Christina Andrea Putri. All rights reserved.</p>
    <div class="footer-links">
      {socialLinks.map((link) => (
        <a href={link.href} target="_blank" rel="noopener noreferrer" aria-label={link.label}>
          {link.label}
        </a>
      ))}
    </div>
    <p class="footer-credit">Built with <a href="https://astro.build" target="_blank" rel="noopener noreferrer">Astro</a></p>
  </div>
</footer>

<style>
  .footer {
    background: var(--color-surface);
    border-top: 1px solid var(--color-primary-light);
    padding: var(--space-8) 0;
    text-align: center;
  }

  .footer-inner {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: var(--space-4);
  }

  .footer-copy {
    font-size: 0.9rem;
    color: var(--color-text-secondary);
  }

  .footer-links {
    display: flex;
    gap: var(--space-6);
  }

  .footer-links a {
    color: var(--color-text-secondary);
    font-size: 0.9rem;
    transition: color var(--transition);
  }

  .footer-links a:hover {
    color: var(--color-primary-dark);
  }

  .footer-credit {
    font-size: 0.8rem;
    color: var(--color-text-secondary);
  }

  .footer-credit a {
    color: var(--color-primary);
  }

  .footer-credit a:hover {
    color: var(--color-primary-dark);
  }
</style>
```

**Step 2: Update `src/layouts/BaseLayout.astro`**

Add `import Footer from '../components/Footer.astro';` in frontmatter, and render `<Footer />` after `<slot />` (before the scroll reveal script).

**Step 3: Commit**

```bash
git add src/components/Footer.astro src/layouts/BaseLayout.astro
git commit -m "feat: add Footer with social links and copyright"
```

---

## Phase 3: Main Page Sections

### Task 7: Hero section

**Files:**
- Create: `src/components/Hero.astro`
- Modify: `src/pages/index.astro`

**Step 1: Write `src/components/Hero.astro`**

```astro
<section class="hero">
  <div class="hero-bg"></div>
  <div class="hero-shapes">
    <div class="shape shape-1"></div>
    <div class="shape shape-2"></div>
    <div class="shape shape-3"></div>
  </div>
  <div class="container hero-content">
    <p class="hero-greeting">Hello, I'm</p>
    <h1 class="hero-name">Christina Andrea Putri</h1>
    <p class="hero-title">SAP Consultant & Developer</p>
    <p class="hero-intro">
      I build enterprise solutions with SAP Fiori, integrate cloud platforms,
      and explore machine learning to solve real-world problems.
    </p>
    <div class="hero-cta">
      <a href="#projects" class="btn btn-primary">View Projects</a>
      <a href="#contact" class="btn btn-outline">Contact Me</a>
    </div>
  </div>
</section>

<style>
  .hero {
    position: relative;
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    overflow: hidden;
  }

  .hero-bg {
    position: absolute;
    inset: 0;
    background: linear-gradient(135deg, #FFF8F9 0%, #FDE2E8 40%, #F4A0B5 100%);
    background-size: 200% 200%;
    animation: gradientShift 8s ease infinite;
    z-index: 0;
  }

  @keyframes gradientShift {
    0% { background-position: 0% 50%; }
    50% { background-position: 100% 50%; }
    100% { background-position: 0% 50%; }
  }

  /* Floating decorative shapes */
  .hero-shapes {
    position: absolute;
    inset: 0;
    z-index: 1;
    pointer-events: none;
  }

  .shape {
    position: absolute;
    border-radius: 50%;
    opacity: 0.15;
  }

  .shape-1 {
    width: 300px;
    height: 300px;
    background: var(--color-primary);
    top: 10%;
    right: -5%;
    animation: float 6s ease-in-out infinite;
  }

  .shape-2 {
    width: 200px;
    height: 200px;
    background: var(--color-accent);
    bottom: 15%;
    left: -3%;
    animation: float 8s ease-in-out infinite reverse;
  }

  .shape-3 {
    width: 150px;
    height: 150px;
    background: var(--color-primary-dark);
    top: 60%;
    right: 20%;
    animation: float 7s ease-in-out infinite 1s;
  }

  @keyframes float {
    0%, 100% { transform: translateY(0) rotate(0deg); }
    50% { transform: translateY(-20px) rotate(5deg); }
  }

  .hero-content {
    position: relative;
    z-index: 2;
    text-align: center;
    max-width: 700px;
  }

  .hero-greeting {
    font-size: 1.1rem;
    color: var(--color-text-secondary);
    margin-bottom: var(--space-2);
    font-weight: 500;
  }

  .hero-name {
    font-size: 3.5rem;
    font-weight: 700;
    color: var(--color-text);
    margin-bottom: var(--space-4);
    line-height: 1.1;
  }

  .hero-title {
    font-size: 1.4rem;
    color: var(--color-primary-dark);
    font-weight: 600;
    font-family: var(--font-heading);
    margin-bottom: var(--space-6);
  }

  .hero-intro {
    font-size: 1.1rem;
    color: var(--color-text-secondary);
    line-height: 1.7;
    margin-bottom: var(--space-8);
  }

  .hero-cta {
    display: flex;
    gap: var(--space-4);
    justify-content: center;
    flex-wrap: wrap;
  }

  @media (max-width: 768px) {
    .hero-name {
      font-size: 2.2rem;
    }

    .hero-title {
      font-size: 1.1rem;
    }

    .hero-intro {
      font-size: 1rem;
    }

    .shape-1 {
      width: 200px;
      height: 200px;
    }

    .shape-2 {
      width: 120px;
      height: 120px;
    }

    .shape-3 {
      width: 80px;
      height: 80px;
    }
  }
</style>
```

**Step 2: Replace `src/pages/index.astro` with:**

```astro
---
import BaseLayout from '../layouts/BaseLayout.astro';
import Hero from '../components/Hero.astro';
---

<BaseLayout>
  <main>
    <Hero />
  </main>
</BaseLayout>
```

**Step 3: Verify** — full-viewport hero with gradient animation, floating shapes, readable text, CTAs.

**Step 4: Commit**

```bash
git add src/components/Hero.astro src/pages/index.astro
git commit -m "feat: add Hero section with animated gradient and CTAs"
```

---

### Task 8: About section

**Files:**
- Create: `src/components/About.astro`
- Modify: `src/pages/index.astro` (add import + render)

**Step 1: Write `src/components/About.astro`**

```astro
---
const stats = [
  { number: '3+', label: 'Projects' },
  { number: '5+', label: 'Technologies' },
  { number: '2+', label: 'Years Learning' },
];
---

<section class="section" id="about">
  <div class="container">
    <h2 class="section-title reveal">About Me</h2>
    <div class="about-grid reveal">
      <div class="about-image">
        <div class="about-image-wrapper">
          <img src="/images/profile.jpg" alt="Christina Andrea Putri" width="300" height="300" />
        </div>
      </div>
      <div class="about-content">
        <p class="about-text">
          I'm an SAP Consultant and Developer with a passion for building enterprise
          solutions that make a difference. My expertise spans SAP Fiori/UI5 development,
          SAP Cloud Platform Integration, and full-stack web applications.
        </p>
        <p class="about-text">
          Beyond SAP, I've explored machine learning through an internship project
          predicting carbon levels from drone imagery — combining my love for technology
          with environmental impact.
        </p>
        <p class="about-text">
          I enjoy turning complex business requirements into clean, user-friendly
          interfaces and robust integration flows.
        </p>
        <div class="about-stats">
          {stats.map((stat) => (
            <div class="stat">
              <span class="stat-number">{stat.number}</span>
              <span class="stat-label">{stat.label}</span>
            </div>
          ))}
        </div>
      </div>
    </div>
  </div>
</section>

<style>
  .about-grid {
    display: grid;
    grid-template-columns: 300px 1fr;
    gap: var(--space-12);
    align-items: center;
  }

  .about-image-wrapper {
    width: 300px;
    height: 300px;
    border-radius: 50%;
    overflow: hidden;
    border: 4px solid var(--color-primary-light);
    box-shadow: var(--shadow-md);
    background: var(--color-primary-light);
  }

  .about-image-wrapper img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  .about-text {
    font-size: 1.05rem;
    line-height: 1.8;
    color: var(--color-text-secondary);
    margin-bottom: var(--space-4);
  }

  .about-stats {
    display: flex;
    gap: var(--space-8);
    margin-top: var(--space-8);
  }

  .stat {
    text-align: center;
  }

  .stat-number {
    display: block;
    font-family: var(--font-heading);
    font-size: 2rem;
    font-weight: 700;
    color: var(--color-primary-dark);
  }

  .stat-label {
    font-size: 0.9rem;
    color: var(--color-text-secondary);
  }

  @media (max-width: 768px) {
    .about-grid {
      grid-template-columns: 1fr;
      justify-items: center;
      text-align: center;
    }

    .about-image-wrapper {
      width: 200px;
      height: 200px;
    }

    .about-stats {
      justify-content: center;
    }
  }
</style>
```

**Step 2: Update `src/pages/index.astro`**

Add `import About from '../components/About.astro';` and render `<About />` after `<Hero />`.

**Step 3: Commit**

```bash
git add src/components/About.astro src/pages/index.astro
git commit -m "feat: add About section with profile photo and stats"
```

---

### Task 9: Experience section

**Files:**
- Create: `src/components/Experience.astro`
- Modify: `src/pages/index.astro`

**Step 1: Write `src/components/Experience.astro`**

```astro
---
import experienceData from '../data/experience.json';
---

<section class="section" id="experience">
  <div class="container">
    <h2 class="section-title reveal">Experience</h2>
    <p class="section-subtitle reveal">My professional journey so far</p>
    <div class="timeline">
      {experienceData.map((item, index) => (
        <div class="timeline-item reveal">
          <div class="timeline-dot"></div>
          <div class="timeline-card">
            <span class="timeline-period">{item.period}</span>
            <h3 class="timeline-role">{item.role}</h3>
            <p class="timeline-company">{item.company}</p>
            <p class="timeline-desc">{item.description}</p>
          </div>
        </div>
      ))}
    </div>
  </div>
</section>

<style>
  .timeline {
    position: relative;
    max-width: 700px;
    margin: 0 auto;
    padding-left: var(--space-8);
  }

  .timeline::before {
    content: '';
    position: absolute;
    left: 0;
    top: 0;
    bottom: 0;
    width: 2px;
    background: var(--color-primary-light);
  }

  .timeline-item {
    position: relative;
    margin-bottom: var(--space-8);
  }

  .timeline-dot {
    position: absolute;
    left: calc(-1 * var(--space-8) - 5px);
    top: var(--space-6);
    width: 12px;
    height: 12px;
    border-radius: 50%;
    background: var(--color-primary);
    border: 3px solid var(--color-bg);
    box-shadow: 0 0 0 2px var(--color-primary-light);
  }

  .timeline-card {
    background: var(--color-surface);
    border-radius: var(--radius);
    padding: var(--space-6);
    box-shadow: var(--shadow-sm);
    transition: box-shadow var(--transition);
  }

  .timeline-card:hover {
    box-shadow: var(--shadow-md);
  }

  .timeline-period {
    display: inline-block;
    font-size: 0.85rem;
    font-weight: 500;
    color: var(--color-primary-dark);
    background: var(--color-primary-light);
    padding: var(--space-1) var(--space-3);
    border-radius: var(--radius-full);
    margin-bottom: var(--space-3);
  }

  .timeline-role {
    font-size: 1.2rem;
    font-weight: 700;
    margin-bottom: var(--space-1);
  }

  .timeline-company {
    font-size: 0.95rem;
    color: var(--color-text-secondary);
    margin-bottom: var(--space-3);
  }

  .timeline-desc {
    font-size: 0.95rem;
    line-height: 1.6;
    color: var(--color-text-secondary);
  }

  @media (max-width: 768px) {
    .timeline {
      padding-left: var(--space-6);
    }

    .timeline-dot {
      left: calc(-1 * var(--space-6) - 5px);
    }
  }
</style>
```

**Step 2: Update `src/pages/index.astro`**

Add `import Experience from '../components/Experience.astro';` and render `<Experience />` after `<About />`.

**Step 3: Commit**

```bash
git add src/components/Experience.astro src/pages/index.astro
git commit -m "feat: add Experience timeline section"
```

---

### Task 10: Skills section

**Files:**
- Create: `src/components/Skills.astro`
- Modify: `src/pages/index.astro`

**Step 1: Write `src/components/Skills.astro`**

```astro
---
import skillsData from '../data/skills.json';
---

<section class="section" id="skills">
  <div class="container">
    <h2 class="section-title reveal">Skills</h2>
    <p class="section-subtitle reveal">Technologies and tools I work with</p>
    <div class="skills-grid">
      {skillsData.map((group) => (
        <div class="skill-card reveal">
          <h3 class="skill-category">{group.category}</h3>
          <div class="skill-tags">
            {group.items.map((item) => (
              <span class="tag">{item}</span>
            ))}
          </div>
        </div>
      ))}
    </div>
  </div>
</section>

<style>
  .skills-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: var(--space-6);
  }

  .skill-card {
    background: var(--color-surface);
    border-radius: var(--radius);
    padding: var(--space-6);
    box-shadow: var(--shadow-sm);
    transition: all var(--transition);
  }

  .skill-card:hover {
    transform: translateY(-4px);
    box-shadow: var(--shadow-md);
  }

  .skill-category {
    font-size: 1.1rem;
    font-weight: 700;
    color: var(--color-primary-dark);
    margin-bottom: var(--space-4);
  }

  .skill-tags {
    display: flex;
    flex-wrap: wrap;
    gap: var(--space-2);
  }

  @media (max-width: 1024px) {
    .skills-grid {
      grid-template-columns: repeat(2, 1fr);
    }
  }

  @media (max-width: 768px) {
    .skills-grid {
      grid-template-columns: 1fr;
    }
  }
</style>
```

**Step 2: Update `src/pages/index.astro`**

Add `import Skills from '../components/Skills.astro';` and render `<Skills />` after `<Experience />`.

**Step 3: Commit**

```bash
git add src/components/Skills.astro src/pages/index.astro
git commit -m "feat: add Skills section with categorized pill badges"
```

---

### Task 11: Certifications section

**Files:**
- Create: `src/components/Certifications.astro`
- Modify: `src/pages/index.astro`

**Step 1: Write `src/components/Certifications.astro`**

```astro
---
const certifications = [
  {
    name: 'SAP Certification',
    issuer: 'SAP',
    date: 'Coming soon',
    description: 'Placeholder — update with actual certification details.',
  },
  {
    name: 'Cloud Integration',
    issuer: 'SAP',
    date: 'Coming soon',
    description: 'Placeholder — update with actual certification details.',
  },
];
---

<section class="section" id="certifications">
  <div class="container">
    <h2 class="section-title reveal">Certifications</h2>
    <p class="section-subtitle reveal">Professional credentials and achievements</p>
    <div class="cert-grid">
      {certifications.map((cert) => (
        <div class="cert-card reveal">
          <div class="cert-icon">
            <svg width="40" height="40" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <circle cx="12" cy="8" r="6" />
              <path d="M15.477 12.89 17 22l-5-3-5 3 1.523-9.11" />
            </svg>
          </div>
          <h3 class="cert-name">{cert.name}</h3>
          <p class="cert-issuer">{cert.issuer}</p>
          <p class="cert-date">{cert.date}</p>
          <p class="cert-desc">{cert.description}</p>
        </div>
      ))}
    </div>
  </div>
</section>

<style>
  .cert-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
    gap: var(--space-6);
    max-width: 800px;
    margin: 0 auto;
  }

  .cert-card {
    background: var(--color-surface);
    border-radius: var(--radius);
    padding: var(--space-8);
    box-shadow: var(--shadow-sm);
    text-align: center;
    transition: all var(--transition);
  }

  .cert-card:hover {
    transform: translateY(-4px);
    box-shadow: var(--shadow-md);
  }

  .cert-icon {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    width: 70px;
    height: 70px;
    border-radius: 50%;
    background: var(--color-primary-light);
    color: var(--color-primary-dark);
    margin-bottom: var(--space-4);
  }

  .cert-name {
    font-size: 1.15rem;
    font-weight: 700;
    margin-bottom: var(--space-2);
  }

  .cert-issuer {
    font-size: 0.95rem;
    color: var(--color-primary-dark);
    font-weight: 500;
    margin-bottom: var(--space-1);
  }

  .cert-date {
    font-size: 0.85rem;
    color: var(--color-text-secondary);
    margin-bottom: var(--space-3);
  }

  .cert-desc {
    font-size: 0.9rem;
    color: var(--color-text-secondary);
    line-height: 1.6;
  }
</style>
```

**Step 2: Update `src/pages/index.astro`**

Add `import Certifications from '../components/Certifications.astro';` and render `<Certifications />` after `<Skills />`.

**Step 3: Commit**

```bash
git add src/components/Certifications.astro src/pages/index.astro
git commit -m "feat: add Certifications section with placeholder cards"
```

---

### Task 12: Projects overview section

**Files:**
- Create: `src/components/ProjectCard.astro`
- Create: `src/components/ProjectsSection.astro`
- Modify: `src/pages/index.astro`

**Step 1: Write `src/components/ProjectCard.astro`**

```astro
---
interface Props {
  title: string;
  subtitle: string;
  description: string;
  tags: string[];
  slug: string;
  badge?: string;
  image: string;
}

const { title, subtitle, description, tags, slug, badge, image } = Astro.props;
---

<article class="project-card">
  <div class="project-image">
    <img src={image} alt={title} width="400" height="225" loading="lazy" />
    {badge && <span class="project-badge">{badge}</span>}
  </div>
  <div class="project-body">
    <h3 class="project-title">{title}</h3>
    <p class="project-subtitle">{subtitle}</p>
    <p class="project-desc">{description}</p>
    <div class="project-tags">
      {tags.map((tag) => (
        <span class="tag">{tag}</span>
      ))}
    </div>
    <a href={`/projects/${slug}`} class="project-link">
      View Details <span aria-hidden="true">&rarr;</span>
    </a>
  </div>
</article>

<style>
  .project-card {
    background: var(--color-surface);
    border-radius: var(--radius);
    overflow: hidden;
    box-shadow: var(--shadow-sm);
    transition: all var(--transition);
    display: flex;
    flex-direction: column;
  }

  .project-card:hover {
    transform: translateY(-6px);
    box-shadow: var(--shadow-lg);
  }

  .project-image {
    position: relative;
    width: 100%;
    height: 200px;
    background: var(--color-primary-light);
    overflow: hidden;
  }

  .project-image img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  .project-badge {
    position: absolute;
    top: var(--space-3);
    right: var(--space-3);
    background: var(--color-accent);
    color: white;
    font-size: 0.75rem;
    font-weight: 600;
    padding: var(--space-1) var(--space-3);
    border-radius: var(--radius-full);
  }

  .project-body {
    padding: var(--space-6);
    display: flex;
    flex-direction: column;
    flex-grow: 1;
  }

  .project-title {
    font-size: 1.2rem;
    font-weight: 700;
    margin-bottom: var(--space-1);
  }

  .project-subtitle {
    font-size: 0.9rem;
    color: var(--color-text-secondary);
    margin-bottom: var(--space-3);
  }

  .project-desc {
    font-size: 0.9rem;
    color: var(--color-text-secondary);
    line-height: 1.6;
    margin-bottom: var(--space-4);
    display: -webkit-box;
    -webkit-line-clamp: 3;
    -webkit-box-orient: vertical;
    overflow: hidden;
  }

  .project-tags {
    display: flex;
    flex-wrap: wrap;
    gap: var(--space-2);
    margin-bottom: var(--space-4);
    margin-top: auto;
  }

  .project-link {
    font-size: 0.95rem;
    font-weight: 600;
    color: var(--color-primary-dark);
    transition: color var(--transition);
  }

  .project-link:hover {
    color: var(--color-primary);
  }
</style>
```

**Step 2: Write `src/components/ProjectsSection.astro`**

```astro
---
import ProjectCard from './ProjectCard.astro';
import projectsData from '../data/projects.json';
---

<section class="section" id="projects">
  <div class="container">
    <h2 class="section-title reveal">Projects</h2>
    <p class="section-subtitle reveal">Some of the things I've built</p>
    <div class="projects-grid">
      {projectsData.map((project) => (
        <div class="reveal">
          <ProjectCard
            title={project.title}
            subtitle={project.subtitle}
            description={project.description}
            tags={project.tags}
            slug={project.slug}
            badge={project.badge}
            image={project.image}
          />
        </div>
      ))}
    </div>
  </div>
</section>

<style>
  .projects-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: var(--space-6);
  }

  @media (max-width: 1024px) {
    .projects-grid {
      grid-template-columns: repeat(2, 1fr);
    }
  }

  @media (max-width: 768px) {
    .projects-grid {
      grid-template-columns: 1fr;
    }
  }
</style>
```

**Step 3: Update `src/pages/index.astro`**

Add `import ProjectsSection from '../components/ProjectsSection.astro';` and render `<ProjectsSection />` after `<Certifications />`.

**Step 4: Commit**

```bash
git add src/components/ProjectCard.astro src/components/ProjectsSection.astro src/pages/index.astro
git commit -m "feat: add Projects overview section with cards linking to detail pages"
```

---

### Task 13: Contact section

**Files:**
- Create: `src/components/Contact.astro`
- Modify: `src/pages/index.astro`

**Step 1: Write `src/components/Contact.astro`**

```astro
---
const contactLinks = [
  {
    label: 'GitHub',
    href: 'https://github.com/christinandrea',
    icon: '🔗',
    display: 'github.com/christinandrea',
  },
  {
    label: 'LinkedIn',
    href: '#',
    icon: '💼',
    display: 'LinkedIn Profile',
  },
  {
    label: 'Email',
    href: 'mailto:your@email.com',
    icon: '✉️',
    display: 'your@email.com',
  },
];
---

<section class="contact-section" id="contact">
  <div class="container">
    <h2 class="section-title reveal">Get in Touch</h2>
    <p class="section-subtitle reveal">Have a question or want to work together? Drop me a message!</p>
    <div class="contact-grid reveal">
      <form class="contact-form" action="https://api.web3forms.com/submit" method="POST">
        <!-- Replace YOUR_ACCESS_KEY with your Web3Forms access key -->
        <input type="hidden" name="access_key" value="YOUR_ACCESS_KEY" />
        <input type="hidden" name="subject" value="New portfolio contact" />

        <div class="form-group">
          <label for="name">Name</label>
          <input type="text" id="name" name="name" required placeholder="Your name" />
        </div>

        <div class="form-group">
          <label for="email">Email</label>
          <input type="email" id="email" name="email" required placeholder="your@email.com" />
        </div>

        <div class="form-group">
          <label for="message">Message</label>
          <textarea id="message" name="message" rows="5" required placeholder="Your message..."></textarea>
        </div>

        <button type="submit" class="btn btn-primary">Send Message</button>
      </form>

      <div class="contact-info">
        <h3 class="contact-info-title">Let's connect</h3>
        <p class="contact-info-text">
          Feel free to reach out through any of these channels.
          I'll get back to you as soon as possible.
        </p>
        <div class="contact-links">
          {contactLinks.map((link) => (
            <a href={link.href} class="contact-link-item" target="_blank" rel="noopener noreferrer">
              <span class="contact-link-icon">{link.icon}</span>
              <div>
                <span class="contact-link-label">{link.label}</span>
                <span class="contact-link-display">{link.display}</span>
              </div>
            </a>
          ))}
        </div>
      </div>
    </div>
  </div>
</section>

<style>
  .contact-section {
    padding: var(--space-24) 0;
    background: linear-gradient(180deg, var(--color-bg) 0%, var(--color-primary-light) 100%);
  }

  .contact-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: var(--space-12);
    align-items: start;
  }

  .contact-form {
    display: flex;
    flex-direction: column;
    gap: var(--space-4);
  }

  .form-group {
    display: flex;
    flex-direction: column;
    gap: var(--space-2);
  }

  .form-group label {
    font-size: 0.9rem;
    font-weight: 500;
    color: var(--color-text);
  }

  .form-group input,
  .form-group textarea {
    padding: var(--space-3) var(--space-4);
    border: 2px solid var(--color-primary-light);
    border-radius: var(--radius-sm);
    font-family: var(--font-body);
    font-size: 0.95rem;
    background: var(--color-surface);
    color: var(--color-text);
    transition: border-color var(--transition);
    outline: none;
  }

  .form-group input:focus,
  .form-group textarea:focus {
    border-color: var(--color-primary);
  }

  .form-group textarea {
    resize: vertical;
  }

  .contact-info {
    padding: var(--space-8);
    background: var(--color-surface);
    border-radius: var(--radius);
    box-shadow: var(--shadow-sm);
  }

  .contact-info-title {
    font-size: 1.3rem;
    font-weight: 700;
    margin-bottom: var(--space-3);
  }

  .contact-info-text {
    font-size: 0.95rem;
    color: var(--color-text-secondary);
    line-height: 1.6;
    margin-bottom: var(--space-6);
  }

  .contact-links {
    display: flex;
    flex-direction: column;
    gap: var(--space-4);
  }

  .contact-link-item {
    display: flex;
    align-items: center;
    gap: var(--space-4);
    padding: var(--space-3) var(--space-4);
    border-radius: var(--radius-sm);
    transition: background var(--transition);
  }

  .contact-link-item:hover {
    background: var(--color-primary-light);
  }

  .contact-link-icon {
    font-size: 1.5rem;
    width: 40px;
    text-align: center;
  }

  .contact-link-label {
    display: block;
    font-weight: 600;
    font-size: 0.9rem;
  }

  .contact-link-display {
    display: block;
    font-size: 0.85rem;
    color: var(--color-text-secondary);
  }

  @media (max-width: 768px) {
    .contact-grid {
      grid-template-columns: 1fr;
    }
  }
</style>
```

**Step 2: Update `src/pages/index.astro`**

Add `import Contact from '../components/Contact.astro';` and render `<Contact />` after `<ProjectsSection />`.

**Step 3: Final `src/pages/index.astro` should now look like:**

```astro
---
import BaseLayout from '../layouts/BaseLayout.astro';
import Hero from '../components/Hero.astro';
import About from '../components/About.astro';
import Experience from '../components/Experience.astro';
import Skills from '../components/Skills.astro';
import Certifications from '../components/Certifications.astro';
import ProjectsSection from '../components/ProjectsSection.astro';
import Contact from '../components/Contact.astro';
---

<BaseLayout>
  <main>
    <Hero />
    <About />
    <Experience />
    <Skills />
    <Certifications />
    <ProjectsSection />
    <Contact />
  </main>
</BaseLayout>
```

**Step 4: Commit**

```bash
git add src/components/Contact.astro src/pages/index.astro
git commit -m "feat: add Contact section with Web3Forms form and social links"
```

---

## Phase 4: Project Detail Pages

### Task 14: Project detail page — Fiori Training

**Files:**
- Create: `src/pages/projects/fiori-training.astro`

**Step 1: Create `src/pages/projects/` directory and write the file**

```astro
---
import BaseLayout from '../../layouts/BaseLayout.astro';

const project = {
  title: 'SAP Fiori Airlines Management',
  subtitle: 'Enterprise CRUD application built with SAP UI5',
  description: `A full-stack SAP Fiori application for managing airlines, built using the SAP UI5 framework.
    The app demonstrates complete CRUD operations with advanced filtering, responsive Object Page layouts,
    and OData 2.0 service integration. It features a home view with a searchable/filterable carriers list,
    a create form with validation, and a detailed carrier profile page showing connections and flight schedules.`,
  tags: ['SAP UI5', 'OData 2.0', 'JavaScript', 'XML Views', 'SAPUI5'],
  features: [
    'Dynamic filtering by Carrier ID and Currency Code with search functionality',
    'Carrier detail page using Object Page Layout with connections and flights tables',
    'Create form with input validation and user feedback',
    'Responsive design working across desktop, tablet, and mobile',
    'Two-way data binding for forms, one-way for read-only views',
    'Internationalization (i18n) support for multi-language readiness',
    'Mock data server for offline local development',
    'Unit tests (QUnit) and integration tests (OPA)',
  ],
  github: 'https://github.com/christinandrea/fiori-training',
  prev: { slug: 'carbon-prediction', title: 'Carbon Prediction' },
  next: { slug: 'groovys-script', title: 'CPI Auth Scripts' },
};
---

<BaseLayout title={`${project.title} — Christina Andrea Putri`}>
  <main class="project-detail">
    <section class="project-hero">
      <div class="container">
        <a href="/#projects" class="back-link">&larr; Back to Projects</a>
        <h1 class="project-detail-title">{project.title}</h1>
        <p class="project-detail-subtitle">{project.subtitle}</p>
        <div class="project-detail-tags">
          {project.tags.map((tag) => (
            <span class="tag">{tag}</span>
          ))}
        </div>
      </div>
    </section>

    <section class="section">
      <div class="container project-content">
        <div class="project-description reveal">
          <h2>About This Project</h2>
          <p>{project.description}</p>
        </div>

        <div class="project-features reveal">
          <h2>Key Features</h2>
          <ul class="features-list">
            {project.features.map((feature) => (
              <li>
                <span class="feature-check">✓</span>
                {feature}
              </li>
            ))}
          </ul>
        </div>

        <div class="project-screenshots reveal">
          <h2>Screenshots</h2>
          <div class="screenshots-placeholder">
            <p>Screenshots coming soon</p>
          </div>
        </div>

        <div class="project-cta reveal">
          <a href={project.github} target="_blank" rel="noopener noreferrer" class="btn btn-primary">
            View on GitHub
          </a>
        </div>

        <nav class="project-nav reveal">
          <a href={`/projects/${project.prev.slug}`} class="project-nav-link prev">
            <span class="nav-direction">&larr; Previous</span>
            <span class="nav-title">{project.prev.title}</span>
          </a>
          <a href={`/projects/${project.next.slug}`} class="project-nav-link next">
            <span class="nav-direction">Next &rarr;</span>
            <span class="nav-title">{project.next.title}</span>
          </a>
        </nav>
      </div>
    </section>
  </main>
</BaseLayout>

<style>
  .project-hero {
    padding: calc(var(--space-24) + 60px) 0 var(--space-12);
    background: linear-gradient(135deg, var(--color-bg) 0%, var(--color-primary-light) 100%);
  }

  .back-link {
    display: inline-block;
    font-size: 0.9rem;
    color: var(--color-text-secondary);
    margin-bottom: var(--space-6);
    transition: color var(--transition);
  }

  .back-link:hover {
    color: var(--color-primary-dark);
  }

  .project-detail-title {
    font-size: 2.5rem;
    font-weight: 700;
    margin-bottom: var(--space-3);
  }

  .project-detail-subtitle {
    font-size: 1.2rem;
    color: var(--color-text-secondary);
    margin-bottom: var(--space-6);
  }

  .project-detail-tags {
    display: flex;
    flex-wrap: wrap;
    gap: var(--space-2);
  }

  .project-content {
    max-width: 800px;
  }

  .project-content h2 {
    font-size: 1.5rem;
    font-weight: 700;
    margin-bottom: var(--space-4);
    color: var(--color-text);
  }

  .project-description {
    margin-bottom: var(--space-12);
  }

  .project-description p {
    font-size: 1.05rem;
    line-height: 1.8;
    color: var(--color-text-secondary);
  }

  .project-features {
    margin-bottom: var(--space-12);
  }

  .features-list {
    display: flex;
    flex-direction: column;
    gap: var(--space-3);
  }

  .features-list li {
    display: flex;
    align-items: flex-start;
    gap: var(--space-3);
    font-size: 1rem;
    line-height: 1.6;
    color: var(--color-text-secondary);
  }

  .feature-check {
    color: var(--color-primary-dark);
    font-weight: 700;
    flex-shrink: 0;
    margin-top: 2px;
  }

  .project-screenshots {
    margin-bottom: var(--space-12);
  }

  .screenshots-placeholder {
    background: var(--color-primary-light);
    border-radius: var(--radius);
    padding: var(--space-16);
    text-align: center;
    color: var(--color-text-secondary);
  }

  .project-cta {
    margin-bottom: var(--space-16);
  }

  .project-nav {
    display: flex;
    justify-content: space-between;
    border-top: 1px solid var(--color-primary-light);
    padding-top: var(--space-8);
  }

  .project-nav-link {
    display: flex;
    flex-direction: column;
    gap: var(--space-1);
    padding: var(--space-3) var(--space-4);
    border-radius: var(--radius-sm);
    transition: background var(--transition);
  }

  .project-nav-link:hover {
    background: var(--color-primary-light);
  }

  .project-nav-link.next {
    text-align: right;
    margin-left: auto;
  }

  .nav-direction {
    font-size: 0.85rem;
    color: var(--color-text-secondary);
  }

  .nav-title {
    font-weight: 600;
    color: var(--color-primary-dark);
  }

  @media (max-width: 768px) {
    .project-detail-title {
      font-size: 1.8rem;
    }

    .project-nav {
      flex-direction: column;
      gap: var(--space-4);
    }

    .project-nav-link.next {
      text-align: left;
    }
  }
</style>
```

**Step 2: Commit**

```bash
git add src/pages/projects/fiori-training.astro
git commit -m "feat: add Fiori Training project detail page"
```

---

### Task 15: Project detail page — Groovys Script

**Files:**
- Create: `src/pages/projects/groovys-script.astro`

**Step 1: Write the file**

Same structure as fiori-training.astro but with groovys-script data plus a code snippet section. Copy the entire structure from Task 14 and change:

```astro
---
import BaseLayout from '../../layouts/BaseLayout.astro';

const project = {
  title: 'SAP CPI Authentication Scripts',
  subtitle: 'OAuth & digital signature flows for SAP Cloud Integration',
  description: `A collection of Groovy scripts designed for SAP Cloud Platform Integration (CPI)
    authentication and API communication flows. These scripts implement OAuth-style authentication
    with digital signature support, enabling secure service-to-service communication within
    the SAP integration landscape.`,
  tags: ['Groovy', 'SAP CPI', 'OAuth', 'JSON', 'Java'],
  features: [
    'Pre-authentication flow extracting timestamps, client keys, and private keys from headers',
    'Access token extraction with HTTP status validation and expiry calculation',
    'Digital signature service integration for secure request signing',
    'Masked logging — sensitive tokens show only first 10 characters for security',
    'Stream-based JSON parsing to handle large payloads efficiently',
    'Property propagation between integration flow steps via message properties',
    'Error handling with HTTP status code validation and detailed error wrapping',
  ],
  github: 'https://github.com/christinandrea/groovys-script',
  prev: { slug: 'fiori-training', title: 'Fiori Airlines' },
  next: { slug: 'carbon-prediction', title: 'Carbon Prediction' },
  codeSnippet: `import com.sap.gateway.ip.core.customdev.util.Message
import groovy.json.JsonSlurper
import groovy.json.JsonOutput

def Message processData(Message message) {
    def body = message.getBody(java.io.Reader)
    def jsonSlurper = new JsonSlurper()
    def json = jsonSlurper.parse(body)

    def accessToken = json.accessToken
    def tokenType = json.tokenType ?: "Bearer"
    def expiresIn = json.expiresIn

    // Calculate token expiry epoch
    def now = System.currentTimeMillis() / 1000
    def expiryEpoch = now + expiresIn

    // Mask token for secure logging
    def maskedToken = accessToken?.take(10) + "..."
    message.setProperty("logToken", maskedToken)

    message.setProperty("accessToken", accessToken)
    message.setProperty("tokenType", tokenType)
    message.setProperty("tokenExpiry", expiryEpoch)

    return message
}`,
};
---

<BaseLayout title={`${project.title} — Christina Andrea Putri`}>
  <main class="project-detail">
    <section class="project-hero">
      <div class="container">
        <a href="/#projects" class="back-link">&larr; Back to Projects</a>
        <h1 class="project-detail-title">{project.title}</h1>
        <p class="project-detail-subtitle">{project.subtitle}</p>
        <div class="project-detail-tags">
          {project.tags.map((tag) => (
            <span class="tag">{tag}</span>
          ))}
        </div>
      </div>
    </section>

    <section class="section">
      <div class="container project-content">
        <div class="project-description reveal">
          <h2>About This Project</h2>
          <p>{project.description}</p>
        </div>

        <div class="project-features reveal">
          <h2>Key Features</h2>
          <ul class="features-list">
            {project.features.map((feature) => (
              <li>
                <span class="feature-check">✓</span>
                {feature}
              </li>
            ))}
          </ul>
        </div>

        <div class="code-showcase reveal">
          <h2>Code Spotlight</h2>
          <p class="code-caption">Token extraction with masked logging for secure audit trails:</p>
          <pre class="code-block"><code>{project.codeSnippet}</code></pre>
        </div>

        <div class="project-cta reveal">
          <a href={project.github} target="_blank" rel="noopener noreferrer" class="btn btn-primary">
            View on GitHub
          </a>
        </div>

        <nav class="project-nav reveal">
          <a href={`/projects/${project.prev.slug}`} class="project-nav-link prev">
            <span class="nav-direction">&larr; Previous</span>
            <span class="nav-title">{project.prev.title}</span>
          </a>
          <a href={`/projects/${project.next.slug}`} class="project-nav-link next">
            <span class="nav-direction">Next &rarr;</span>
            <span class="nav-title">{project.next.title}</span>
          </a>
        </nav>
      </div>
    </section>
  </main>
</BaseLayout>

<style>
  .project-hero {
    padding: calc(var(--space-24) + 60px) 0 var(--space-12);
    background: linear-gradient(135deg, var(--color-bg) 0%, var(--color-primary-light) 100%);
  }

  .back-link {
    display: inline-block;
    font-size: 0.9rem;
    color: var(--color-text-secondary);
    margin-bottom: var(--space-6);
    transition: color var(--transition);
  }

  .back-link:hover {
    color: var(--color-primary-dark);
  }

  .project-detail-title {
    font-size: 2.5rem;
    font-weight: 700;
    margin-bottom: var(--space-3);
  }

  .project-detail-subtitle {
    font-size: 1.2rem;
    color: var(--color-text-secondary);
    margin-bottom: var(--space-6);
  }

  .project-detail-tags {
    display: flex;
    flex-wrap: wrap;
    gap: var(--space-2);
  }

  .project-content {
    max-width: 800px;
  }

  .project-content h2 {
    font-size: 1.5rem;
    font-weight: 700;
    margin-bottom: var(--space-4);
    color: var(--color-text);
  }

  .project-description {
    margin-bottom: var(--space-12);
  }

  .project-description p {
    font-size: 1.05rem;
    line-height: 1.8;
    color: var(--color-text-secondary);
  }

  .project-features {
    margin-bottom: var(--space-12);
  }

  .features-list {
    display: flex;
    flex-direction: column;
    gap: var(--space-3);
  }

  .features-list li {
    display: flex;
    align-items: flex-start;
    gap: var(--space-3);
    font-size: 1rem;
    line-height: 1.6;
    color: var(--color-text-secondary);
  }

  .feature-check {
    color: var(--color-primary-dark);
    font-weight: 700;
    flex-shrink: 0;
    margin-top: 2px;
  }

  .code-showcase {
    margin-bottom: var(--space-12);
  }

  .code-caption {
    font-size: 0.95rem;
    color: var(--color-text-secondary);
    margin-bottom: var(--space-4);
  }

  .code-block {
    background: #2D2D2D;
    color: #F8F8F2;
    border-radius: var(--radius);
    padding: var(--space-6);
    overflow-x: auto;
    font-family: var(--font-mono);
    font-size: 0.85rem;
    line-height: 1.7;
  }

  .project-cta {
    margin-bottom: var(--space-16);
  }

  .project-nav {
    display: flex;
    justify-content: space-between;
    border-top: 1px solid var(--color-primary-light);
    padding-top: var(--space-8);
  }

  .project-nav-link {
    display: flex;
    flex-direction: column;
    gap: var(--space-1);
    padding: var(--space-3) var(--space-4);
    border-radius: var(--radius-sm);
    transition: background var(--transition);
  }

  .project-nav-link:hover {
    background: var(--color-primary-light);
  }

  .project-nav-link.next {
    text-align: right;
    margin-left: auto;
  }

  .nav-direction {
    font-size: 0.85rem;
    color: var(--color-text-secondary);
  }

  .nav-title {
    font-weight: 600;
    color: var(--color-primary-dark);
  }

  @media (max-width: 768px) {
    .project-detail-title {
      font-size: 1.8rem;
    }

    .project-nav {
      flex-direction: column;
      gap: var(--space-4);
    }

    .project-nav-link.next {
      text-align: left;
    }
  }
</style>
```

**Step 2: Commit**

```bash
git add src/pages/projects/groovys-script.astro
git commit -m "feat: add Groovys Script project detail page with code showcase"
```

---

### Task 16: Project detail page — Carbon Prediction

**Files:**
- Create: `src/pages/projects/carbon-prediction.astro`

**Step 1: Write the file**

Same base structure, but adds: internship badge, metrics section, pipeline visualization, architecture diagram placeholder.

```astro
---
import BaseLayout from '../../layouts/BaseLayout.astro';

const project = {
  title: 'Carbon Prediction from Drone Imagery',
  subtitle: 'Machine learning internship project',
  badge: 'Internship Project',
  description: `An ML pipeline that predicts carbon values from drone-captured Near-Infrared (NIR)
    images. The system uses computer vision techniques (FAST keypoint detection + SIFT descriptor
    extraction) combined with a neural network to estimate carbon concentrations from cropped
    30×30 pixel image regions. The model was trained and validated on real drone data and deployed
    as a serverless inference endpoint on AWS Lambda.`,
  tags: ['Python', 'TensorFlow', 'OpenCV', 'AWS Lambda', 'Docker', 'Scikit-learn'],
  features: [
    'FAST (Features from Accelerated Segment Test) keypoint detection',
    'SIFT (Scale-Invariant Feature Transform) descriptor extraction',
    'CLAHE (Contrast Limited Adaptive Histogram Equalization) image enhancement',
    'Neural network model trained with Keras/TensorFlow',
    'MaxAbsScaler feature normalization pipeline',
    'AWS Lambda serverless inference with Docker containerization',
    'S3 integration for image storage and retrieval',
    'API Gateway endpoint for real-time predictions',
  ],
  github: 'https://github.com/christinandrea/isai-carbon-prediction-2024',
  prev: { slug: 'groovys-script', title: 'CPI Auth Scripts' },
  next: { slug: 'fiori-training', title: 'Fiori Airlines' },
  metrics: {
    mape: '10%',
    r2: '0.75',
    bestHeight: '25m',
    bestError: '6.77%',
  },
};

const pipelineSteps = [
  { step: '1', label: 'Image Acquisition', detail: 'Grayscale NIR from drones' },
  { step: '2', label: 'Preprocessing', detail: 'Center crop 30×30 + CLAHE' },
  { step: '3', label: 'Feature Extraction', detail: 'FAST keypoints + SIFT descriptors' },
  { step: '4', label: 'Normalization', detail: 'MaxAbsScaler transformation' },
  { step: '5', label: 'Prediction', detail: 'Neural network inference' },
];
---

<BaseLayout title={`${project.title} — Christina Andrea Putri`}>
  <main class="project-detail">
    <section class="project-hero">
      <div class="container">
        <a href="/#projects" class="back-link">&larr; Back to Projects</a>
        <div class="badge-row">
          <span class="internship-badge">{project.badge}</span>
        </div>
        <h1 class="project-detail-title">{project.title}</h1>
        <p class="project-detail-subtitle">{project.subtitle}</p>
        <div class="project-detail-tags">
          {project.tags.map((tag) => (
            <span class="tag">{tag}</span>
          ))}
        </div>
      </div>
    </section>

    <section class="section">
      <div class="container project-content">
        <div class="project-description reveal">
          <h2>About This Project</h2>
          <p>{project.description}</p>
        </div>

        <div class="metrics-section reveal">
          <h2>Model Performance</h2>
          <div class="metrics-grid">
            <div class="metric-card">
              <span class="metric-value">{project.metrics.mape}</span>
              <span class="metric-label">MAPE</span>
            </div>
            <div class="metric-card">
              <span class="metric-value">{project.metrics.r2}</span>
              <span class="metric-label">R² Score</span>
            </div>
            <div class="metric-card">
              <span class="metric-value">{project.metrics.bestHeight}</span>
              <span class="metric-label">Best Height</span>
            </div>
            <div class="metric-card">
              <span class="metric-value">{project.metrics.bestError}</span>
              <span class="metric-label">Best Error Rate</span>
            </div>
          </div>
        </div>

        <div class="pipeline-section reveal">
          <h2>Processing Pipeline</h2>
          <div class="pipeline">
            {pipelineSteps.map((item, index) => (
              <div class="pipeline-step">
                <div class="pipeline-number">{item.step}</div>
                <div class="pipeline-info">
                  <span class="pipeline-label">{item.label}</span>
                  <span class="pipeline-detail">{item.detail}</span>
                </div>
                {index < pipelineSteps.length - 1 && (
                  <div class="pipeline-arrow">→</div>
                )}
              </div>
            ))}
          </div>
        </div>

        <div class="project-features reveal">
          <h2>Key Features</h2>
          <ul class="features-list">
            {project.features.map((feature) => (
              <li>
                <span class="feature-check">✓</span>
                {feature}
              </li>
            ))}
          </ul>
        </div>

        <div class="architecture-section reveal">
          <h2>AWS Serverless Architecture</h2>
          <div class="architecture-placeholder">
            <img src="/images/serverless-architecture.png" alt="AWS serverless architecture diagram" loading="lazy" />
          </div>
        </div>

        <div class="project-cta reveal">
          <a href={project.github} target="_blank" rel="noopener noreferrer" class="btn btn-primary">
            View on GitHub
          </a>
        </div>

        <nav class="project-nav reveal">
          <a href={`/projects/${project.prev.slug}`} class="project-nav-link prev">
            <span class="nav-direction">&larr; Previous</span>
            <span class="nav-title">{project.prev.title}</span>
          </a>
          <a href={`/projects/${project.next.slug}`} class="project-nav-link next">
            <span class="nav-direction">Next &rarr;</span>
            <span class="nav-title">{project.next.title}</span>
          </a>
        </nav>
      </div>
    </section>
  </main>
</BaseLayout>

<style>
  .project-hero {
    padding: calc(var(--space-24) + 60px) 0 var(--space-12);
    background: linear-gradient(135deg, var(--color-bg) 0%, var(--color-primary-light) 100%);
  }

  .back-link {
    display: inline-block;
    font-size: 0.9rem;
    color: var(--color-text-secondary);
    margin-bottom: var(--space-4);
    transition: color var(--transition);
  }

  .back-link:hover {
    color: var(--color-primary-dark);
  }

  .badge-row {
    margin-bottom: var(--space-4);
  }

  .internship-badge {
    display: inline-block;
    background: var(--color-accent);
    color: white;
    font-size: 0.85rem;
    font-weight: 600;
    padding: var(--space-1) var(--space-4);
    border-radius: var(--radius-full);
  }

  .project-detail-title {
    font-size: 2.5rem;
    font-weight: 700;
    margin-bottom: var(--space-3);
  }

  .project-detail-subtitle {
    font-size: 1.2rem;
    color: var(--color-text-secondary);
    margin-bottom: var(--space-6);
  }

  .project-detail-tags {
    display: flex;
    flex-wrap: wrap;
    gap: var(--space-2);
  }

  .project-content {
    max-width: 800px;
  }

  .project-content h2 {
    font-size: 1.5rem;
    font-weight: 700;
    margin-bottom: var(--space-4);
    color: var(--color-text);
  }

  .project-description {
    margin-bottom: var(--space-12);
  }

  .project-description p {
    font-size: 1.05rem;
    line-height: 1.8;
    color: var(--color-text-secondary);
  }

  /* Metrics */
  .metrics-section {
    margin-bottom: var(--space-12);
  }

  .metrics-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: var(--space-4);
  }

  .metric-card {
    background: var(--color-surface);
    border-radius: var(--radius);
    padding: var(--space-6);
    text-align: center;
    box-shadow: var(--shadow-sm);
    border: 1px solid var(--color-primary-light);
  }

  .metric-value {
    display: block;
    font-family: var(--font-heading);
    font-size: 1.8rem;
    font-weight: 700;
    color: var(--color-primary-dark);
    margin-bottom: var(--space-1);
  }

  .metric-label {
    font-size: 0.85rem;
    color: var(--color-text-secondary);
    font-weight: 500;
  }

  /* Pipeline */
  .pipeline-section {
    margin-bottom: var(--space-12);
  }

  .pipeline {
    display: flex;
    align-items: flex-start;
    gap: var(--space-2);
    flex-wrap: wrap;
    justify-content: center;
  }

  .pipeline-step {
    display: flex;
    align-items: center;
    gap: var(--space-3);
  }

  .pipeline-number {
    width: 36px;
    height: 36px;
    border-radius: 50%;
    background: var(--color-primary);
    color: white;
    display: flex;
    align-items: center;
    justify-content: center;
    font-weight: 700;
    font-size: 0.9rem;
    flex-shrink: 0;
  }

  .pipeline-info {
    display: flex;
    flex-direction: column;
  }

  .pipeline-label {
    font-weight: 600;
    font-size: 0.9rem;
  }

  .pipeline-detail {
    font-size: 0.8rem;
    color: var(--color-text-secondary);
  }

  .pipeline-arrow {
    color: var(--color-primary);
    font-size: 1.2rem;
    font-weight: 700;
    margin: 0 var(--space-2);
  }

  /* Features */
  .project-features {
    margin-bottom: var(--space-12);
  }

  .features-list {
    display: flex;
    flex-direction: column;
    gap: var(--space-3);
  }

  .features-list li {
    display: flex;
    align-items: flex-start;
    gap: var(--space-3);
    font-size: 1rem;
    line-height: 1.6;
    color: var(--color-text-secondary);
  }

  .feature-check {
    color: var(--color-primary-dark);
    font-weight: 700;
    flex-shrink: 0;
    margin-top: 2px;
  }

  /* Architecture */
  .architecture-section {
    margin-bottom: var(--space-12);
  }

  .architecture-placeholder {
    background: var(--color-primary-light);
    border-radius: var(--radius);
    padding: var(--space-8);
    text-align: center;
    min-height: 200px;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .architecture-placeholder img {
    max-width: 100%;
    border-radius: var(--radius-sm);
  }

  .project-cta {
    margin-bottom: var(--space-16);
  }

  .project-nav {
    display: flex;
    justify-content: space-between;
    border-top: 1px solid var(--color-primary-light);
    padding-top: var(--space-8);
  }

  .project-nav-link {
    display: flex;
    flex-direction: column;
    gap: var(--space-1);
    padding: var(--space-3) var(--space-4);
    border-radius: var(--radius-sm);
    transition: background var(--transition);
  }

  .project-nav-link:hover {
    background: var(--color-primary-light);
  }

  .project-nav-link.next {
    text-align: right;
    margin-left: auto;
  }

  .nav-direction {
    font-size: 0.85rem;
    color: var(--color-text-secondary);
  }

  .nav-title {
    font-weight: 600;
    color: var(--color-primary-dark);
  }

  @media (max-width: 768px) {
    .project-detail-title {
      font-size: 1.8rem;
    }

    .metrics-grid {
      grid-template-columns: repeat(2, 1fr);
    }

    .pipeline {
      flex-direction: column;
      align-items: flex-start;
    }

    .pipeline-arrow {
      transform: rotate(90deg);
      margin: var(--space-1) 0 var(--space-1) var(--space-4);
    }

    .project-nav {
      flex-direction: column;
      gap: var(--space-4);
    }

    .project-nav-link.next {
      text-align: left;
    }
  }
</style>
```

**Step 2: Commit**

```bash
git add src/pages/projects/carbon-prediction.astro
git commit -m "feat: add Carbon Prediction project detail page with metrics and pipeline"
```

---

## Phase 5: Polish & Deploy

### Task 17: Add favicon and placeholder images

**Files:**
- Create: `public/favicon.svg`
- Create: `public/images/profile.jpg` (placeholder SVG)
- Create: `public/images/fiori-training.png` (placeholder SVG)
- Create: `public/images/groovys-script.png` (placeholder SVG)
- Create: `public/images/carbon-prediction.png` (placeholder SVG)

**Step 1: Create `public/favicon.svg`**

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 32 32">
  <rect width="32" height="32" rx="8" fill="#F4A0B5"/>
  <text x="16" y="22" text-anchor="middle" font-family="sans-serif" font-weight="700" font-size="16" fill="white">C</text>
</svg>
```

**Step 2: Create placeholder SVGs for images**

For each image (`profile.jpg`, `fiori-training.png`, `groovys-script.png`, `carbon-prediction.png`), create a simple placeholder SVG or a 1x1 transparent pixel. Use SVG files with `.png`/`.jpg` extension for now — the user will replace them with real images later.

Actually, create proper placeholder SVGs as `.svg` files and update the JSON references. OR create minimal placeholder PNGs.

**Simpler approach:** Create a single `public/images/placeholder.svg` and update the data files to reference it temporarily. Then create the real image placeholders:

`public/images/placeholder.svg`:
```svg
<svg xmlns="http://www.w3.org/2000/svg" width="400" height="225" viewBox="0 0 400 225">
  <rect width="400" height="225" fill="#FDE2E8"/>
  <text x="200" y="120" text-anchor="middle" font-family="sans-serif" font-size="16" fill="#D4708F">Image Coming Soon</text>
</svg>
```

Then update `src/data/projects.json` to use `/images/placeholder.svg` for all image fields.

Also create `public/images/profile.svg` as profile placeholder:
```svg
<svg xmlns="http://www.w3.org/2000/svg" width="300" height="300" viewBox="0 0 300 300">
  <rect width="300" height="300" fill="#FDE2E8"/>
  <circle cx="150" cy="120" r="50" fill="#F4A0B5"/>
  <ellipse cx="150" cy="250" rx="80" ry="60" fill="#F4A0B5"/>
</svg>
```

And update `src/components/About.astro` to reference `/images/profile.svg` instead of `/images/profile.jpg`.

**Step 3: Commit**

```bash
git add public/ src/data/projects.json src/components/About.astro
git commit -m "feat: add favicon and placeholder images"
```

---

### Task 18: GitHub Actions deploy workflow

**Files:**
- Create: `.github/workflows/deploy.yml`

**Step 1: Create directory and write the file**

```bash
mkdir -p .github/workflows
```

`.github/workflows/deploy.yml`:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: npm
      - run: npm ci
      - run: npm run build
      - uses: actions/upload-pages-artifact@v3
        with:
          path: dist

  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - id: deployment
        uses: actions/deploy-pages@v4
```

**Step 2: Verify build succeeds locally**

```bash
npm run build
```

Expected: `dist/` folder generated with static HTML files, no errors.

**Step 3: Commit**

```bash
git add .github/workflows/deploy.yml
git commit -m "ci: add GitHub Actions workflow for Pages deployment"
```

---

### Task 19: Final build verification and smoke test

**Step 1: Full build**

```bash
npm run build
```

Expected: Clean build, no errors or warnings.

**Step 2: Preview production build**

```bash
npm run preview
```

Open `localhost:4321` and run through this checklist:

**Step 3: Smoke test checklist**

- [ ] Pearl background (#FFF8F9) visible
- [ ] Fonts loaded (Poppins headings, Inter body)
- [ ] Navbar sticky at top, glass-morphism on scroll
- [ ] Mobile hamburger menu opens/closes at narrow viewport
- [ ] Hero section: full viewport, gradient animation, floating shapes
- [ ] Hero CTAs link to #projects and #contact
- [ ] About section: profile image area, bio text, stats row
- [ ] Experience section: timeline with dot markers, cards render
- [ ] Skills section: 5 category cards with pill badges
- [ ] Certifications section: placeholder cards visible
- [ ] Projects section: 3 cards in grid, "Internship Project" badge on carbon prediction
- [ ] Each "View Details →" navigates to correct `/projects/` page
- [ ] Fiori Training page: features list, screenshots placeholder, GitHub link, prev/next nav
- [ ] Groovys Script page: features list, code snippet block (dark background), prev/next nav
- [ ] Carbon Prediction page: badge, metrics cards, pipeline visualization, architecture section, prev/next nav
- [ ] "Back to Projects" links return to main page #projects
- [ ] Contact form: 3 fields + submit button present
- [ ] Footer: copyright with current year, social links, "Built with Astro"
- [ ] Scroll reveal animations trigger on scroll (elements fade in)
- [ ] No console errors
- [ ] Responsive: check at 375px (mobile), 768px (tablet), 1200px+ (desktop)

**Step 4: Commit if any fixes were needed**

```bash
git add -A
git commit -m "chore: final build verification and fixes"
```

---

## Task Summary

| # | Task | Phase | Key Files |
|---|------|-------|-----------|
| 1 | Initialize Astro project | Scaffolding | `package.json`, `astro.config.mjs` |
| 2 | Design tokens & global CSS | Scaffolding | `src/styles/global.css` |
| 3 | JSON data files | Scaffolding | `src/data/*.json` |
| 4 | BaseLayout | Layout | `src/layouts/BaseLayout.astro` |
| 5 | Navbar | Layout | `src/components/Navbar.astro` |
| 6 | Footer | Layout | `src/components/Footer.astro` |
| 7 | Hero section | Main Page | `src/components/Hero.astro` |
| 8 | About section | Main Page | `src/components/About.astro` |
| 9 | Experience section | Main Page | `src/components/Experience.astro` |
| 10 | Skills section | Main Page | `src/components/Skills.astro` |
| 11 | Certifications section | Main Page | `src/components/Certifications.astro` |
| 12 | Projects overview | Main Page | `src/components/ProjectCard.astro`, `ProjectsSection.astro` |
| 13 | Contact section | Main Page | `src/components/Contact.astro` |
| 14 | Fiori Training page | Project Pages | `src/pages/projects/fiori-training.astro` |
| 15 | Groovys Script page | Project Pages | `src/pages/projects/groovys-script.astro` |
| 16 | Carbon Prediction page | Project Pages | `src/pages/projects/carbon-prediction.astro` |
| 17 | Favicon & placeholders | Polish | `public/favicon.svg`, `public/images/` |
| 18 | GitHub Actions deploy | Deploy | `.github/workflows/deploy.yml` |
| 19 | Final smoke test | Deploy | — |
