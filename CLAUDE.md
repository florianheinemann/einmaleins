# Einmaleins-Dojo – notes for Claude

A times-table trainer for one child (the owner's son), installed as a PWA on an iPhone home screen. UI text is German; code and comments are English.

## Layout
- `index.html` – the whole app: CSS, markup for all screens, and one inline script. No build step, no dependencies, no tests.
- `sw.js` – service worker: app files network-first, Google Fonts cache-first; only deletes caches with its own `dojo-` prefix.
- `manifest.webmanifest`, `*.png` – install metadata and icons.

## How the app is organised
- Screens are `<section class="screen">` elements swapped by `show(id)`: `dojo`, `game`, `result`, `master`, `sensei`, `teach`.
- Progress lives in `localStorage` (`dojo.*` keys). Facts are keyed by `fkey(a,b)`, so 3·7 and 7·3 share one level (0–3).
- `STAGES` is the progression: each stage has its rows (each × 1 to 10), ranks by shuriken (`BELTS`, `DANS`), and a finale stored for good under its `key`. Stage 1: rows 1–10, belts, Großmeister. Stage 2: rows 11–15, Dan stripes, dragon, Sensei.
- To add a stage: append to `STAGES` and add its finale screen to `showFinale()`. Never change existing storage keys or the `fkey` format, or the child loses progress.

## Releasing (always do all of these)
1. Bump the version in **both** `index.html` (`const VERSION`) and `sw.js` (`const CACHE`). Patch for fixes, minor for features. Put the version in the commit title, e.g. `Fix … (1.3.1)`.
2. Test in headless Chromium at phone size (390×844). Playwright is preinstalled; serve the folder with `python3 -m http.server` and seed `localStorage` to reach later stages.
3. Merge to `main` via a PR.
4. **Deploy:** fast-forward `gh-pages` to `main` (`git push origin origin/main:refs/heads/gh-pages`). GitHub Pages serves from `gh-pages`; `main` alone does not go live. `gh-pages` holds no commits of its own, so this is always a fast-forward; never force-push it.

## Conventions
- The parents' text refers to the child as "er". That is intentional (it's the owner's son); don't neutralise it.
- Mistakes never punish: no lives, no red crosses. Keep new features in that spirit.
- Respect `prefers-reduced-motion` (the `reduce` flag) for any new animation.
