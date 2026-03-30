# Pretext · Text Physics

A fun interactive demo showcasing [@chenglou/pretext](https://github.com/chenglou/pretext) — a library for fast, accurate multiline text measurement and layout without touching the DOM.

**Live demo → [pretext-demo.netlify.app]([https://lovely-granita-7f5362.netlify.app](https://pretext-demo.netlify.app/))**

## What it demonstrates

Pretext's `layoutNextLine` API lets you flow text around arbitrary shapes by computing a different `maxWidth` (and x offset) per line. Every frame, obstacles report their current position and size — the text reflows around them purely through arithmetic, no DOM layout or reflow triggered.

## Scenes

| Scene | Description |
|---|---|
| ☁ Raindrops | Teardrops fall and explode into ripple rings. Text parts around each ripple as it expands. Click anywhere to spawn one. |
| ⊕ Gravity | A glowing orb follows your mouse with spring physics. Text flows around the field as you drag it through. |
| ◎ Orbit | Three planets orbit a sun at different speeds and radii. Text fills the gaps between them as they move. |
| ⌁ UFO | A saucer traces a sine wave path across the screen. Text wraps around it in real time. |

## How it works

```js
import { prepareWithSegments, layoutNextLine } from '@chenglou/pretext'

// One-time: segment + measure the text (fast, ~19ms per 500 texts)
const prepared = prepareWithSegments(text, '16px "Space Mono"')

// Every frame: reflow around current obstacle positions
let cursor = { segmentIndex: 0, graphemeIndex: 0 }
let y = topPadding

while (y < height) {
  const { x, maxWidth } = availableRegion(y, obstacles) // your geometry
  const line = layoutNextLine(prepared, cursor, maxWidth)
  if (!line) break
  ctx.fillText(line.text, x, y + baseline)
  cursor = line.end
  y += lineHeight
}
```

`prepare` runs once. `layoutNextLine` is pure arithmetic — no DOM, no reflow, no canvas measurement per call.

## Stack

- Vanilla JS (no framework)
- Canvas 2D for rendering
- [`@chenglou/pretext`](https://github.com/chenglou/pretext) for text layout
- [Space Mono](https://fonts.google.com/specimen/Space+Mono) via Google Fonts
