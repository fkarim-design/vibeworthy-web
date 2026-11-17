# CLAUDE.md

Standing policy for this repository. Read it before making any change here.

## What this repo is

A Cloudflare Workers static-assets site — the **pitch deploy for Vibe
Worthy**. Since v1.7 the front page `/` is **her original site**: the
owner-supplied "Webpage, Complete" capture of vibeworthy.co, vendored whole.
The pitch adds her creatives as `#people` at the foot of `/` (with booking
pages under `/creatives/<name>/`) and the `/offer/` page one tab away. Everything served lives in `public/` and there is no build step - the
files in that directory are the site. The repo is connected to Cloudflare
Workers Builds, so **every push to `main` deploys to production**. The
demo is served at **vibe.fkarimdesign.com** and at
vibeworthy-web.cloudflare-passport599.workers.dev — one Worker, byte-identical
responses. Any new address must also be added to the font kit's domains.

```
public/            everything served
  index.html       her original page (vendored capture — see rules below)
  assets/rm/       its assets: all images (client logos, her photo, the
                   wordmark), viewer.css, google-fonts.css
  creatives/       per-creative booking pages (the roster itself lives on /
                   as #people; /creatives/ just forwards there)
  offer/           the offer page
  assets/css       site.css · offer.css · creatives.css (pitch pages)
  fonts/           self-hosted Lora + Poppins (pitch pages)
  og.png           social-sharing card, 1200×630, her wordmark on her cream
  _headers         security + caching headers
  robots.txt       DISALLOW ALL — pitch demo, never indexed
wrangler.jsonc     assets-only config, no Worker script
package.json       wrangler devDependency + dev/deploy scripts
```

## Pitch-demo rules

- `robots.txt` disallows everything and every page carries
  `noindex,nofollow`. Keep both until the site transfers to the client's own
  domain; on transfer, the demo bar, the `/offer/` route and the noindex
  machinery all come out.
- `/` is **her page, verbatim** (v1.7, "just take the original" — the owner's
  instruction, 25 Aug 2026): a static rendition of the Readymag DOM she saved,
  with asset paths localised to `assets/rm/`, our og/description meta and the
  demo bar injected, the Readymag runtime and its `__RM_PROPS__` blob removed,
  and a small `static-canvas-*` style/script layer at the end of the file
  doing what the runtime did for layout and motion (canvas height 4372 ×
  width 1375, scaled to fill narrower desktop viewports and capped at its
  natural size — centred, type constant — on wider ones; the hero's
  logo-to-masthead scrub, the pinned masthead with working nav pills and the
  marquees and the process cards' scroll-driven slide rebuilt from the
  captured animation tracks). Do not edit her DOM, copy or images (the one
  sanctioned exception: her `font-family` declarations, rewritten as below).
  A desktop window of any width keeps her canvas, scaled, exactly as her
  own site does (the owner's side-by-side comparison, 15 Nov 2026). Only
  touch phones — `(max-width:767px) and (pointer:coarse)`, one predicate
  shared by `phone-render-css`, `PHONE` in `static-canvas-js`,
  `phone-motion-flag`/`phone-motion-js` and the phone rule in `people-css`
  — get
  `#phone-render`: our hand-stacked phone rendition of the same page, her
  copy, images and colours verbatim, typeset from her measured type map
  (FreightTextCmp for nearly everything, Poppins for the small print,
  Pinyon Script numerals). Its client wall IS her phone wall (v1.26,
  measured from her live phone site): "Client Experience" at her size,
  then her 3 × 5 grid in her phone order on her design grid (unit
  0.3125vw; the wall 277 × 452 units, 22 in), her 0.3-unit hairline rules
  drawn between the cells only, and every logo sized and placed so its ink
  lands within a pixel of hers. Its hero IS her live phone hero: rebuilt
  from the owner's iPhone screenshot of vibeworthy.co (16 Nov 2026) and
  positioned in screen-width units, so every element lands within a point
  of hers on any phone. The sections below the hero are still our
  restacking, but their type is hers: sizes and weights for About, The
  Process, Values, Contact, the ticker and the footer were measured from
  her phone screenshots (16 Nov 2026; face, weight and size inferred from
  each line's ink width and height against the real font metrics) and set
  in screen-width units — the About text breaks on the same words as hers.
  Services and Founder are not yet matched to her phone layout, nor are
  her other section layouts (three separate values boxes, the ticker's star
  separators, her services order). **Phone motion** (v1.24, the
  owner's "bring back the sliding scroll bits … like the original"): her
  live phone site renders in headless Chromium at 402 × 874 (touch) once
  the environment allows vibeworthy.co, st-p.rmcdn1.net, c-p.rmcdn.net,
  c-p.rmcdn1.net, i-p.rmcdn.net and fonts.googleapis.com; scrolling its
  `.content-scroll-wrapper` and reading each widget's box gave her whole
  scroll choreography (16 Nov 2026), which `phone-motion-js` replays on
  `#phone-render`. As on hers, every move is scrubbed by the scroll
  position with easeInOutQuad and starts the moment the element's top
  reaches the bottom of the screen, and every distance and scroll length
  is in screen-width units (confirmed at 402 and 360 wide): the wordmark
  shrinks to a third and "Welcome to" fades from the top of the page; her
  navy (`#091f92`) Services panel sweeps in from the right; headings, text,
  service lines, logo rows and values rise and fade in; the process cards
  sit on her staircase (68.75 × 80vw, each 71.4vw right and 42.8vw down)
  and slide 932.5vw left over 619.4vw of scroll (each card's line breaks
  where hers does and never re-wraps: her first lines fill their boxes to
  within 1–2px, and iPhone's slightly wider text measure pushed card 2's
  last word out of the card, v1.25); a shooting star crosses
  above the closing ticker — since v1.27 in a lane cut to 11vw under the
  contact lines (the owner's "cut these gaps"), flying only once those
  lines have settled so it never crosses them. `phone-motion-flag` sets `html.pr-motion`
  before the rendition parses; without it — reduced motion, no script, an
  error — the still, stacked rendition stands. The direct call to her
  widget API was refused as unauthorised access: measure the rendered page,
  never her API. Below her page — after the canvas and
  the phone rendition alike — sits `#people`, our pitch-added roster of her
  three creatives (owner request, 25 Aug 2026) linking to their
  `/creatives/<name>/` booking pages; `/creatives/` itself forwards to
  `/#people`. On phones the sign-off — her wordmark with "Vibe Worthy |
  London" and the email under it — is `#phone-foot`, the last thing on the
  page, below `#people` (owner request, 16 Nov 2026), and `#people` takes
  the phone rendition's 44px rhythm; desktop keeps her canvas footer. **Fonts:** her kit faces are FreightTextCmp Pro = `qhhw`,
  Poppins = `thsk`, Sweet Sans Pro = `tgzb`, Neue Haas Grotesk Text =
  `wtqc` (confirmed: our kit serves the very same Adobe font files her page
  links). Our Adobe Fonts web project, kit `dhe6bjp` (licensed for
  vibe.fkarimdesign.com, the workers.dev address, vibeworthy.co and
  localhost; it replaced `xmc1iac`, which lacked the new address), is
  loaded with Adobe's
  standard `<link>` in the head — no script. Every `font-family` in her
  styles reads licensed family first, her captured face second, stand-in
  last, unquoted so inline style attributes stay valid:
  `freighttextcmp-pro,qhhw,vw-serif` · `poppins,thsk,vw-sans` ·
  `sweet-sans-pro,tgzb,vw-sans` · `neue-haas-grotesk-text,wtqc,vw-sans`.
  The Lora/Poppins stand-ins in `#fallback-faces` live under their own
  names (`vw-serif`, `vw-sans`). **Never declare stand-ins under her family
  codes:** a later same-name face overrides her real one outright — that is
  what kept her type off the live page from v1.15 on. Deploys stopped after v1.17 when the repository moved to fkarim-design/vibeworthy-web and resumed once it was reconnected (v1.21, 16 Nov 2026): check the live page actually changed after every release. The one-line width
  patches in `static-canvas-js` apply only when the stand-in serif is what
  loaded. Her captured Typekit rules stay in the head untouched. Inter +
  Pinyon Script still load remotely via gstatic. Verified 15 Nov 2026 in
  Chromium on localhost: every face 200 from Adobe, stand-ins unused.
- The pitch pages (`/creatives/`, `/offer/`, 404) keep the brand-matched look
  of v1.6 (her blues and cream, Lora + Poppins standing in for her faces).
  Do not change that look without being asked. On those pages the one-line
  explanation under each service card is our placeholder copy — not her
  wording.
- The demo bar (tabs: The site · The offer · Current site ↗) is
  chrome on every page except the 404; its last link goes out to her live
  site in a new tab. Keep it a plain anchor: nothing is fetched or framed
  from vibeworthy.co by any page.
- **No rates anywhere on /creatives/** — the client's explicit instruction.
  Booking is an enquiry scoped to the brief; never add prices, day rates or
  budget menus to those pages.
- Creative profiles (names, handles, work links, artist lists, photos) come
  from the client's own deck (VW_Deck_-_AK_2.pdf, 24 Aug 2026) and stay
  verbatim. The deck's Ama Louise link was broken (pointed at apple.com), so
  that credit is unlinked pending the real URL.
- **No prices on the offer page** (owner instruction, 26 Aug 2026): it sells
  the improvements — the tale of the tape, the "Already in the demo" list and
  ownership — and money is discussed off-page. The "Already in the demo" list
  claims only what the demo verifiably does; keep it true when the front page
  changes.
- `og.png` is the social-sharing card, referenced from every page's `og:image`
  by absolute URL on the workers.dev deploy. On transfer to her domain,
  re-point those URLs; regenerate the card if the site's look changes.

## Local development

```bash
npm install
npm run dev          # wrangler dev
```

## Verification - before every push to main

1. `npx wrangler deploy --dry-run`
2. Serve `public/`, render it with headless Chromium, and inspect the
   screenshots: styles applied, fonts loaded, layout intact.

Never leave pushed work unverified or half-finished. Work in small, complete
batches: implement, verify, commit, push.

## Git and release workflow

- Before committing: `git config user.name "Fid" && git config user.email "fid_kk@proton.me"`
- Develop on the working branch and push there first. Release verified work by
  fast-forwarding `main` onto it and pushing `main`.
- Every push to `main` is a release. Versions are an ascending `vMAJOR.MINOR`
  sequence starting at `v1.0`; every push bumps the minor regardless of size. A
  major bump is reserved for a ground-up overhaul.
- With every push to `main`, provide release-tag text in the reply, in exactly
  this shape. The owner creates the GitHub release manually - **never push tags**:

  ```
  Tag: v<next>  —  Title: <five to nine words, plain and evocative>
  Description: <one to three sentences of editorial prose describing what changed
  from the owner's point of view — outcomes, not implementation. No bullet lists,
  no jargon, no file names.>
  ```

- Append the release line to the ledger below as part of the same push.
- Commit messages: descriptive imperative first line (what the change does, not
  "update X"), then a short prose body; dash bullets are fine there. One commit
  per coherent piece of work; several may share a push, but each push gets
  exactly one version entry.
- Never include model names, AI attribution trailers, session links, or other
  tooling identifiers in commit messages, titles, or code.

## The page itself

Content, design, and behaviour are as supplied by the owner. Do not tidy markup,
rename classes, rewrite copy, or modernise CSS unless asked - changes to the
design are their own release, requested deliberately.

## Release ledger

| Version | Title | Description |
| --- | --- | --- |
| v1.0 | The Vibe Worthy pitch demo, complete | A new one-page site carrying her eight services and the wall of fifteen clients she has worked with, alongside an offer page with the £500/£50 terms — verified at thirteen widths and ready to connect to Cloudflare. |
| v1.1 | The demo dressed in midnight and gold | The whole demo now wears the night-sky look from the astrology page — deep midnight blue with old gold, elegant serif headlines and a quiet scatter of stars — from the front page through each creative's booking form to the offer. |
| v1.2 | Back to raspberry on warm paper | The night-sky experiment is undone at the owner's word: the demo returns to its original look — warm paper, raspberry and bold condensed headlines — exactly as it first shipped, from the front page through the booking forms to the offer. |
| v1.3 | A proper card when the link is shared | Sharing any page of the demo in a chat or a post now shows a real preview — the Vibe Worthy name in its bold house look on warm paper with the raspberry stripe — instead of a bare link. |
| v1.4 | A stray file swept back out | A scrap of tooling debris that slipped into the previous release is removed; nothing about the site itself changes. |
| v1.5 | The demo now wears her own blue | The whole demo now dresses in Vibe Worthy's own branding — the deep royal blue and soft cream of her site, with elegant serif headlines to match — across the front page, the booking pages, the offer and the sharing card. |
| v1.6 | Matched against the real thing | With her live site finally in view, the demo now carries its true character — the soft serif headlines, the italic titles, the airy rounded outlines on pale grey, and a wordmark that ends in her star — across every page and the sharing card. |
| v1.7 | Her own site takes the front page | The front page is now Kalpna's actual site, captured whole and hosted with us — her wordmark, her photo and all fifteen client logos served from our own files — with the creatives and the offer one tab away, and the sharing card carrying her real logo. |
| v1.8 | Another scrap swept out the door | A leftover screenshot from the build tooling is removed from the repository; it never reached the site and nothing about it changes. |
| v1.9 | The front page scrolls again | Her page arrived with scrolling switched off — its old platform handled scrolling itself, and without it the site froze on the first screen. The page now scrolls like any other, top to bottom, on desktop and phone alike. |
| v1.10 | The pictures come back on every phone | The captured images had been saved under the wrong file type, which stricter browsers — iPhones especially — refuse to show, leaving the page full of empty frames. Every image is now served as what it truly is, and the client wall carries the crisper logo files she supplied. |
| v1.11 | The front page moves like the real thing | The hero opens as it should — Welcome to, the big wordmark, the definitions — then the logo glides up and shrinks into a pinned masthead as you scroll, the marquees run again, and the masthead's About, Services and Contact buttons carry you to their sections. |
| v1.12 | The ticker reads clean again | The running ticker's little star separators had drifted onto the words themselves, striking through the text; they are retired, and the line now scrolls past unobstructed on both bands. |
| v1.13 | A right size for every screen | On big monitors the page no longer inflates — it sits centred at its true size with the type held steady. And phones now get a page built for them: the same words, logos and colours restacked into one readable column instead of a shrunken desktop. |
| v1.14 | The cards glide and the people move in | The process cards now slide across as you scroll, the way her original moves. Her three creatives now live at the foot of the front page itself — bookable in a tap — and the offer page carries a plain list of what the demo already does that the current site doesn't. |
| v1.15 | The type finds its voice again | Her font service refuses to dress the demo's address, so every headline had been falling back to a plain default face. The whole page — headlines, tickers, cards and all — now wears the site's own elegant serif and clean sans, with each line held to the width it was drawn for. |
| v1.16 | The offer speaks only of the work | The demo bar slims to two tabs — the site and the offer — now that the people live on the front page itself. And the offer page no longer quotes a price: it makes the case on the work alone, with money left for the conversation. |
| v1.17 | Every face fits its frame | The three portraits at the foot of the front page now sit properly in their windows — each crop anchored where the face is — so nobody loses their head on a phone or a monitor. |
| v1.18 | The real typefaces take the stage | The demo now wears her actual faces — the compressed Freight serif, Poppins, Sweet Sans and Neue Haas — served legitimately through our own Adobe Fonts licence covering this address and hers. Wherever that service can't reach, the stand-ins stay on without a seam. (It never reached the live site: deploys stopped after v1.17, when the repository moved; v1.19 replaced it.) |
| v1.19 | Her own type, her own grid, every face framed | Her real typefaces now load the standard way — the compressed Freight serif, Poppins and the script numerals of the process cards — on desktop and phone alike, and a narrowed desktop window keeps her own composition, scaled, just like her site. On phones, the client wall is her ruled five-by-three grid at her exact logo proportions, and the three portraits sit with every head whole. |
| v1.20 | The fonts follow the demo to its new home | The demo's new address, vibe.fkarimdesign.com, is now covered by the font licence, so her real typefaces load there just as they do everywhere else the demo lives. |
| v1.21 | Her phone welcome, line for line | On phones, the opening screen now matches her own site's: Welcome to above, the big wordmark, a definition at each corner joined to it by her fine rules and dots, and the tagline beneath, all in her type and at her proportions on every size of phone. |
| v1.22 | A safe fit when her fonts can't load | If a phone's browser blocks her font service, the opening screen now sets its definitions and tagline a size smaller so every line stays on screen; with her fonts in place, nothing changes. |
| v1.23 | Her phone type, measured line by line | On phones, the headings, the About text, the process cards, the values and the contact lines now use her own sizes and weights, measured from her live site, so the About text even breaks on the same words as hers; the ticker and footer come down to her quieter size. |
| v1.24 | Her phone page moves the way hers does | On phones, the page now moves as you scroll just as her own phone site does: the wordmark shrinks away, the Services panel sweeps in from the side, headings and lines rise into place, the four process cards glide across on her diagonal, and a shooting star crosses at the foot. Phones set to reduce motion keep the still page. |
| v1.25 | Every process card holds its two lines | On phones, each process card's line now always sits on two lines, breaking exactly where hers does, so the Strategy & Planning card no longer pushes its last word out of the bottom on iPhones; the words and their size are unchanged. |
| v1.26 | Her client wall, the way phones see it | On phones, Client Experience now matches her own phone site: her larger heading, then her fifteen logos in three columns and five rows in her order, divided by her fine hairlines, each logo sitting exactly where hers does, with the rows rising into place as you scroll. |
| v1.27 | A tighter close, signed off at the foot | On phones, the end of the page is tighter: the empty space under the contact email and the gap above the creatives are gone, the shooting star now crosses the trimmed space once the contact lines have settled, and the Vibe Worthy sign-off with her London and email line now closes the page beneath the creatives. |
