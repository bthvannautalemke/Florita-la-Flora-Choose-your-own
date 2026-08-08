# Bag configurator — prototype

Working build of the "create your own" bag configurator described in the
project brief. Single self-contained file, no build step, no dependencies.

Open `index.html` directly in a browser, or serve the folder with any static
file server.

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
