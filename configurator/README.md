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
