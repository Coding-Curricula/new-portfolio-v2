# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project shape

Static, hand-authored HTML + CSS portfolio. **No build step, no package manager, no test runner, no JavaScript** — the "zero JavaScript" stance is an explicit design constraint (see `index.html:6`, `index.html:64`, `index.html:283`, `index.html:311`), not an accident. Do not introduce JS, bundlers, or framework dependencies; if a feature seems to require JS, first attempt a CSS-only solution using the pattern below.

## Running / previewing locally

Open `index.html` directly in a browser, or serve the directory with any static server, e.g.:

```sh
python3 -m http.server 8000   # then visit http://localhost:8000
```

There are no `npm`/lint/test commands because there's no toolchain.

## CSS architecture

`index.html` loads stylesheets in a deliberate cascade order — preserve it when adding new ones:

1. `css/reset.css` — minimal modern reset.
2. `css/tokens.css` — CSS custom properties (colors, type scale, spacing, motion). **All theming and design values live here.**
3. `css/base.css` — element defaults + reusable primitives (`.btn`, `.section-label`, `.dot`, `.skip-link`, `.state-input`).
4. `css/layout.css` — page chrome (header/footer), `.container`, section rhythm, breakpoints.
5. `css/components.css` — **referenced by `index.html:14` but not yet present.** Section/component styles (hero, projects, skills, contact, cards) belong here.
6. `css/interactive.css` — **referenced by `index.html:15` but not yet present.** Owns the `:checked`-driven behavior described below, including the `.theme-dark` override rule that pairs with `tokens.css`.

When creating `components.css` or `interactive.css`, match the existing files' style: file-header banner comment, lowercase-hyphen class names, BEM-ish (`block__element--modifier`), and tokens via `var(--…)` rather than literal values.

## The "no-JS interactivity" pattern

State toggles use hidden `<input>` elements placed near the top of `<body>` so their `:checked` state can drive any later element via the general sibling (`~`) combinator. The hidden inputs use `.state-input` (visually clipped but focusable):

- `#theme-toggle` (checkbox) — drives light/dark theme. `interactive.css` is expected to contain a rule like `#theme-toggle:checked ~ * .theme-dark, …` that applies the dark token block defined in `tokens.css:70`. OS preference is honored via `@media (prefers-color-scheme: dark)` in `tokens.css:84`.
- `#nav-toggle` (checkbox) — drives the mobile nav drawer (`.primary-nav` is `transform: translateY(-100%)` by default, see `layout.css:83`).
- `#f-all` / `#f-web` / `#f-cli` / `#f-other` (radios) — drive project-grid filtering by toggling visibility on `.project-card[data-cat="…"]` siblings.

The visible "buttons" are `<label for="…">` elements styled to look interactive (`.hamburger`, `.theme-switch`, `.filter-tab`). When adding a new toggle, follow the same recipe: hidden `.state-input` near the top of `<body>` → visible `<label>` somewhere in the page → sibling-combinator rules in `interactive.css`.

## Theming

Light is the default; dark applies in two ways and `tokens.css` already handles both:

- Manual user toggle: `.theme-dark` class on `<html>` (the `#theme-toggle:checked` rule in `interactive.css` is responsible for adding the equivalent override — currently this rule is missing along with the file).
- OS preference: `@media (prefers-color-scheme: dark)` block, scoped to `:root:not(.theme-light)` so a future manual "force light" class can win.

When adding colors, add them as tokens in **both** the `:root` block (`tokens.css:7`) and the `.theme-dark` block (`tokens.css:70`) plus the `prefers-color-scheme` block (`tokens.css:84`).

## Visual identity (don't drift)

Brutalist/monospace: 2px ink borders (`--border`), hard offset shadows (`--shadow-hard` = `6px 6px 0 var(--ink)`), hot orange accent (`--accent: #ff3a00`), `ui-monospace` font stack for everything (body and headings both use `--font-mono`). Background has a faint 32px grid via two `linear-gradient`s in `base.css:18`. Reuse tokens; avoid literal colors, pixel values, or transition timings.

## Accessibility expectations

The page already wires: skip link (`index.html:19`), visible `:focus-visible` outlines (`base.css:73`), `prefers-reduced-motion` opt-out (`base.css:34`), `aria-label`/`role` on the label-as-button controls, and `color-scheme: light dark`. Preserve these when editing — in particular, keep `.state-input` focusable (don't `display:none` it) and keep `<label>`-as-button elements keyboard-reachable.

## Known gaps in the current checkout

- `css/components.css` and `css/interactive.css` are linked from `index.html` but absent — browsers will 404 them. They are expected to be authored.
- `assets/favicon.svg` and `assets/img/project-01.svg` … `project-06.svg` are referenced but the `assets/` directory does not exist.
- Several content strings are placeholders (`[Your Name]`, `YOUR NAME`, `hello@example.com`, `#` project links).
