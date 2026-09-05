# VisitingQR

A single-page **digital visiting card** — one link that holds every contact
method, meant to be reached by scanning a QR code.

No build step, no dependencies, no tracking. One HTML file.

## Live URL

Once GitHub Pages is enabled (see below), the card is served at:

```
https://sapta1997.github.io/VisitingQR/
```

That clean root URL is what the QR code should encode — shorter URLs make a
simpler, easier-to-scan QR. This is why the page is named `index.html`.

## Customising the card

Everything editable in [`index.html`](index.html) is marked with an `EDIT:`
comment. In order:

| What | Where |
| --- | --- |
| Page title & share preview | `<title>` and the `og:` meta tags |
| Profile picture | `.avatar` — initials, or swap the `<div>` for `<img src="photo.jpg">` |
| Name | `.name` |
| Tagline | `.tagline` |
| The links | `href` on each `<a class="link">` |
| Footer | `<footer>` |

### Link formats

| Type | Format |
| --- | --- |
| WhatsApp | `https://wa.me/918974151996` — country code + number, no `+` or spaces |
| Email | `mailto:someone@example.com` |
| Phone | `tel:+918974151996` |
| Everything else | the normal profile URL |

### Adding a new link

Copy any existing `<a class="link">` block and change three things: the
`--brand` colour, the `<path d="...">` inside `.icon`, and the `.label` text.
Icon paths can be copied from [simple-icons](https://simpleicons.org).

## Deploying

GitHub Pages must be switched on once, by the repository owner:

**Settings → Pages → Source: `Deploy from a branch` → Branch: `main` / `/ (root)` → Save**

After that, every push to `main` republishes the site automatically, usually
within a minute.

## Design notes

The page is a single poster image with four link buttons positioned on top of
it. The artwork is a torn-paper photograph reveal, designed in
`design-new/` and exported to `assets/`.

- **Buttons are placed in percentages** of the poster (`left: 30.566%`,
  `width: 39.0625%`, `--top` per button), so they stay locked to the artwork at
  every screen size. Their pill shape comes from `border-radius: 999px` clipping
  a rectangular image - the artwork corners are opaque and get cut away.
- **The poster always fits the screen whole**: `min(100vw, 100svh * 0.6667)`
  against a `1024/1536` aspect ratio, so nothing is ever cropped.
- **106 KB total**, down from 3.3 MB of source PNGs - a 97% cut, with the
  poster measured at 41 dB PSNR against the original (visually identical).
  WebP is served first with a JPEG fallback via `<picture>`.
- **Zero external requests.** No fonts, scripts, trackers or CDNs; the artwork
  carries the typography. The whole card is `index.html` plus `assets/`.
- **Accessible** - the poster's baked-in text is repeated in a screen-reader
  heading and the image `alt`, every button has an `aria-label`, focus rings are
  visible, and all motion is disabled under `prefers-reduced-motion`.

## Editing the artwork

`design-new/` holds the design canvas (`.dc.html`) and the full-resolution PNG
exports. After changing the artwork there, re-export and re-optimise into
`assets/`. Button geometry must stay at an 800x150 ratio (16:3) to match the
`400x75` slot it is drawn into.
