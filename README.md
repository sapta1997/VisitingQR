# VisitingQR

A single-page **digital visiting card** for Saptadeep Sharma — one link holding
every contact method, meant to be reached by scanning a QR code.

No build step, no framework. Two HTML files and a handful of assets.

## Live

**https://sapta1997.github.io/VisitingQR/**

Served by GitHub Pages from `main` / `/ (root)`. Every push to `main`
republishes within a minute or so.

The page lives at `index.html` so the QR encodes the shortest possible URL —
shorter URLs make a lower-density, easier-to-scan code.

## The card

`index.html` is a single poster image with four link buttons positioned on top
of it. The artwork carries the typography; the buttons are separate images.

- **Buttons are placed in percentages** of the poster (`left: 30.566%`,
  `width: 39.0625%`, `top` per button), so they stay locked to the artwork at
  every screen size. Their pill shape comes from `border-radius: 999px`
  clipping a rectangular image whose corners are opaque.
- **The poster always fits the screen whole** - `min(100vw, 100svh * 0.6667)`
  against a `1024/1536` ratio - so nothing is ever cropped.
- **102 KB of artwork**, down from 2.4 MB of source PNGs. WebP is served first
  with a JPEG fallback: `<picture>` for the buttons, `image-set()` for the
  poster, which is a CSS background.
- The poster's text is repeated in a visually hidden `<h1>` and every button
  has an `aria-label`, since text inside an image is invisible to screen
  readers and search engines.
- Motion is disabled under `prefers-reduced-motion`.

### Known trade-off: tap targets

Because the whole poster scales as one fixed composition, the buttons shrink
with it. Measured:

| Viewport | Button | Centre-to-centre |
| --- | --- | --- |
| 360x780 | 141 x 26 px | 29.9 px |
| 390x844 | 152 x 29 px | 32.4 px |
| 430x932 | 168 x 31 px | 35.7 px |

That is below the 44 px (iOS) and 48 px (Android) minimums, and it cannot be
fixed by padding the hit area: at ~32 px between button centres, 44 px targets
would overlap and taps near a boundary would open the wrong link. Raising it
would mean letting the poster crop at the sides, or laying the buttons out in
flow beneath the artwork instead of on top of it.

### Links

The four buttons are `<a class="link-btn">` in `index.html` — edit the `href`
values there.

| Button | Target |
| --- | --- |
| Instagram | profile |
| YouTube | channel |
| Facebook | profile |
| WhatsApp | `https://wa.me/<countrycode><number>` — no `+` or spaces |

### Assets

| File | What it is |
| --- | --- |
| `assets/background.webp` / `.jpg` | the torn-paper artwork, full-bleed |
| `assets/avatar.jpg` | 320x320, cropped square on the face |

Fonts (Cormorant Garamond, Plus Jakarta Sans, La Belle Aurore) load from Google
Fonts. Self-hosting them would remove the only external requests the page makes.

## The QR code

**https://sapta1997.github.io/VisitingQR/qr.html**

Enter the address, download the code as **SVG** for print or PNG for screen.
It builds in the things that make printed codes fail:

- the **quiet zone** (4 modules of margin) — it looks like empty padding and
  gets cropped, but scanners need it
- a **minimum print width**, calculated from a 0.5 mm floor per module
- a **contrast check**, warning below 4:1 and on inverted (light-on-dark) codes
- **whole-pixel modules** in the PNG, so edges stay crisp

Print the SVG. Then scan the actual printed sample with two different phones
before ordering a batch — that URL is what gets committed to paper.

## Notes

`design-new/` holds the canvas and artwork for an **earlier** version of the
card. Nothing in the live page uses it; it is kept only as history.
