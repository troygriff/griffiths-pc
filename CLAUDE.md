# Griffiths, PC — Site & Brand Reference

Static site, no build step. `index.html` is the homepage; `branding/index.html` is
the live brand guidelines page and the canonical visual reference for this system.

**When changing anything visual on this site (colors, type, the dithering effect,
logo), update `branding/index.html` in the same pass so it never drifts out of
sync with reality — it's documentation that's meant to be checked against the
live page, not a static spec written once and forgotten.**

## Colors

All colors are phthalo green (a real pigment reference, `#123524`) at different
concentrations, plus a neutral ink/paper scale. Not arbitrary hexes — if you need
a new shade, derive it from the same hue/saturation as the existing ramp rather
than picking a new color.

| Token | Hex | Usage |
|---|---|---|
| `--accent-ink` | `#123524` | Phthalo green mass-tone. Headlines on dark, logo fill, darkest brand value. |
| `--accent` | `#1C5439` | Mid-concentration phthalo green. Eyebrows, links, hover states, CTA hover. |
| Dither Tint | `#319162` | Bright phthalo tint. **Only** for the dithering effect — needed to read as color, not gray, at small dot size. |
| `--ink` | `#14181B` | Primary text. Also the CTA button fill. |
| `--ink-soft` | `#4A5049` | Secondary/muted text — body copy, captions, credentials. |
| `--paper` | `#E7E9E3` | Primary background. The site commits to one light register — no dark mode. |
| `--paper-deep` | `#DBDFD5` | Secondary surface — alternating section backgrounds. |
| `--line` | `#B9BFB2` | Hairline borders and dividers. |

## Typography

Three roles, no more:

- **Display** — `ui-serif, "New York", "Iowan Old Style", "Palatino Linotype", Georgia, "Times New Roman", serif`
  Headlines, the wordmark, the logo glyph. Never used for body copy or labels.
- **Body** — `-apple-system, BlinkMacSystemFont, "SF Pro Text", system-ui, "Segoe UI", Roboto, sans-serif`
  Paragraph text only.
- **Mono** — `ui-monospace, "SF Mono", Menlo, "Cascadia Code", "Roboto Mono", monospace`
  Eyebrows/labels (uppercase, letter-spacing 0.1–0.18em), credentials, addresses,
  phone numbers, anything tabular.

Scale (all via `clamp()` so it degrades gracefully on mobile — see the type ramp
on `/branding` for live examples):
- H1 (hero): `clamp(2.75rem, 8vw, 6rem)`
- H2 (section): `clamp(2rem, 4.2vw, 3.1rem)`
- H3 (subsection): ~1.15–1.5rem depending on context
- Body: 1rem base / 1.65 line-height

## Logo

A lowercase serif "g" (display face) in a solid circular roundel. Two variants
only — defined as CSS classes on an inline SVG, not separate image assets:

- `.logo-mark` — standard: `--accent-ink` circle, `--paper` glyph. For light backgrounds.
- `.logo-mark.on-dark` — reversed: `--paper` circle, `--accent-ink` glyph. For dark backgrounds.

Rules: minimum render size 32px (glyph loses legibility below that). Clearspace
of at least half the mark's diameter on all sides. Never recolor outside the two
variants above, never add shadows/outlines/gradients/rotation, never stretch off
the 1:1 circle.

## Dithering

The site's one generative flourish — an 8×8 ordered (Bayer) dither over a
cosine-wave interference field, rendered in two WebGL passes, colored in the
Dither Tint (`#319162`). Full technique and a live interactive demo live on
`/branding#dither`.

- **Where**: homepage hero background only. Don't scatter it as decoration elsewhere.
- **Motion**: animated + mouse-reactive on the live hero. For print/static exports
  (business cards, banners, any merch), bake a **single static frame** instead —
  see `renderStaticDither()` in `branding/index.html` for the non-animated
  Canvas2D version used there.
- **Dots, not squares**: always sample brightness at the cell center and draw a
  circle at fixed radius per the Bayer threshold. Filling the whole cell as a
  square is the bug this specifically avoids — see prior iteration history.

## Imagery

Full-color photography only — no desaturation, duotone, or grayscale treatment.
Currently sourced from Unsplash as placeholders (not a locked style); documented
with usage captions on `/branding#imagery`. When replacing with the firm's own
photography, match:

- **Full color, always** — no filters.
- **Scale over staging** — wide industrial/infrastructure landscapes, not staged
  handshakes or gavels.
- **Landscape, full-bleed** — hero and section headers run edge-to-edge; vertical
  or tightly-cropped shots don't work here.

If you swap an image file, double-check its `alt` text actually describes the
new photo — this has drifted out of sync before.

## Merch / applications

Business card, LinkedIn banner (1584×396), and email signature templates are on
`/branding#applications`, built from the same tokens above. If asked to produce
new collateral (letterhead, social templates, etc.), match that section's
patterns rather than introducing new colors or type choices.

## File structure

```
index.html            homepage (single self-contained file, images inlined as base64)
branding/index.html   brand guidelines page (this doc's live counterpart)
CLAUDE.md             this file
```

No build tooling, no npm packages, no framework — plain HTML/CSS/JS in both
pages. Keep it that way; it's what makes GitHub Pages hosting trivial.
