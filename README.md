# Sharath B B — Portfolio Website

A single-file, static personal portfolio site built from the resume of **Sharath B B** (Senior Frontend Developer & E-Commerce Site Administrator, Titan Company Ltd. / Tanishq).

## Tech Stack

- **HTML5** — semantic single-page markup
- **Tailwind CSS** — official CDN (`https://cdn.tailwindcss.com`) for layout utilities, no build step
- **GSAP + ScrollTrigger** (jsDelivr) — preloader, hero intro, split-text reveals, scroll-scrubbed text, timeline progress, counters, tilt / magnetic effects
- **Lenis** (jsDelivr) — smooth scrolling, wired into ScrollTrigger; also drives anchor-link scrolling
- **Google Fonts** — `Instrument Serif` (display), `Manrope` (body/UI), `JetBrains Mono` (terminal)
- **Vanilla JavaScript** — no framework, no bundler; the hero terminal's typewriter is hand-written

No `npm install`, no build tooling. Open `index.html` directly in a browser, or deploy the folder as-is to any static host (GitHub Pages, Netlify, Vercel, Cloudflare Pages, etc.). For local preview, `npx serve .` also works.

## Files

| File | Purpose |
|---|---|
| `index.html` | The entire site — markup, styles, and scripts in one file |
| `Sharath_BB_Resume.pdf` | Linked from the "Resume" / "Download Resume" buttons (must stay next to `index.html`, same filename) |
| `README.md` | This file |

## Structure of `index.html`

1. `<head>` — meta, fonts, Tailwind CDN + config, the Three.js import map, then a `<style>` block with the **design token system** and component classes (`.card`, `.chip`, `.btn-primary`, `.display`, etc.)
2. `<body>` — preloader → progress bar → backdrop (aurora blobs, film grain) → floating nav → `<main>` with hero (headline + typing terminal) → skills marquee → about → experience (timeline) → skills matrix → key impact → contact → footer
3. One classic `<script>` — all UI and animation: theme toggle, mobile menu, anchor scrolling, nav highlighting, contact form (emails via FormSubmit), hero terminal typewriter, GSAP/ScrollTrigger/Lenis setup

## Hero Terminal

The hero terminal (`#termBody`) types out a short profile and a fake `npm run deploy:campaign`, then loops. To change what it types, edit the `script` array inside `startTerminal()` — `cmd` lines are typed after a `$` prompt, `code` lines are arrays of `[cssClass, text]` segments (`k` keyword, `v` variable, `y` key, `s` string, `p` punctuation, `n` literal), and `out` lines appear instantly. The terminal stays dark in both themes. If you add or remove lines, update the `19` in `.term-body`'s `height: calc(19 * 1.65em + …)` so the box doesn't jump while typing.

## Motion & Graceful Fallbacks

- If GSAP/ScrollTrigger fail to load, or the visitor has `prefers-reduced-motion: reduce`, all animation is skipped: content shows immediately, counters show final numbers, the loader is hidden, and the terminal renders fully typed.
- A CSS failsafe hides the preloader after 7 seconds no matter what.
- Tilt, spotlight, magnetic buttons and the cursor ring only run on devices with a fine pointer (mouse/trackpad), not touch.
- Switching theme uses the View Transitions API for a circular wipe where supported (Chromium); other browsers switch instantly.

## Design System (Light/Dark Theme)

Colors are CSS custom properties defined once in `:root` (light) and overridden under `html.dark` (dark). The default is **dark**; the visitor's choice is saved in `localStorage` (`theme`).

```css
:root {              /* light mode */
  --bg: ...;
  --text: ...;
  --accent: ...;
}
html.dark {           /* dark mode overrides */
  --bg: ...;
  --text: ...;
  --accent: ...;
}
```

To change the accent color or palette, edit those two blocks (the terminal's syntax colors are fixed in the `.term-body` rules).

## Editing Content

Everything is plain HTML — search for the text you want to change in `index.html`:

- **Hero headline/summary** — inside `<section id="top">`; the headline is three `.line > .line-in` rows; the floating tags and info card sit around the terminal
- **Marquee items** — the `.marquee-item` block under the hero (the list appears twice for a seamless loop; edit both)
- **About text** — inside `<section id="about">`; the large scroll-highlight paragraph is `data-scrub`, headings with `data-split` get the word-reveal effect
- **Work history** — inside `<section id="experience">`, one `<article class="job">` per role (all details are always visible)
- **Skills Matrix** — inside `<section id="skills">`, one `.skill-card` per category with a `<span class="chip">` per skill/tool
- **Stats** — inside `<section id="impact">`; each `<span class="counter" data-target="N">` counts up from 0 to N on scroll
- **Contact info** — inside `<section id="contact">`, and duplicated in the hero and footer

### Current Skills Matrix accuracy notes

These were deliberately worded to match Sharath's actual hands-on level (not overstated):

- **PSI** — reports issues and assigns them to developers; does not personally implement performance fixes
- **Git** — basic only (commit / push / pull)
- **Jira & Confluence** — ticket creation and documentation only, not workflow administration
- **Figma & Photoshop** — minor asset editing (e.g. cropping), not full design work
- **SFCC** — owns Business Manager configuration end-to-end; no hands-on dev-side tools (ISML/SFRA, etc.)
- **Interaction Studio** — basic awareness only
- **Bitly** — built a custom internal tool on top of the Bitly API (link shortening + QR generation) — a genuine technical highlight, kept prominent
- Not included: Google Analytics (GA4), Google Tag Manager, Google Search Console — no hands-on experience with these

If skills or experience change, update the relevant `<span class="chip">` text directly. The marquee deliberately lists only the strongest, accurate capabilities.

## Deployment

Fully static — any static host works:

- **GitHub Pages**: push this folder to a repo, enable Pages on the `main` branch
- **Netlify / Vercel**: drag-and-drop the folder, or connect the repo — no build command needed
- Keep `index.html` and `Sharath_BB_Resume.pdf` in the same directory

## Browser Support

Uses modern CSS (custom properties, `backdrop-filter`, `color-mix()`, CSS Grid) and `async/await`. Works in current Chrome, Edge, Firefox, and Safari. `backdrop-filter` and the View Transitions theme wipe degrade gracefully on older browsers.

## Contact Form

The form posts to [FormSubmit](https://formsubmit.co) (`FORM_ENDPOINT` in the script), which emails each submission to the address in that URL. No server or API key is needed. Spam is limited with a hidden honeypot field. If sending fails, the page shows a direct `mailto:` link instead.

**One-time activation:** the first real submission makes FormSubmit email a confirmation link to that address. Click it once and all later messages arrive normally. After activating, FormSubmit also gives you a random-string endpoint you can swap into `FORM_ENDPOINT` so your email address isn't in the URL.
