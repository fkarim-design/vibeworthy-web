# vibeworthy-web

Pitch deploy for **Vibe Worthy** (vibeworthy.co) — Kalpna Tandon's culture
marketing agency, London. The front page is her own site, captured whole and
served from this repo; the pitch adds her creatives at the foot of that page
and the offer one tab away. Packaged as a Cloudflare Workers static-assets
site.

| Route | Page |
| --- | --- |
| `/` | Her original site — the owner-supplied capture of vibeworthy.co, every asset vendored |
| `/#people` | The talent roster, at the foot of the front page (`/creatives/` forwards here) |
| `/creatives/<name>/` | Per-creative pitch page + booking enquiry (Naomi, Nee, Poonam) |
| `/offer/` | The offer — tale of the tape and what the new site already does better |

## Structure

```
public/                everything served — no build step
  index.html           her original page (see "The original copy" below)
  assets/rm/           its vendored assets — all images (client logos, her
                       photo, the wordmark), viewer.css, google-fonts.css
  creatives/           per-creative booking pages and their photos
  offer/index.html     the offer
  assets/css/          site.css · offer.css · creatives.css (pitch pages)
  assets/js/           booking.js — calendar picker + mailto assembly
  fonts/               self-hosted Lora + Poppins (pitch pages)
  404.html  favicon.svg  og.png  robots.txt  _headers
wrangler.jsonc         assets-only config, no Worker script
```

## The original copy

`/` is a static rendition of the live Readymag page, built from the owner's
"Webpage, Complete" save. Every image is served locally from `assets/rm/`.
The Readymag runtime was removed — it cannot run against a saved DOM without
their servers. A small inline layer (the `static-canvas-*` style/script at
the end of `index.html`) does what the runtime did for layout and motion:
gives the canvas its height, scales it to fill narrower desktop viewports,
caps it at its natural 1375px — centred, type constant — on wider ones, and
rebuilds the scroll choreography (the logo's shrink into a pinned masthead,
the fading welcome line, the running marquees, the nav pills) from the
captured animation tracks. A desktop window of any width keeps that canvas,
scaled, as her own site does; only touch phones under 768px get
`#phone-render`, a hand-stacked phone rendition of the same content — her
copy, images and colours verbatim, set in her own type map, with her client
wall as her own phone layout's 3 × 5 hairline grid (her live site serves its
own phone layout, which a desktop save cannot contain, so it was measured
from her live phone site). On phones that allow motion,
`phone-motion-js` replays her phone layout's scroll choreography, measured
from her live phone site: the shrinking wordmark, the Services panel's
sweep, headings and lines rising into place, the process cards gliding
across on her staircase and the shooting star at the foot. Phones that ask
for reduced motion keep the still rendition. The live original stays one
click away on the demo bar.

## Local development

```bash
npm install
npm run dev          # wrangler dev
```

## Deployment

Connected to Cloudflare Workers Builds: every push to `main` deploys to
production. Verify before every push (`npm run check`, then render and inspect
screenshots). This is a pitch demo: `robots.txt` disallows everything and every
page carries `noindex,nofollow` — never lift either while it lives on a
temporary link.

## Social previews

Every page carries Open Graph + Twitter-card meta pointing at `/og.png`
(1200×630 — her real wordmark on her cream) by absolute URL on the workers.dev
deploy. On transfer to the client's domain, re-point those `og:image` URLs.

## External resources

- **Fonts on `/`**: her real faces — FreightTextCmp Pro, Poppins, Sweet Sans
  Pro and Neue Haas Grotesk Text — load from Adobe Fonts (use.typekit.net)
  through our own web project, kit `dhe6bjp`, licensed for
  vibe.fkarimdesign.com, the workers.dev address, vibeworthy.co and
  localhost, embedded with Adobe's standard
  stylesheet link. Her styles' font stacks name the licensed family first,
  her captured face second, and a self-hosted Lora / Poppins stand-in
  (`vw-serif`, `vw-sans`) last, so the page stays legible if Adobe is
  unreachable. Inter and Pinyon Script load from fonts.gstatic.com via the
  vendored `assets/rm/google-fonts.css`.
- The pitch pages (`/creatives/`, `/offer/`, 404) self-host Lora + Poppins
  from `/fonts/`.
- The demo bar's last link opens `https://vibeworthy.co/` in a new tab; the
  LinkedIn and social links on the pitch pages are plain anchors. Nothing is
  fetched or framed from vibeworthy.co by any page.
- The booking form runs entirely client-side: `booking.js` assembles the brief
  into a `mailto:` for the visitor's own email app. Nothing is stored or
  POSTed anywhere.
