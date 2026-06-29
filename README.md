# One Piece — Gear 5 Hover Reveal

An interactive hero banner where moving your cursor over **Monkey D. Luffy** carves a glowing comet trail that reveals **Gear 5 (Nika) Luffy** underneath. Built with a single HTML5 `<canvas>`, no frameworks, no build step.

The effect uses a smoothed (lerped) cursor that trails a fading line of circles. Those circles act as a mask: the top image is composited *into* the trail shape with `source-in`, so the second image appears in a tapering comet that streaks behind the pointer — finished with a white-gold "awakening" glow on the cursor head.

---

## Demo

Open `one-piece-gear5-reveal.html` in any modern browser and move your mouse across the screen. On touch devices, drag a finger across the image.

---

## Features

- **Comet-trail reveal** — 60-point smoothed trail masks the hidden image into a tapering streak.
- **Awakening glow** — white-gold radial glow on the cursor head for the Gear 5 reveal moment.
- **Black overlay** — both layers are darkened with a configurable transparent overlay for a moody look.
- **One Piece theming** — layered "ONE PIECE" wordmark, Bangers/Bebas display fonts, straw-hat gold vs. Nika purple color split.
- **Responsive `cover` fitting** — images fill any screen ratio without stretching, scaled for device pixel ratio.
- **Self-contained** — images are embedded as base64, so the single HTML file runs offline with no external assets (except Google Fonts).

---

## Quick start

1. Download `one-piece-gear5-reveal.html`.
2. Double-click it, or open it in a browser.

That's it — there is no install step and no server required. An internet connection is only needed the first time, to load the Google Fonts.

---

## Using your own images

The shipped file embeds the two images as base64 data URIs. To swap in your own art, replace the two `Image` sources in the `<script>` block.

For a **production / project** setup, point them at real files instead of base64:

```js
const bottom = new Image(); // revealed on hover (Gear 5)
const top    = new Image(); // shown by default (base Luffy)

bottom.src = './images/luffy-gear5.jpg';
top.src    = './images/luffy-base.jpg';
```

The drawing loop already waits for **both** images to load before it starts:

```js
let loaded = 0;
const onLoad = () => { if (++loaded === 2) draw(); };
bottom.onload = onLoad;
top.onload = onLoad;
```

> Note: `top` is the image you see at rest; `bottom` is the one revealed inside the trail.

---

## Tuning

All the knobs live at the top of the `<script>` block or inside the `draw()` loop.

| What | Where | Default | Effect |
|------|-------|---------|--------|
| Trail length | `const TRAIL_LENGTH` | `60` | Longer = longer comet tail |
| Reveal size | `let HEAD_RADIUS` | `~190` (auto-scaled) | Radius of the revealed circle |
| Cursor smoothing | `smooth.x += (mouse.x - smooth.x) * 0.13` | `0.13` | Lower = more lag/drift in the trail |
| Darkness | `const OVERLAY` | `'rgba(0,0,0,0.42)'` | Higher alpha = darker scene |
| Glow color | radial gradient in `draw()` | white-gold | The "awakening" head glow |

**Examples**

- Darker mood: `const OVERLAY = 'rgba(0,0,0,0.6)';`
- Subtle overlay: `const OVERLAY = 'rgba(0,0,0,0.25)';`
- Snappier cursor: change `0.13` to `0.22`.

---

## How it works

1. The **bottom** image (Gear 5) is drawn to the visible canvas, then darkened with the overlay.
2. Every frame, the smoothed cursor position is pushed to the front of a `trail` array (capped at `TRAIL_LENGTH`).
3. On an **offscreen canvas**, each trail point is drawn as a black circle that shrinks and fades toward the tail.
4. The **top** image (base Luffy) is composited onto that offscreen canvas with `globalCompositeOperation = 'source-in'`, so it only appears where the trail circles are.
5. The offscreen result is drawn over the visible canvas — base Luffy now shows only inside the comet, with Gear 5 everywhere else.
6. A `lighter`-blended radial gradient is drawn at the head for the glow.

```
bottom image ──► visible canvas (+ overlay)
trail circles ──► offscreen canvas
top image ──► offscreen canvas (source-in mask)
offscreen ──► visible canvas
glow ──► visible canvas (lighter blend)
```

---

## Browser support

Works in all current versions of Chrome, Edge, Firefox, and Safari. Requires Canvas 2D and `requestAnimationFrame` (universally supported). Touch is supported via `touchmove`.


```

---

## Credits & licensing

- Code: free to use and modify for your own projects.
- **Artwork & "One Piece" / characters**: One Piece is created by Eiichiro Oda; all character art and trademarks belong to their respective rights holders. The images here are fan/demo assets — replace them with art you have the right to use before publishing.
- The official One Piece logo typeface is proprietary and is **not** bundled; the wordmark in this demo is a CSS approximation using the free [Luckiest Guy](https://fonts.google.com/specimen/Luckiest+Guy) font. For an exact logo, drop in a transparent PNG of the official wordmark and swap the `.logo .op` text for an `<img>`.

---

*Made as a canvas hover-reveal experiment. Set sail. 🏴‍☠️*
