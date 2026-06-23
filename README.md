# KOSSOV.IT — Sovereign AI website

Static marketing site for KOSSOV.IT. No build step, no dependencies, no
server-side code — plain HTML/CSS/JS that can be dropped onto any static host.

## Structure

```
index.html          Homepage (Hero · Why · Services · Data flow · Deployment ·
                    Compliance · SLA · FAQ · Contact)
legal-notice.html   Impressum / Legal Notice (§ 5 DDG)
assets/
  k-logo.svg        Brand logo (the "K" mark)
  favicon.svg       Favicon / apple-touch-icon
  flags/de.svg      Language switch — German
  flags/en.svg      Language switch — English
_headers            Security + cache headers (Cloudflare Pages)
_redirects          /impressum and /legal → /legal-notice.html
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

- **Contact form** is currently front-end only (it shows a success message). A
  working `mailto:hi@kossov.it` link is provided beside it. To actually receive
  submissions, wire the `<form>` to an endpoint (e.g. a Cloudflare Pages Function,
  Formspree or Web3Forms).
- **Social preview image:** Open Graph tags reference `assets/k-logo.svg`. For
  best results on social platforms, add a 1200×630 PNG and point `og:image` at it.
- **Model names / claims** (e.g. "GPT-5.5", "99.95% uptime", certification
  statuses) are marketing copy — keep them accurate and verifiable.
