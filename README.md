# joanna-obrebska.pl — Angular rebuild

Angular 22 + Tailwind CSS v4 rebuild of [joannaobrebska.pl](https://joannaobrebska.pl/).
Recreated the existing design 1:1 first, then fixed what was broken about it.

## Layout

```
web/          Angular app (the site)
tools/        Playwright verification scripts
```

## Commands

Run from the repo root:

| Command | What it does |
|---|---|
| `npm install` | Installs root tooling **and** the web app (postinstall) |
| `npm start` | Dev server → http://localhost:4310 |
| `npm run build` | Production build → `web/dist/web/browser` |
| `npm test` | Unit tests (Vitest via Angular) |
| `npm run verify` | 29 e2e checks against a **running** dev server |
| `npm run parity` | Diffs computed styles vs the live site |
| `npm run format` | Prettier write |
| `npm run lint` | Prettier check (format:check) |
| `npm run check` | test + build |
| `npm run check:all` | test + build + verify |

`verify`, `parity`, `shoot`, `slice`, and `mobile` need `npm start` running first.
Output lands in `tools/shots/`, `tools/slices/`, `tools/parity.json` — all gitignored.

## Content

Everything editable lives in **`web/src/app/content.ts`**. You should not need to
touch a template to change copy.

### Config switches

```ts
SITE.bookingUrl    // set → "Umów rozmowę" opens the calendar instead of scrolling
SITE.formEndpoint  // set → form POSTs here; unset → opens mail client (honest about it)
```

Both default to `null`, which is why the contact form currently hands off to the
visitor's mail app rather than claiming to have sent anything.

### Placeholder content — read this before launch

Content that must be replaced with real data is **flagged in code and labelled in
the UI**, so nothing invented reads as fact:

| Item | Flag | Renders as |
|---|---|---|
| Testimonials | `TESTIMONIALS[].isPlaceholder` | "Przykładowa treść — do podmiany na prawdziwy cytat" badge |
| Testimonial photos | `TESTIMONIALS[].photo` | stock faces from randomuser.me |
| Project metrics | `PROJECTS[].isSample` | "przykładowa skala" beside each number |
| Privacy policy | — | draft with a visible TODO; **needs legal review** |

Names, quotes, and metrics currently in the file are **realistic samples written
to demonstrate the layout**, not claims about real clients.

## Design tokens

Colours, fonts, and layout values are ported 1:1 from the live site's inline styles
and live in the `@theme` block in `web/src/styles.scss`. Do not change them to
"improve" the palette — they are the design.

One deliberate quirk is reproduced: the live WordPress/Elementor kit forces **all
links into the serif display face** (`.elementor-kit-70 a { font-family: Fraunces }`).
It looks like a bug, but matching the original required keeping it. It is commented
in `styles.scss`.

## Verified

`npm run verify` covers: all routes, form validation and its real submission
behaviour, the testimonial carousel, card→detail routing, absence of dead `#`
links, SEO/meta tags, horizontal overflow at 1440/820/390/320, the mobile drawer,
44px tap targets, and console errors.

Last full run: **29/29 pass**, production bundle **90.85 kB gzipped**.

## Known gaps

- No deployment setup — nothing is published, this is a dev server only.
- Blog is a placeholder; no articles yet.
- Cookie banner (on the live site) is still unaddressed here — no banner is
  implemented at all in this rebuild.