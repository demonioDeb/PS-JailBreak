# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository overview

This repo is a single static landing page for **ModLab Mérida**, a local PS4 software-modding/repair business in Mérida, Yucatán, Mexico. It is deployed via GitHub Pages (canonical URL `https://demoniodeb.github.io/PS-JailBreak/`). There is no backend, no build system, and no package manager — the entire site is one self-contained HTML file plus a handful of static images.

## Repository structure

- `index.html` — the entire site: markup, `<style>` CSS, structured data (JSON-LD), and vanilla JS, all inline in one file.
- `assets/` — favicons and touch icons (`favicon-16.png`, `favicon-32.png`, `favicon-48.png`, `apple-touch-icon.png`). Note: `index.html` references `assets/og-image.jpg` for Open Graph/Twitter card previews, but that file does not currently exist in `assets/`.

## Development workflow

There is no build, lint, test, or package-install step — this is plain HTML/CSS/JS with no dependencies.

- **Preview locally**: open `index.html` directly in a browser, or serve the directory (e.g. `python3 -m http.server`) from the repo root so relative asset paths resolve correctly.
- **Deploy**: pushing to the branch that GitHub Pages is configured to serve (check repo Settings → Pages) publishes the site automatically; there is no separate build/deploy step to run.
- When editing, verify changes by opening the file in a browser rather than relying on any test suite — none exists.

## Content & structure conventions

- **Single-file architecture**: All CSS lives in the `<style>` block in `<head>`; all JS lives in a `<script>` block at the end of `<body>`. Keep new styles/scripts inline in `index.html` rather than splitting into separate files, consistent with the current structure.
- **Site language is Spanish** (`<html lang="es">`) and targets a Mexican audience (`es_MX` locale, MXN currency). Keep all user-facing copy in Spanish unless told otherwise.
- **Section-based layout**: the page is a sequence of `<section>` blocks with descriptive `id`s (`beneficios`, `paquetes`, `ubicacion`, `garantia`, `faq`, `contacto`) used both for the nav/anchor links and as natural editing boundaries — most content edits target one section.
- **CSS variables**: colors, spacing, and radii are centralized in `:root` custom properties (`--bg`, `--cyan`, `--green-wa`, `--text`, etc. at the top of the `<style>` block). Reuse these variables instead of hardcoding new colors.
- **Pricing/package selectors**: the "Con Disco Interno" and "Interno + Externo" package cards have interactive capacity pills (`.pill` buttons with `data-price`/`data-label` attributes) wired up by two near-identical IIFEs at the bottom of the script. Each IIFE updates the displayed price (via `fmt()`), the descriptive note, and the WhatsApp deep-link text/URL for that package. When adding a new capacity option, add a `.pill` button with the right `data-price`/`data-label` in the matching `.pill-row`; when adding a new package tier, mirror one of these existing IIFEs rather than inventing a new pattern.
- **WhatsApp is the sole contact/CTA channel**: every call-to-action link points to `https://wa.me/529991558206` with a pre-filled, URL-encoded message (`?text=...`). New CTAs should follow this same `wa.me` deep-link pattern with an appropriately encoded message.
- **SEO/social metadata**: `<head>` contains Open Graph, Twitter Card, and JSON-LD (`LocalBusiness`) structured data. If prices, phone number, address, or service description change in the visible content, update the corresponding values in the JSON-LD block and meta tags too, so they stay in sync.
