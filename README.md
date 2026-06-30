# Sharp Frame

A fast, content-focused marketing homepage built with [Astro](https://astro.build).

## Research

### 1. What is Astro?
A web framework for building fast, content-focused websites. It compiles your components into plain HTML at build time instead of shipping a full JS app to the browser.

### 2. Why use Astro for this type of website?
A marketing homepage like Sharp Frame is mostly static content — text, images, links. It doesn't need a heavy JS framework to manage complex client-side state, so Astro's lightweight output is a perfect fit.

### 3. What is Astro best for?
Content-heavy, mostly-static sites: marketing pages, blogs, portfolios, documentation. Anywhere the priority is fast load times and SEO over complex interactivity.

### 4. Benefits of Astro
- Fast page loads
- Great Lighthouse/SEO scores out of the box
- Component-based structure (reusable `.astro` files with scoped styles)
- Flexibility to mix in React/Vue components only where you actually need interactivity

### 5. What does "less JavaScript by default" mean?
Astro renders everything to static HTML at build time and sends zero JS to the browser unless you explicitly opt in (e.g. `client:load` on a specific component). Most sites don't need JS for every section, so this keeps the page lightweight by default.

### 6. How does JSON-driven content help future updates?
Content lives separately from layout code. Want to change a headline, add an FAQ, or update a phone number? Edit the JSON file — no touching component markup or styling. This makes the site easier to maintain, especially for non-developers or future you.

## Development Reflection

### 1. How did you understand the reference homepage?
I studied the reference site section by section before writing any code, with the goal of understanding content structure rather than copying the visual design. I identified 9 distinct sections:

- Navbar — logo, 4 nav links, Contact Us CTA
- Hero — full-width image with overlaid card, headline, subheadline, CTA
- Services — 4 service cards (Indoor Billboard, Social Media, Website Design, Lead Generation)
- Venue Partner Network — image left, text + bullet grid right
- Full-Service Digital Marketing — 3-column layout: text, image, feature list
- Why Choose Us — full-bleed photo with overlaid text card
- FAQ — 3 accordion items
- CTA Banner — centered heading, body, and button
- Footer — logo, tagline, 5-column link grid, contact info, legal

I noted what each section was trying to communicate (the hero establishes credibility, services shows breadth, the venue section demonstrates the network, and "why us" builds trust), which helped preserve the content intent while changing the visual execution entirely.

### 2. What design direction did you choose and why?
I moved completely away from the reference layout to establish a premium, high-contrast, yet welcoming brand experience: an organic, warm editorial palette pairing a rich Forest Green (`#2D5A3D`) base with soft Sage Cream surfaces (`#F4F7F2`) and deep charcoal text accents (`#1A3325`). Primary calls-to-action use a vibrant Terracotta Orange (`#C4622A`).

These earth-toned, warm choices evoke an approachable, neighborhood feel fitting for a business marketing to local, community-driven brands. Typography reinforces the balance: Plus Jakarta Sans for clean, modern heading hierarchy, and IBM Plex Mono for subtle, internal file-style labels that give the agency's identity a structured, precise edge.

To elevate this aesthetic from a static design into an engaging digital product, I layered in smooth, fluid hover animations across all interactive components and card grids. By utilizing crisp CSS transitions on scale, background filters, and shifts in depth, the interface fields highly responsive, tactile, and interactive.

### 3. How did you split the content into JSON?
I separated layout logic from copywriting by abstracting all text strings, array structures, list metrics, and link pathways out of the Astro layout pages into isolated JSON models inside a centralized `src/data/` directory (e.g. `services.json`, `faq.json`, `footer.json`, `scanmodal.json`). Iterative structures like the consultation modal's feature checklist or the services breakdown are modeled as structured JSON object arrays, so layout copy can be changed without touching application code.

### 4. What reusable components did you create?
The interface is modularized into self-contained, atomic building blocks within `src/components/`, split into Page Sections, Interactive UI Elements, and a Data Layer — components simply loop through structured JSON.

**Global & Interactive UI Components**
- `NavBar.astro` — sticky, accessible navigation with desktop dropdown layers and a mobile hamburger menu drawer
- `FloatingCTA.astro` — persistent, fixed trigger button driving conversion visibility across long scrolling sessions
- `ScanModal.astro` — dual-panel dialog built on the native HTML `<dialog>` element with automated modal state behavior

**Data-Driven Page Sections**
- `HeroSection.astro` — full-bleed visual showcase with an offset card component
- `ServicesSection.astro` & `FullServiceSection.astro` — multi-column layouts rendering icon arrays and feature cards over themed panels
- `VenueSection.astro` & `MapSection.astro` — side-by-side informational splits with location breakdown sidecars
- `WhyUsSection.astro` & `PartnerSection.astro` — background graphical sections with glassmorphism cards and cream surface overlays
- `FaqSection.astro` — interactive accordion card matrix
- `CtaBanner.astro` & `Footer.astro` — full-bleed transition banners and multi-tier footer navigation

**Decoupled JSON Data Layer**
Every component above is entirely decoupled from hardcoded text, consuming isolated data models from `src/data/` (`hero.json`, `services.json`, `venue.json`, `fullservice.json`, `why-us.json`, `faq.json`, `mapsection.json`, `partner.json`, `cta.json`, `footer.json`, `nav.json`, `scanmodal.json`). This headless architecture guarantees content updates never require touching template syntax.

### 5. What steps did you follow while developing?
A structured pipeline prioritizing layout planning, separation of content from view logic, and component-driven assembly:

1. **Discovery & Blueprinting** — Audited the reference site, cataloging every structural section, content block, and copywriting asset into a data requirement map.
2. **Headless Data Modeling** — Translated scraped content into 12 isolated JSON models in `src/data/`, decoupling copy from presentation before touching layout files.
3. **Visual Framing & Prototyping** — Designed a high-fidelity layout in Figma with a warm editorial aesthetic, then drafted foundational HTML structures.
4. **Environment Initialization** — Initialized a clean Astro workspace (`npm create astro@latest`) with a bare template for an organized, modular project structure.
5. **Component Architecture Porting** — Broke the master HTML template down into individual `.astro` components in `src/components/`.
6. **Dynamic Data Wiring** — Injected JSON files into component frontmatter, replacing static text with `.map()`-driven, programmatic arrays.
7. **Asset Integration & Optimization** — Replaced placeholder gradients with production-ready `.avif` media assets in `public/images/`.
8. **Responsive Layering & Scoped Styling** — Added mobile and desktop `@media` breakpoints inside each component's scoped `<style>` block for mobile-first fluidity.
9. **Verification & Browser Audit** — Ran `npm run dev` to smoke-test data rendering, breakpoint scaling, asset load speed, and cross-browser consistency.

### 6. What problems did you encounter and how did you solve them?

**WhyUsSection glass card disappearance (Astro scoping & stacking context)**
Moving absolute positioning rules from inline styles into a scoped CSS class caused the card to disappear, due to Astro's component isolation interfering with the parent grid's layout engine. *Fix:* reverted critical layout properties (`position`, `background`, `backdrop-filter`, `display`, `box-shadow`) back to inline styles, keeping the scoped stylesheet for motion behaviors only.

**FullServiceSection animation defect (conflicting declarations)**
The scroll-triggered entrance animation failed because duplicate transition timings on `.fs-card` and `.fs-card-animate` cancelled each other out. *Fix:* flattened the architecture, folding baseline transition states into `.fs-card` and using a single `.in-view` modifier as the trigger.

**ServicesSection hover lag (transition delay leakage)**
A staggered slide-in effect used `transition-delay`, which then leaked into hover interactions, making cards feel sluggish. *Fix:* stripped the CSS delay and offloaded the initial stagger sequence to JavaScript `setTimeout` calls, restoring instant hover response.

**Footer grid misalignment (wrapper div pollution)**
Changing the footer's top band background color broke the multi-column grid alignment due to unnecessary wrapper elements. *Fix:* used a container shell pattern — a full-bleed wrapper for the background color, with a nested inner block at `max-width: 1200px; margin: 0 auto` to mirror the section above it.

**ScanModal viewport scrollbar flash (layout spill)**
The modal's entrance animation used a heavy `translateY(48px)` offset that briefly pushed content past the screen boundary, flashing the scrollbar. *Fix:* applied `overflow: hidden` directly on the parent `<dialog>` wrapper to stop the layout spill before it could trigger a shift.

**MapSection structural collapse (inheritance failure)**
Switching to a side-by-side layout collapsed the embedded map iframe to `0px` height, since `height: 100%` had no valid parent height to reference. *Fix:* hard-coded an explicit `height: 520px` on the iframe.

### 7. What would you improve if you had more time?
Moving from a high-fidelity prototype to an enterprise-ready, production-grade deployment, targeting four areas:

**Performance & Asset Delivery**
- Convert remaining `.png` overlay graphics to `.avif` and apply `loading="lazy"` to all non-hero images.
- Subset the Plus Jakarta Sans font package to only the weights actually used in CSS.

**Architectural Refactoring & Code Quality**
- Extract inline JavaScript in `NavBar.astro` (e.g. `closeAll` routines, event listeners) into an external TypeScript module.
- Revisit `WhyUsSection` to remove the inline-style workaround, and replace broad `<style is:global>` blocks with strictly scoped styles.
- Move hardcoded metadata (`<title>`, `<meta description>`) out of `index.astro` into a centralized configuration or JSON contract.

**Advanced UX, Motion & Accessibility**
- Add missing semantic attributes (`aria-controls`, `aria-labelledby`) to `FaqSection` accordions, implement keyboard tab-trapping and arrow navigation in dropdowns, and adjust cream-on-green contrast to meet WCAG guidelines.
- Add `prefers-reduced-motion` fallbacks and configure scroll-driven animations to fire once rather than re-triggering on scroll-back.
- Refactor `ScanModal` from a fixed 3-second delay to an intent- or scroll-depth-based trigger (e.g. firing after 50% viewport scroll).

**Production Readiness**
- Replace all placeholder `href="#"` links with live routing, and add a custom `404.astro` page, an automated `sitemap.xml` build step, and a `robots.txt`.
