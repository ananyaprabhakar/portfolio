# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A static, build-free personal UX/design portfolio for Ananya Prabhakar. It is plain HTML deployed as-is — there is **no package manager, bundler, framework, transpiler, or test suite**. Every page is a single self-contained `.html` file with all CSS (`<style>`) and JavaScript (`<script>`) inlined; the only runtime dependencies are Google Fonts and Fontshare loaded from CDN. Assets live in `uploads/`.

## Running / previewing

No build step. Open a file directly, or serve the folder for correct relative-path resolution:

```bash
python -m http.server 8000   # then visit http://localhost:8000
```

Deployment is "copy the files to a static host" — there is no CI, no config file (no `package.json`, `netlify.toml`, `vercel.json`, or `CNAME`).

## Page structure

- `index.html` — landing page; links out to every case study and to `about.html`.
- `about.html` — about page.
- Case studies (one self-contained file each): `acme.html`, `freshbite.html`, `matchdog.html`, `rockx.html`, `southwest.html`, `ticketsque.html`. Most case studies include a per-page image **lightbox** (e.g. `.sw-lightbox` in `southwest.html`) that opens screenshots full-screen — also prefixed per project.
- `favicon.svg` — site icon.
- `uploads/` — all images, organized into per-project subfolders (`acme/`, `freshbite/`, `matchdog/`, `RockX/`, `Southwest/`, `ticketsque/`), each often with `artefacts/`, `mockups/`, `frames/` etc. Note: some folder/file names contain **spaces and mixed case** (e.g. `uploads/images for about page/`, `RockX/axure ss/`) — quote paths and preserve casing.

## Conventions shared across pages

These patterns are duplicated (copy-paste) into every page rather than shared from a common file. When changing one, check whether the others need the same change.

- **Theme tokens**: a `:root` block defines CSS custom properties (`--bg`, `--accent`, `--heading`, shadows, etc.); an `html.dark { … }` block overrides them for dark mode. Style everything via these variables, not hardcoded colors.
- **Dark mode toggle**: a `.theme-toggle` button calls `document.documentElement.classList.toggle('dark')`. The choice is **not persisted** (no `localStorage`) and resets on reload / does not carry across pages — keep this in mind before assuming a persistence bug.
- **Custom cursor**: every page sets `cursor: none` on `html, body` and renders a custom `#cursor` element that follows the mouse and scales on `.hovering`. Interactive elements must account for this (the native cursor is hidden site-wide).
- **Fonts**: headings use `Fraunces` (serif), body uses `Chillax` (sans), both from CDN.
- **Animations/interactions**: scroll-reveal via `IntersectionObserver`, fade-up keyframes, and per-page fit/scroll helpers (e.g. `fitCard`, `setupCardScroll`, and project-specific variants like `fitRXC`/`fitMDC`/`fitTQC`). These are vanilla JS, no libraries. Most pages also gate motion behind `@media (prefers-reduced-motion: reduce)`.
- **Per-project class prefixes**: the elaborate "device mockup" scenes (phones/laptops/browser chrome that hold project screenshots) are namespaced per project — `fbm-` (FreshBite), `tqc-` (TicketsQue), `mdc-` (MatchDog), `rxc-`/RockX, `swc-`/`sw-` (Southwest). On `index.html` these render inside each `.card-sleeve`; the same prefix scheme recurs inside the matching case-study page. When restyling a project's mockup, search its prefix.
- **Navigation**: a fixed `nav` with a `.nav-hamburger` for mobile; links are plain relative `href="*.html"`.

## Editing guidance

- Edits are direct HTML/CSS/JS surgery inside the relevant file; there is nothing to compile or lint.
- Because pages are large single files (~80–130 KB), use targeted searches (Grep) to locate a section rather than reading whole files.
- External links (Figma prototypes, Axure, Google Docs, IEEE/Springer publications, social profiles) are hardcoded `<a href>`s — update them in place.
