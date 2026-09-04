# Pelvic Organ Prolapse Support Society, Website

A static, accessibility-first information and support site built with [Astro](https://astro.build).

## Running it

```bash
npm install
npm run dev      # local dev server at localhost:4321
npm run build    # static output to ./dist
npm run preview  # preview the production build
```

## Deploying

The site builds to fully static HTML, no server needed.

**Vercel:** import the repo, framework preset "Astro", deploy. Nothing else to configure.
**Netlify:** build command `npm run build`, publish directory `dist`.

Before launch, set the real domain in `astro.config.mjs` (`site:` field) so canonical URLs and any future sitemap are correct.

## Structure

```
src/
  layouts/Base.astro      Header, nav, text-size control, closing CTAs, footer disclaimer
  styles/global.css       All design tokens and styling
  pages/
    index.astro           Home
    understanding.astro   Understanding Prolapse
    symptoms.astro        Symptoms + self-check tool
    questions.astro       Common Questions (FAQ)
    community.astro       Community / group join
    resources.astro       External resources
    about.astro           About & contact
    treatment/
      index.astro         Treatment hub
      non-surgical.astro
      surgery.astro
      recovery.astro
      recurrence.astro
```

## Design system

Editorial and print inspired, chosen deliberately over the usual card grid:

- **One centre axis.** Every band is full bleed; every inner wrapper shares `--page` (60rem) or `--measure` (62ch) and centres with `margin-inline: auto`. Nothing is left pinned inside a wider parent.
- **Measure in `ch` and `rem`**, so the column scales with the text size control and characters per line stay roughly constant at all three sizes.
- **Hairline rules instead of floating cards.** Near zero corner radius, no drop shadows, no hover lift transforms.
- **Numbered indexes.** Entry numbers come from a CSS counter, so markup stays clean.
- **Fixed column counts** (3 up on the home index, 2x2 on treatment) so a row never orphans a single item.
- **Two colours used with discipline**: forest for structure, clay for accent. Every foreground and background pair passes WCAG AA.

## Accessibility decisions

The audience skews older, so these are deliberate, not incidental:

- **20px base font size** (most sites use 16px), with a persistent **text-size control** in the utility bar offering 20 / 22 / 24px. The choice saves to `localStorage` and applies on every page.
- **Atkinson Hyperlegible** body typeface, designed by the Braille Institute specifically to increase character distinction for low-vision readers.
- Line length capped at 62 characters; line-height 1.7 on body copy.
- All tap targets at least 44x48px.
- Colour contrast meets WCAG AA throughout (verified; lowest pair is 6.07:1). The focus ring is a high-contrast blue that never blends into the palette.
- Skip-to-content link, semantic landmarks, `aria-current` on the active nav item, real `<label>` elements wrapping every checkbox.
- `prefers-reduced-motion` respected.

## House style

- **No em dashes or en dashes anywhere in body copy.** Use a comma, a colon, or a full stop instead. Numeric ranges are written out ("30 to 40 percent"), not hyphenated. Ordinary hyphens in compound words are fine.
- Plain, direct, second person. Short sentences are welcome.
- Avoid rhetorical scaffolding and three-part lists used for rhythm rather than meaning.

## The self-check tool

`src/pages/symptoms.astro`. Deliberately simple and deliberately limited:

- Runs entirely in the browser. **Nothing is stored, sent, or logged**, no analytics on answers, no email gate. The page says so to the visitor.
- Never returns a diagnosis, score, or percentage. Three or more checked items shows the "worth a conversation" result; fewer shows the "can't rule anything out, trust yourself" result. Both point to a healthcare provider.
- Framed as "Is it worth talking to someone?" rather than "Do I have prolapse?"

If you ever add analytics to this site, **do not** track individual checkbox answers. Page views only.

## Content

All copy comes from the approved content pack (`POPSS-Website-Content-Pack.docx`), drafted from Cleveland Clinic, peer-reviewed urogynecology journals, Cochrane reviews, and patient advocacy sources.

The medical disclaimer appears in the footer of every page via the layout, don't remove it from `Base.astro`.

## Before launch, outstanding items

- [ ] Confirm contact email (`src/pages/about.astro` has a placeholder)
- [ ] Confirm the Facebook group join link is final (currently in `Base.astro` and `community.astro`)
- [x] Favicon added (`public/favicon.svg`)
- [ ] Add an Open Graph share image (`public/`). Group members will share links on Facebook, so the OG preview matters more than usual here
- [ ] Add member stories if admins supply them with written permission
- [ ] Admin sign-off on the disclaimer wording
