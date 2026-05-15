# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Marketing site for **Pure Exterior Home Solutions**, a residential exterior-cleaning business (window washing, pressure washing, solar panel cleaning, surface cleaning) serving Davis and Salt Lake Counties, Utah. Owner: Trevor Jenson. Phone displayed on site: 801-500-4545.

## Tech & Workflow

- **Pure static HTML.** No build step, no package manager, no test suite, no framework. Each page is one self-contained `.html` file with inline `<style>` and inline `<script>`.
- **Deploy:** Vercel auto-deploys on push to `main` — there is no separate build/deploy command to run. Production hostname: `pure-exteriors.vercel.app`; canonical domain: `www.pureexteriorhomesolutions.com`.
- **Local preview:** open the `.html` file directly in a browser, or `python3 -m http.server` from the repo root if you need real paths (favicons/OG images use absolute `/...` URLs).
- **After significant changes:** update the project's Notion page with a summary of what changed. Ask the user for the page link if you don't have it in context.

## Repo layout

- `index.html` — homepage (longest file, contains the canonical nav/footer/quote-form patterns)
- `pressure-washing.html`, `window-washing.html`, `solar-panel-cleaning.html`, `surface-cleaning.html` — one page per service
- `our-work.html` — gallery / case studies
- `images/` — all photos and icons referenced by the pages
- Favicons + `purehero.png` live at the repo root (referenced as `/favicon-32.png`, `/images/purehero.png`, etc.)

## Architecture notes (the non-obvious parts)

**There is no shared CSS or JS file.** Every page duplicates the nav, footer, quote form, CSS variables, and the JS for mobile menu / accordion / scroll-fade / form submit. When you change a shared component (nav, footer, brand color, button styles), you must update **every** HTML file or pages will drift. Grep across `*.html` before considering a change "done."

**Design tokens** are defined as CSS variables at the top of each page's `<style>` block (`:root { --blue: #29ABE2; --blue-dark: #1485B8; ... }`). Fonts: `Barlow Condensed` (display) + `Inter` (body), loaded from Google Fonts.

**Quote form** posts to [Web3Forms](https://web3forms.com) — the `access_key` is embedded client-side in the inline `<script>` of each page (this is how Web3Forms is designed to work; the key is public-by-design, not a secret). All pages share the same access key. Submissions are handled with a `fetch` to `api.web3forms.com/submit` and an inline success/error state on the button.

**SEO:** each page has its own canonical URL, Open Graph tags, Twitter card, and Schema.org `HomeAndConstructionBusiness` JSON-LD. Keep these in sync with on-page copy when content changes.

## Reference docs (outside the repo)

Sales script, pricing sheet, and other source material live at `~/Documents/Claude/Projects/Pure Exterior - Reference Docs/`. This folder is **not** part of the repo and is not committed. Consult it when you need authoritative copy, prices, or service definitions — don't invent these from what's already on the site.
