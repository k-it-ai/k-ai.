# KOSSOV.IT — Sovereign AI website

Static marketing site for KOSSOV.IT. No build step, no dependencies, no
server-side code — plain HTML/CSS/JS that can be dropped onto any static host.

## Structure

```
index.html          Homepage (Hero · Why · Services · Data flow · Deployment ·
                    Compliance · SLA · FAQ · Contact)
legal-notice.html   Impressum + Datenschutzerklärung (Legal Notice + Privacy)
assets/
  k-logo.svg        Brand logo (the "K" mark)
  favicon.svg       Favicon / apple-touch-icon
  og-image.png/.svg Social share image (1200×630)
  fonts.css         @font-face for the self-hosted fonts
  fonts/            Inter + JetBrains Mono (woff2, self-hosted, OFL)
  flags/de.svg      Language switch — German
  flags/en.svg      Language switch — English
_headers            Security + cache headers (Cloudflare Pages)
_redirects          /impressum, /datenschutz, /legal → legal-notice.html
robots.txt          Crawl rules + sitemap reference
sitemap.xml         Sitemap (homepage)
```

## Languages (DE / EN)

The site is bilingual. **German is the default** and lives directly in the HTML,
so visitors with JavaScript disabled and search-engine crawlers always get
German. English strings are stored in a small dictionary (`I18N_EN`) inside each
page and applied on top when needed.

Selection logic, in order:

1. A previously chosen language saved in `localStorage` (`lang` = `de` | `en`).
2. Otherwise the browser language — only switches to **English** if the browser
   explicitly prefers English (`navigator.language` starts with `en`).
3. Otherwise **German** (fallback).

Clicking a flag switches immediately and remembers the choice. `<html lang>`,
`<title>` and the meta description update with the language.

To edit copy: change the German text in the HTML, and the matching key in the
`I18N_EN` object for the English version.

## Local preview

It is just static files, so any static server works:

```bash
python3 -m http.server 8080
# then open http://localhost:8080
```

## Deploy — Cloudflare Pages

This repo is ready for Cloudflare Pages with **no build step**:

1. Cloudflare dashboard → **Workers & Pages** → **Create** → **Pages** →
   **Connect to Git**, and pick `kossov-it/kossov.it-ai`.
2. Build settings:
   - **Framework preset:** None
   - **Build command:** *(leave empty)*
   - **Build output directory:** `/`
3. Deploy. Then add the custom domain **kossov.it** under the project's
   **Custom domains** tab and follow the DNS instructions.

`_headers` and `_redirects` are picked up automatically by Pages.

## Notes / TODO before going fully live

- **Contact form** is ready for [Web3Forms](https://web3forms.com): paste your free
  access key into `WEB3FORMS_KEY` in `index.html` to receive submissions by email.
  Until then — and as a permanent fallback — the form opens a pre-filled email to
  the contact address. Other options: a Cloudflare Pages Function, Formspree, etc.
- **Email obfuscation:** the address is never written in the HTML/JS source. It is
  assembled at runtime from `data-` attributes (no `mailto:` or `x@y` for
  harvesters), with a `hi(at)kossov.it` fallback if JS is off. Contact: `hi [at]
  kossov.it`.
- **Fonts** (Inter, JetBrains Mono) are self-hosted under `assets/fonts/` (SIL OFL).
  No request ever goes to Google; nothing font-related to disclose to third parties.
- **Privacy:** `legal-notice.html` contains both the Impressum and a
  Datenschutzerklärung (anchor `#datenschutz`). Have both reviewed by a lawyer and
  conclude a data-processing agreement (AVV) with Cloudflare before going live.
- **Social preview:** `og:image` points at `assets/og-image.png` (1200×630). Edit
  `assets/og-image.svg` and re-export the PNG if you want to change it.
- **Model names / claims** (e.g. "GPT-5.5", "99.95% uptime", certification
  statuses) are marketing copy — keep them accurate and verifiable.
