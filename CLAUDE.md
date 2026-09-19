# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Static website for Stichting Devoxx4Kids Nederland, built with Hugo (extended). No theme and no npm/JS toolchain: all templates live in `layouts/`, and the one stylesheet is `assets/css/main.css` (minified and fingerprinted via Hugo Pipes in `layouts/partials/meta.html`). `assets/css-not-used/` is dead code. Site content is written in Dutch (`locale = 'nl-NL'`); use `time.Format` rather than `.Format` for dates so month and day names come out in Dutch.

## Design system

The look follows the Devoxx4Kids Netherlands design system (https://claude.ai/artifact/2S599ykmqUXadysx2wek3D). `main.css` starts with its tokens as CSS custom properties (light theme on `:root` is always the default; the dark theme is `:root[data-theme="dark"]`, set by the header toggle in `layouts/partials/header.html` and remembered in `localStorage` under `d4k-theme`, restored before first paint by a script in `meta.html`) and reuses its component classes with the `d4k-` prefix: `d4k-hero`, `d4k-btn`, `d4k-badge`, `d4k-card`, `d4k-alert`, `d4k-event`. Key rules from the system:
- `brand` orange is a fill only, never text; links and legible orange use `brand-deep`; text on orange is `on-brand`.
- Controls and blocks get a 2px `border-strong` outline and a hard `shadow-pop` offset (no blurred shadows); spacing only in `space-*` steps.
- Fonts: Outfit (display/headings), Nunito (body), JetBrains Mono (code), loaded from Google Fonts.
- Copy addresses the reader as "je", uses sentence case, writes ages as "8 t/m 14 jaar", and never promises that a place is reserved or that something costs money.
- Voice: workshop descriptions and day schedules talk to the child ("je", one action per sentence, 18px/30px body-l); practical info (location, times, registration) stays informative for parents.
- Workshop illustrations in `static/images/illustrations/` are cartoon SVGs with fixed brand colours and a 4px black outline; they sit on a fixed light plate so they work in dark mode too. Draw new ones in the same style rather than using product photos.
- The logo is `static/images/d4k-nl-horizontal.png`, used as supplied (white plate on dark backgrounds).

## Commands

- `hugo server` — local dev server with live reload (add `-D` to include drafts, `-F` to include pages with a future `date`)
- `hugo --minify` — production build into `public/` (git-ignored); this is what CI runs
- `hugo new content/events/YYYYMMDD-City-Company.md` — new event from `archetypes/events.md` (created with `draft = true`)

There are no tests or linters.

## Deployment

`.github/workflows/deploy.yml` builds on every push to `main` (or manual dispatch) using the latest Hugo extended, then pushes `public/` to the `gh-pages` branch of the **separate** repo `Devoxx4Kids-NPO/Devoxx4Kids-NPO.github.io` via the `SSH_KEY_GITHUB_IO` deploy key. `baseURL` in `hugo.toml` points to that GitHub Pages site. The site is not served from this repo.

## Architecture

- **Navigation** is defined in `hugo.toml` under `[[menu.main]]`, not in front matter. Add new top-level pages there.
- **Events** (`content/events/`, files named `YYYYMMDD-City-Company.md`, images in `static/images/events/`; README.md documents every front matter field) use two dates in front matter:
  - `date` — publish date (Hugo skips pages with a future `date` unless built with `-F`)
  - `eventDate` — when the event takes place; used for upcoming/past splitting and sorting
  - `location` (`name`, `note`, `address`, `postcode`), `startTime`, `endTime`, `contact` — the practical facts; `layouts/partials/event-facts.html` shows them in a panel in the hero with a Google Maps route link. Keep them out of the Markdown body, which is only for extra information
  - `city`, `host`, `ages` — shown on the event card and hero (card title becomes "Devoxx4Kids <city>")
  - `registration` — external sign-up URL; shows an "Aanmelden" button while the event is upcoming
  - `status` — optional `open` / `vol` / `binnenkort`, rendered as a badge; only set it when it is known
  - `modules` — module file names from `content/modules/`, resolved by `layouts/partials/event-modules.html` and rendered as linked module cards (`module-grid.html` / `module-card.html`)
  - `programma` — list of `tijd`/`titel`/`tekst`, rendered as a `Stepper` (`layouts/partials/day-programme.html`); all steps show as done once the event is past
  - `flyer` (organiser's flyer, own section with an "Open de flyer" button) and `photo` (+ optional `photoWidth` percentage; a picture from the day) — keep them apart, they get different context; `summary` is the hero lead
  - `hugo new content/events/YYYYMMDD-City-Company.md` uses `archetypes/events.md` with all of these fields
- Upcoming vs. past events are decided by comparing `eventDate` to `now` **at build time** (`layouts/events/list.html`, `layouts/partials/events-home-page.html`, `layouts/partials/event-card.html`). Because the site is static, an event only moves to "Vorige events" after the next rebuild/deploy. The home page (`layouts/_default/home.html`) shows the next 3 upcoming events.
- **Modules** (`content/modules/<id>.md`) are managed once and reused everywhere: front matter `title` (to the child), `tool`, `summary`, `image` (file in `static/images/illustrations/`), `weight`, `requirements`, `links`. `/modules/` shows the card grid (`layouts/modules/list.html`), each module has a detail page (`layouts/modules/single.html`) that lists the events using it, and the home page shows the first six. The old `/pages/modules/` URL is an alias. `hugo new content/modules/<id>.md` uses `archetypes/modules.md`.
- **Gastlessen** posts live in `content/posts/gastlessen/` with their own list/single layouts in `layouts/posts/gastlessen/`. The section page (`content/posts/gastlessen/_index.md`) is structured: `summary` (hero lead), `contact`, `modules` (resolved like an event's); the three promise cards and the request steps are in the template and follow the design system's Devoxx4Kids@School section — only those three promises may be printed. The hero uses the scene illustration `static/images/illustrations/gastles.svg`.
- **Event map**: the `{{< d4k_events >}}` shortcode renders a Leaflet map (loaded from unpkg) using `static/map.js`, with marker locations from `data/d4k_events.yml` (name/latitude/longitude). This list is maintained by hand and is independent of the event pages in `content/events/`.
- `layouts/_default/_markup/render-link.html` makes every Markdown link starting with `http` open in a new tab.
- PDFs (annual reports, policy plan for ANBI) are in `static/documents/` and linked from `content/pages/`.
- **Formal pages** (Stichting, Partners) use `layout: stichting` / `layout: partners` (`layouts/_default/stichting.html`, `partners.html`) with the `page-header.html` partial: no Hero band, flat cards, a `facts-panel`. Their data lives in `data/stichting.yml` (address, KvK, RSIN, IBAN, board, documents) and `data/partners.yml` (grouped partners) — edit those, not the templates. They keep the formal "u" the foundation used.
- **ANBI** (`layout: anbi`) renders `data/anbi.yml` + `data/stichting.yml` as label/value lists (`.kv`) and tables (`.data-table`); **Over ons** (`layout: overons`) has three linked tiles, the city list and map (`layouts/partials/event-map.html`, also behind the `d4k_events` shortcode), then the page text. `content/about.md` is an unlinked older copy of Over ons.

