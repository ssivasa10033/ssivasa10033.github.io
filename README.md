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

**Live at https://ssivasa10033.github.io/**

Deployment is automatic. Every push to `main` triggers
`.github/workflows/deploy.yml`, which runs `npm ci && npm run build` and
publishes `dist/` to GitHub Pages. Nothing to do by hand.

```bash
git add -A && git commit -m "your change" && git push
```

Watch a deploy with `gh run watch`. A build takes roughly a minute.

### Attaching a custom domain

The repo is named `ssivasa10033.github.io`, so the site serves from the **root**
of its domain. Every internal link is root absolute, which is why this matters:
a project repo would serve under `/repo-name/` and break all of them.

To move to a real domain later:

1. Add the domain in Settings, Pages, Custom domain on GitHub.
2. At your registrar, point the apex at GitHub with four A records:
   `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`.
   For a `www` subdomain use a CNAME to `ssivasa10033.github.io` instead.
3. Update `site:` in `astro.config.mjs` to the new URL.
4. Tick Enforce HTTPS once the certificate is issued.

No `base` config is needed at any point, because the site is at the root both
before and after the move.

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

- [x] Contact route decided: the About page points people to the Facebook group
- [ ] Confirm the Facebook group link is final. It now appears in three files: `Base.astro`, `community.astro`, and `about.astro`
- [x] Favicon added (`public/favicon.svg`)
- [ ] Add an Open Graph share image (`public/`). Group members will share links on Facebook, so the OG preview matters more than usual here
- [ ] Add member stories if admins supply them with written permission
- [ ] Admin sign-off on the disclaimer wording
