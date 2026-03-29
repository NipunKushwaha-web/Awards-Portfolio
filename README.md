# Awwwards Portfolio — 3D Interactive Developer Portfolio

> A premium, award-worthy interactive portfolio showcasing full-stack development expertise through cutting-edge 3D graphics, scroll-driven animations, and polished micro-interactions.

> Inspired by [NipunKushwaha-web/Awards-Portfolio](https://github.com/NipunKushwaha-web/Awards-Portfolio)

---

## Table of Contents

- [Project Overview](#project-overview)
- [Features](#features)
- [System Design \& Architecture](#system-design--architecture)
- [Tech Stack](#tech-stack)
- [How It Works](#how-it-works)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Future Scope](#future-scope)
- [Contributing](#contributing)
- [License](#license)

---

## Project Overview

This is an **Awwwards-level interactive 3D developer portfolio** — a production-ready web application designed to leave a lasting impression on recruiters, clients, and peers. The portfolio demonstrates mastery of modern frontend technologies through:

- **Immersive 3D visuals** — An animated planet with a golden ring rendered in real-time using Three.js
- **Cinematic scroll animations** — Every section features purpose-built GSAP animations triggered by scroll position
- **Premium micro-interactions** — Responsive hover states, magnetic cursor effects, and fluid transitions throughout
- **Editorial-grade typography** — Custom-designed fonts with staggered line reveals and typewriter-style entrances

The portfolio is architected as a **single-page application (SPA)** with no backend dependencies — ideal for deployment as a static site on Vercel, Netlify, or any CDN.

---

## Features

### 1. 3D Hero Section

An animated planet with a golden ring floating in space, rendered with React Three Fiber. The planet uses Drei's `Float` component for gentle bobbing motion and `Environment` for realistic lighting.

### 2. Animated Slide-in Navigation

Full-screen overlay navigation with GSAP timeline-controlled entrance/exit animations. The hamburger menu features animated line transitions that morph into an X on open.

### 3. Horizontal Service Marquee

A continuously scrolling word banner showcasing service keywords (Architecture, Development, APIs, Frontends, Databases, Scalability) with parallax speed variations based on scroll direction.

### 4. Sticky Services Grid

Cards that pin to the viewport while content scrolls beneath, revealing service details with staggered scroll-triggered animations.

### 5. About Section with Clip-Path Reveals

Profile image reveals using clip-path animations synchronized with text line entrances for an editorial magazine feel.

### 6. Interactive Works Showcase

Six portfolio projects displayed as cards with:

- Floating desktop preview images that follow the cursor on hover
- Hover overlays revealing project details
- Mobile-friendly preview variants

### 7. Pinned Contact Marquee

A scroll-pinned marquee section displaying an inspirational quote alongside a gold-accented CTA.

### 8. Contact Section

Animated social link icons with GSAP entrance orchestration and a clean contact information layout.

---

## System Design & Architecture

### Component Architecture

The application follows a **section-based component hierarchy**:

```
App
├── LoadingScreen          # Asset preloader with progress bar
├── Navbar                 # Overlay navigation + hamburger menu
└── Sections
    ├── Hero               # 3D canvas with Planet
    ├── ServiceSummary     # Horizontal marquee
    ├── Services           # Sticky card grid
    ├── About               # Image reveal + bio text
    ├── Works               # Project showcase grid
    ├── ContactSummary      # Pinned marquee quote
    └── Contact             # Social links + contact info
```

### Data Flow

1. **Loading Phase** — The app displays a loading screen while Three.js loads the `Planet.glb` model and all images preload
2. **Initialization** — After assets load, GSAP plugins (`ScrollTrigger`, `Observer`) are registered and smooth scroll (Lenis) is initialized
3. **User Interaction** — Scroll position drives `ScrollTrigger` animations; mouse movement affects 3D elements and cursor-follow effects
4. **Section Navigation** — `react-scroll` enables smooth anchor-based navigation between sections

### Animation System

The animation architecture is built on two pillars:

| Layer | Technology | Purpose |
|-------|------------|---------|
| **3D Graphics** | Three.js / React Three Fiber | Real-time WebGL rendering of the hero planet |
| **UI Animation** | GSAP + ScrollTrigger | Scroll-driven reveals, timeline sequences, entrance orchestration |

GSAP is configured with two key plugins:

- **`ScrollTrigger`** — Pins sections, creates scroll-based timelines, parallax effects
- **`Observer`** — Detects scroll velocity to modulate marquee speed

### Smooth Scrolling

[Lenis](https://lenis.studiofreight.com/) provides buttery-smooth scrolling with momentum, replacing the browser's native scroll behavior. It integrates with GSAP's `ScrollTrigger` via `ScrollTrigger.scrollerProxy`.

---

## Tech Stack

### Core Framework

| Technology | Version | Purpose |
|------------|---------|---------|
| [React](https://react.dev/) | 19.1.0 | UI component framework |
| [Vite](https://vitejs.dev/) | 6.3.5 | Build tool and dev server |

### Styling

| Technology | Version | Purpose |
|------------|---------|---------|
| [Tailwind CSS](https://tailwindcss.com/) | 4.1.7 | Utility-first CSS framework |
| Custom CSS | — | Design tokens, custom fonts, clip-path utilities |

### Animation

| Technology | Version | Purpose |
|------------|---------|---------|
| [GSAP](https://greensock.com/gsap/) | 3.13.0 | Animation platform |
| [@gsap/react](https://github.com/akx/gsap-react) | 2.1.2 | React integration for GSAP |
| [Lenis](https://lenis.studiofreight.com/) | 1.3.4 | Smooth scrolling |

### 3D Graphics

| Technology | Version | Purpose |
|------------|---------|---------|
| [Three.js](https://threejs.org/) | 0.176.0 | WebGL 3D library |
| [@react-three/fiber](https://docs.pmnd.rs/react-three-fiber) | 9.1.2 | React renderer for Three.js |
| [@react-three/drei](https://github.com/pmndrs/drei) | 10.0.8 | Useful helpers for R3F |
| [maath](https://github.com/pmndrs/maath) | 0.10.8 | Math utilities (easing, colors) |

### Utilities

| Technology | Version | Purpose |
|------------|---------|---------|
| [react-scroll](https://github.com/fisshy/react-scroll) | 1.9.3 | Smooth scroll navigation |
| [react-responsive](https://github.com/contra/react-responsive) | 10.0.1 | Responsive breakpoint hooks |
| [@iconify/react](https://icon-sets.iconify.design/) | 6.0.0 | Icon library |

### Code Quality

| Technology | Version | Purpose |
|------------|---------|---------|
| [ESLint](https://eslint.org/) | — | Code linting |
| eslint-plugin-react-hooks | — | Enforces React Hooks rules |
| eslint-plugin-react-refresh | — | Validates hot reload compatibility |

---

## How It Works

### Loading Sequence

1. `App.jsx` renders `LoadingScreen` immediately
2. A `useEffect` hook initiates asset preloading:
   - The `Planet.glb` 3D model is loaded via Three.js `GLTFLoader`
   - Background and project images are preloaded via `new Image()`
3. A `progress` state (0–100) updates as assets load
4. Once `progress === 100`, a 300ms fade-out transition plays, then `LoadingScreen` unmounts

### 3D Planet Rendering

Inside `Hero.jsx`:

```jsx
<Canvas>
  <Suspense fallback={null}>
    <Float speed={2} rotationIntensity={0.5} floatIntensity={1}>
      <Planet />
    </Float>
    <Environment resolution={512}>
      <Lightformer intensity={2} position={[0, 5, -5]} />
    </Environment>
  </Suspense>
</Canvas>
```

- `Canvas` from `@react-three/fiber` creates a WebGL context
- `Suspense` allows async 3D model loading without blocking
- `Float` from Drei adds continuous bobbing motion
- `Environment` with `Lightformer` creates realistic reflections on the gold ring

### Scroll Animations

GSAP `ScrollTrigger` is used throughout. Example — Service Cards:

```js
gsap.to(cardRef.current, {
  scrollTrigger: {
    trigger: sectionRef.current,
    start: "top top",
    end: "bottom bottom",
    scrub: 1,
  },
  y: -50,
  opacity: 0,
})
```

The `scrub: 1` option creates a smooth 1-second lag between scroll position and animation, giving weight to the motion.

### Smooth Scroll Integration

Lenis is initialized with GSAP ticker integration:

```js
const lenis = new Lenis()
lenis.on('scroll', ScrollTrigger.update)
gsap.ticker.add((time) => lenis.raf(time * 1000))
gsap.ticker.lagSmoothing(0)
```

---

## Project Structure

```
awwwards-portfolio-main/
├── public/
│   ├── assets/
│   │   ├── backgrounds/          # Section background images
│   │   └── projects/             # Project preview thumbnails
│   ├── fonts/
│   │   ├── otf/                  # OpenType fonts (Amiamie family)
│   │   └── ttf/                  # TrueType fonts
│   ├── images/                   # Misc assets (profile photo, etc.)
│   └── models/
│       └── Planet.glb            # 3D planet model
├── src/
│   ├── components/
│   │   ├── AnimatedHeaderSection.jsx   # Reusable section header with animation
│   │   ├── AnimatedTextLines.jsx       # Staggered line reveal component
│   │   ├── Marquee.jsx                  # Horizontal scroll marquee
│   │   └── Planet.jsx                  # 3D planet mesh + ring geometry
│   ├── sections/
│   │   ├── About.jsx
│   │   ├── Contact.jsx
│   │   ├── ContactSummary.jsx
│   │   ├── Hero.jsx
│   │   ├── Navbar.jsx
│   │   ├── ServiceSummary.jsx
│   │   ├── Services.jsx
│   │   └── Works.jsx
│   ├── constants/
│   │   └── index.js             # Services data, projects list, social links
│   ├── App.jsx                   # Root component + loading state
│   ├── index.css                 # Tailwind imports, custom fonts, theme tokens
│   └── main.jsx                  # React entry point
├── index.html
├── package.json
├── vite.config.js
├── eslint.config.js
└── README.md
```

### Key Files

| File | Purpose |
|------|---------|
| `src/App.jsx` | Root component managing loading state and section composition |
| `src/sections/Hero.jsx` | Contains the R3F `Canvas` and 3D planet |
| `src/constants/index.js` | All static content — services, projects, social URLs |
| `src/index.css` | Tailwind directives, `@theme` tokens, `@font-face` declarations |
| `vite.config.js` | Vite configuration with Tailwind plugin |

---

## Getting Started

### Prerequisites

- **Node.js** ≥ 18.x
- **npm** ≥ 9.x (or yarn/pnpm equivalent)

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/NipunKushwaha-web/Awards-Portfolio.git
   cd awwwards-portfolio
   ```

   > Inspired by [NipunKushwaha-web/Awards-Portfolio](https://github.com/NipunKushwaha-web/Awards-Portfolio)

2. **Install dependencies**
   ```bash
   npm install
   ```

### Development

Start the development server with hot reload:

```bash
npm run dev
```

The app will be available at `http://localhost:5173`.

### Production Build

Build optimized assets for deployment:

```bash
npm run build
```

Preview the production build locally:

```bash
npm run preview
```

### Linting

Run ESLint to check for code quality issues:

```bash
npm run lint
```

---

## Future Scope

The following enhancements are planned or could be considered for future iterations:

- [ ] **CMS Integration** — Connect a headless CMS (Contentful, Sanity) for dynamic project and service management
- [ ] **Blog Section** — Technical blog with MDX support and code syntax highlighting
- [ ] **Dark/Light Mode** — Theme toggle with CSS custom properties
- [ ] **Contact Form** — Functional form with serverless function (Vercel/Netlify Functions) or Formspree
- [ ] **3D Scene Transitions** — Morph between section-specific 3D backgrounds
- [ ] **Performance Optimization** — Lazy-load sections below the fold, implement `IntersectionObserver` for deferred GSAP initialization
- [ ] **Accessibility Audit** — WCAG 2.1 AA compliance pass: keyboard navigation, ARIA labels, reduced motion support
- [ ] **Multi-language Support** — i18n integration for bilingual portfolios

---

## Contributing

Contributions are welcome. Please follow these guidelines:

1. **Fork** the repository and create your branch from `main`
2. **Keep changes focused** — avoid unrelated refactoring mixed with feature work
3. **Test your changes** — verify animations still trigger correctly after modifications
4. **Follow the existing code style** — ESLint rules are enforced
5. **Write meaningful commit messages** — describe *why* not *what*

---

## License

This project is licensed under the **MIT License**. You are free to use, modify, and distribute this codebase for personal or commercial projects, provided attribution is included.

---

*Built with React, Three.js, and GSAP — deployed on Vercel.*
