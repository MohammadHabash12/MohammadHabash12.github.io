# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Mohammad Alhabash's personal portfolio: a static, single-page site with no build step, dependencies, linter or tests. The whole site is `index.html` (inline `<style>` and `<script>`). Fonts come from Google Fonts; nothing else is loaded externally. `_config.yml` keeps repo-only files (`CLAUDE.md`, and `image.jpg`, an old portrait the site no longer shows) out of the GitHub Pages build; add any new repo-only file there.

## Run and deploy

- Local preview: open `index.html` directly in a browser; no server or build is needed. (`.claude/launch.json` holds a small static-server config only so the Claude app can preview the page in its browser pane; the site itself doesn't use it.)
- Deploy: this repo is `MohammadHabash12.github.io`, served by GitHub Pages from `main`. **Pushing to `main` publishes the live site** (it updates in ~30–40s). Confirm with the user before pushing.
- Git tags `violet-version` and `amber-version` mark earlier colour schemes of the redesign, to restore with `git checkout <tag> -- index.html`.

## How `index.html` fits together

**Theming.** All colours are CSS custom properties: dark theme on `:root`, light theme on `[data-theme="light"]`. An inline script in `<head>` applies the saved theme (`localStorage` key `theme`) before first paint and adds a `js` class to `<html>`; scroll-reveal styles only hide `.reveal` elements under `.js`, so the page still renders without JS. Change colours via the tokens, not hard-coded values. Exceptions:
- The hero terminal always stays dark and has its own `--t-*` tokens.
- The hero dot-grid canvas reads its colour from the `--dot` token (an `r, g, b` triplet) in JS whenever the theme changes.
- `toggleTheme()` also updates `<meta name="theme-color">`, so keep those hex values in sync with `--bg`.

**Content lives in more than one place.** When editing site content, update every copy:
- Projects, stack, experience and contact details also appear as text in the terminal's `commands` object in the main script (`projects`, `stack`, `experience`, `contact`, `whoami`).
- The contact email is in the `EMAIL` constant, the `mailto:` links, and the copy button's `data-copy`.
- Section ids must match the `SECTIONS` array (used by the nav scroll-spy and the terminal's `cd`), the desktop nav links, and the mobile menu links.
- The marquee has two identical `.marquee-group` blocks; the infinite scroll relies on them matching.
- The name ("Mohammad Alhabash") also appears in `<title>`, meta/OG tags, the About sidebar, and the footer.

**Behaviour** is a single IIFE at the end of `<body>`: nav scroll state and progress bar, an IntersectionObserver scroll-spy, the mobile menu (uses `inert`), reveal-on-scroll, stat count-up, pointer spotlight/tilt on `.spot`/`.tilt` (fine pointers only), the dot-grid canvas (paused when the hero is off-screen), the terminal, copy-email, and the contact form. There is no form backend: the form builds a `mailto:` draft.

**Motion.** Everything respects `prefers-reduced-motion` (a global CSS override plus `reduceMotion` checks in JS); keep new animations compatible with both.

**Project cards** each have an inline SVG illustration (`.project-viz`) styled by `.viz-*` classes. Neither card has a "View project" link yet; a commented placeholder marks where one goes.
