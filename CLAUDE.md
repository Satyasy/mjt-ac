# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm run dev      # start Vite dev server with HMR
npm run build    # production build to dist/
npm run preview  # serve the built dist/ locally
```

There is no test runner, linter, or formatter configured. `dist/` is committed to the repo but git-ignored for changes; it is the deployed build output.

## What this is

A single-page marketing/landing site for **Berkah Jaya Teknik**, a 24-hour call-out AC (air conditioning) repair service in Bali, Indonesia (Denpasar, Kuta, Canggu, Jimbaran). All user-facing copy is in **Indonesian** — match the existing language and tone when editing content. The business was recently rebranded from "Multi Jaya Teknik"; watch for stale references to the old name.

## Architecture

Deliberately minimal — Vue 3 (Composition API, `<script setup>`) on Vite, no router, no state library, no backend.

- **`src/App.vue`** — the entire page lives here as one component: preloader, navbar, hero, services, testimonials, CTA, footer. All content is hardcoded inline (services, testimonials, phone numbers) rather than data-driven. Reactive logic: scroll-based navbar styling, mobile menu toggle, preloader fade-in, and an `IntersectionObserver` that adds `reveal-in` to `.reveal` elements for scroll-reveal animation.
- **`src/main.js`** — mounts `App.vue` to `#app` and imports the global stylesheet.
- **`src/assets/style.css`** — all styling (plain CSS, no preprocessor). **Flat, friendly, e-commerce-style** design (WCAG-AA colors, no gradients). Design tokens are CSS custom properties in `:root` at the top: `--primary` (blue `#1E40AF`), `--accent` (teal CTA `#0F766E`), `--whatsapp` (`#15803D`), neutrals, radii, shadows; reuse them instead of hardcoding. Fonts (loaded in `index.html`): **Rubik** (headings) + **Nunito Sans** (body). Navbar is a floating white oval pill (`.nav-container`) with a text brand-name; footer logo is rendered as a white silhouette via `filter: brightness(0) invert(1)`.
- **`public/`** — images served at absolute root paths. In markup reference them as `/logo2.png`, `/ac.jpeg`, etc. (not relative or imported).
- **`index.html`** — loads external CDN assets (Google Fonts: Outfit + Inter; Font Awesome 6.4.0) and contains all SEO metadata: `<meta>` tags, Open Graph, and a `LocalBusiness` JSON-LD block. Update these when business details (phone, service area, name) change.

## Editing notes

- **Nav anchors must match section IDs.** The navbar links (`#beranda`, `#layanan`, `#testimoni`, `#kontak`) scroll to `<section>` elements with those exact IDs in `App.vue`. Renaming one requires updating both sides.
- **Icons** are Font Awesome classes (`fa-solid`, `fa-brands`, etc.) loaded via CDN — no icon package is installed.
- The contact phone / WhatsApp number (`6282143196216`) appears in multiple places: `App.vue` (WhatsApp link + display) and `index.html` (JSON-LD `telephone`). Change all occurrences together.
