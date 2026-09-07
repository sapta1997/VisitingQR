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

`index.html` is a fixed, non-scrolling single screen: a torn-paper photograph
fills the viewport, and all the type sits on top as real HTML text.

- **The typography is text, not pixels.** Nothing is baked into the artwork, so
  it stays sharp at any density, can be selected and translated, and is read
  properly by screen readers.
- **`background.webp` is 1425x2532** — the size a 390px-wide phone needs at 3x
  pixel density under `object-fit: cover`. WebP is served first, with a JPEG
  fallback via `<picture>`. The WebP is 121 KB, smaller than the 576x1024 JPEG
  it replaced.
- **Tap targets are 44-50px**, meeting the iOS (44) and Android (48) minimums.
- **Pinch-zoom is left enabled** — deliberately, since disabling it fails
  WCAG 1.4.4 and the artwork rewards a closer look.
- Layout is driven by `dvh` units and `clamp()`, and respects the safe-area
  insets so notches and home indicators do not clip anything.
- Motion is disabled under `prefers-reduced-motion`.

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
