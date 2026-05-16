# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository overview

A single-page marketing site for Vigil LLC, an investment fund. The entire site lives in `index.html` — there is no build system, no package manager, no test suite, and no CI. The only runtime dependency is Chart.js, loaded from `cdn.jsdelivr.net` via a `<script>` tag.

## Development

- **Preview locally**: open `index.html` directly in a browser, or serve the directory with `python3 -m http.server` and visit `http://localhost:8000`. A static server is only needed if you want to test relative-path behavior; `file://` works for everything currently in the page.
- **Deploy**: the repo is hosted as-is (GitHub Pages-style). Pushing to `main` is the deploy.
- **No build, lint, or test commands exist.** Do not add a `package.json`, bundler, or tooling unless the user explicitly asks.

## Architecture

`index.html` is structured as one document containing:

1. Inline `<style>` block (lines ~8–502) — all CSS, including the responsive breakpoints at 1024px and 768px.
2. Page sections in order: `header` → `.hero` → `.comparison-section` → `.thesis-section` → `.strategy-section` → `.cta-section` → `footer`.
3. Inline `<script>` (lines ~637–700) that instantiates a single Chart.js bar chart on `#cagrChart`.

The site has no routing, no state, no forms — CTAs are `mailto:` links to the two founders.

## Conventions

### Numeric data must stay consistent across sections

Performance numbers appear in **three** places and have to agree, or the page contradicts itself:

- `.performance-card .big-number` values (the two large stat cards).
- `.outperformance-banner h3` (the delta between Vigil and S&P 500).
- The Chart.js `data.datasets[0].data` array in the inline script.

When updating returns/CAGR figures, update all three together. The bar chart's `scales.y.max` (currently `50`) should also be revisited if any CAGR exceeds it.

### Brand styling

- Color palette is gold (`#FFD700`, `#FFA500`, `#B8860B`) on near-black (`#000`, `#0a0a0a`) with neutral grays for body copy. New elements should pull from these existing values rather than introducing new colors.
- Section structure pattern: `<section class="…-section">` wrapping a `.container` (max-width 1400px, 40px padding). Reuse this when adding sections.
- Hover effects on cards consistently use `translateY(-5px)` or `translateY(-10px)` plus a gold-tinted box-shadow.

### Content tone

Copy throughout uses confident, exclusivity-focused language ("elite", "discerning", "accredited investors only", $250K minimum). Match this register when editing. The footer disclaimer ("Past performance does not guarantee future results") must remain on any page that cites returns.

### External dependency

Chart.js is pinned to `4.4.0` via the jsDelivr CDN. If you change the version, verify the chart still renders — the config uses Chart.js v4 API shape (e.g., `scales.y` not `scales.yAxes`).
