# Bag configurator — prototype

Working build of the "create your own" bag configurator described in the
project brief. Single self-contained file, no build step, no dependencies.

Open `index.html` directly in a browser, or serve the folder with any static
file server.

## v2 — Tesla-configurator layout

Rebuilt structurally after feedback that v1 read as an editorial mockup, not
a professional configurator, and didn't follow the Tesla Model 3 design page
reference or the Stiletto theme. Network policy in this environment blocks
fetching tesla.com, floritalaflora.nl, and Shopify's theme store directly, so
this was rebuilt from Tesla's well-documented configurator structure rather
than a live pixel reference:

- Full-bleed split screen (bag stage ~62%, option rail ~38% on desktop,
  stacked on mobile) — no card, no marketing headline; the configurator is
  the whole page, like Tesla's.
- Near-neutral ground (off-white / near-black text) with a single accent —
  the product's own Cobalt Blue swatch — instead of the earlier warm
  editorial palette.
- Circular thin-ring swatches (not chips), selection shown as text next to
  the swatch row (name + price delta), matching Tesla's paint-picker pattern.
- Price prominent at the top of the option rail, under the product name —
  Tesla's price-under-trim-name placement.
- Drag-to-explore on the stage image itself (pointer events, not just arrow
  clicks), plus arrows/dots/keyboard for discoverability and accessibility.
- Soft contact shadow under the product on its "studio floor."

**Still pending real brand fidelity**: exact hex/fonts from your Stiletto
theme and floritalaflora.nl. Send a screenshot (or the theme's style name —
Stiletto/Luster/Linen/Glimmer/Tapestry) and the palette/typography tokens at
the top of `<style>` are the only thing that needs updating to match exactly.

## v3 — real brand tokens + redrawn illustration from your reference renders

Two inputs closed most of the remaining gaps:

1. **A screenshot of the actual Shopify theme editor** gave real tokens:
   white ground, near-black text/chrome, a soft blush-pink accent (from the
   promo banner) used only for swatch-selection rings — no more placeholder
   navy blue. CTA buttons switched from a pill shape to Stiletto's sharp,
   minimal-radius, high-contrast black-on-white style. The wordmark now
   reads "FLORITA" + lowercase "la" + "FLORA" with wide tracking, matching
   the real nav casing (font itself is still a safe system stack pending
   your actual typeface file).
2. **Four AI-rendered concept grids** (cobalt, leopard, zebra, plain
   line-art), each showing the same 6 compositions, gave real ground truth
   for the product: a rounder gathered-top body, round bow loops with a
   gold floral charm tag, a thin rolled handle with a small buckle, and a
   wide crossbody strap with a square slide buckle draped diagonally across
   the front. The SVG was redrawn to match this, and the swatch colors
   (cobalt blue, leopard, zebra) were resampled from the real renders.

**Important limitation, unchanged from v0.1 of the brief**: those 4 renders
are finished raster images (baked-in shading per colorway), not neutral
masks — so they can't be used directly as live-swappable layers in the
browser. What's here is a redrawn SVG that matches their silhouette and
hardware, not the renders themselves. True pixel-fidelity to those exact
renders would need the illustrator to deliver actual separated/maskable
layers (see brief §6).

**Angle set changed** to match what the reference actually shows, since a
true 6-angle rotation (front/side/back) was never supplied: Front → Strap
draped → Strap flat (detail) → Strap hardware (detail) → Back → Bow & tag
(detail). **Back is still an inferred guess** — no back-view reference has
been supplied for any version; flagged in the frame's own `aria-label` and
here.

**QA pass**: went through all 6 angles at high resolution, in the default
colorway and in an all-one-color stress test (a good way to catch shape
bugs that contrasting colors hide) and found four real, fixed defects: a
buckle rotated with the wrong sign (rendered as a floating disconnected
diamond), a stroke-only buckle outline that didn't read as hardware, a
visible seam gap between the strap-hardware buckle and the sides shape
below it, and a crossfade duration slower than a test's wait time (masked
as a "ghost frame" artifact until the wait was corrected — also shortened
the real transition from 480ms to 300ms since it felt sluggish regardless).

## What's real vs. placeholder

- **Interaction, layout, accessibility, pricing logic, and the request
  form are real** — this is the actual mechanism the live site will use.
- **The bag illustrations are placeholder line-art**, hand-built in SVG to
  demonstrate the layered-2D recoloring technique (each of the 4 zones —
  front, back, sides, strap — is its own paintable shape, shared across all
  6 angles). Swap these for the commissioned illustration set from the
  brief's Open Questions without touching any JS: just replace the `<path>`
  `d` data under each `data-zone="…"` element with the real artwork's paths.
- **Pricing (€245 base, +€25 per pattern zone) is a placeholder matrix.**
  Update `BASE_PRICE` and each swatch's `surcharge` in the `<script>` block
  once the real price matrix is confirmed.
- **The "Request this design" form does not send real email.** It validates
  and shows a confirmation with the full recap, and logs the payload to the
  console — wire `requestForm`'s submit handler to a real endpoint (e.g.
  Formspree, Resend, or a small serverless function) before launch.

## Structure

- 4 configurable zones: `front`, `back`, `sides`, `strap` — each swatch
  choice is painted onto every matching `[data-zone="…"]` element across all
  6 `<svg>` frames at once.
- 3 swatches: Cobalt Blue (solid fill), Leopard and Zebra (tileable SVG
  `<pattern>` fills defined once in a shared `<defs>` sprite at the top of
  the document).
- 6 fixed angles: Front · Front ¾ · Side · Back ¾ · Back · Interior peek.
- Fixed (non-configurable) elements — hardware, the bow/lace, the brand tag
  — are drawn with a constant fill and never touched by the swatch logic.

See the published brief for the full decision log and open items.
