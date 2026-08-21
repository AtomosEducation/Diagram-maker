# Diagram Maker

A single-page web app for drawing science and maths diagrams — circuits, lab apparatus,
optics, geometry — and exporting them as clean SVG for print and study material.

## Contents

| File | Purpose |
| --- | --- |
| `index.html` | The whole app: markup, styles and component logic |
| `support.js` | Runtime the template is built against |
| `icon.svg` | App icon; the source the other two are rendered from |
| `favicon.ico` | 32px fallback, also serves the bare `/favicon.ico` request |
| `apple-touch-icon.png` | 180px icon for iOS home screens |
| `.thumbnail` | App thumbnail |
| `uploads/` | Reference images |

The icon links live in the `<helmet>` block, not the document `<head>` — the
runtime rebuilds `<head>` from `<helmet>`, so anything placed in the outer head
is discarded.

## Running locally

Serve the folder over HTTP (opening the file directly with `file://` will not load
`support.js` reliably):

```bash
python -m http.server 8765
```

Then visit <http://localhost:8765/>.

## Features

- Three subjects — Physics, Chemistry, Mathematics — across 12 units
- Click to place components, drag to move, snap to grid and to connection points
- Wire tool: click each corner, double-click to finish; straight or right-angle routing
- Per-element label, size, rotation, flip and line weight
- Character palettes under the Label field — subscript, superscript, Greek in
  both cases, and common science and maths symbols, inserted at the caret
- Label can sit above or below a component
- Free-standing text placed anywhere on the canvas, rotatable to any angle
- Duplicate any placed element — the **Copy** button in Properties, or `Ctrl+D`
- Undo/redo, zoom, fit-to-view, rulers, light/dark theme, sans/serif diagram text
- Cross-subject component search
- Export the artwork as SVG

## Notes

- The exported SVG pulls DM Sans via a Google Fonts `@import`, which Illustrator and
  InDesign ignore — text falls back to a generic sans. Convert text to outlines if you
  need exact type in print.
- Diagrams are not persisted; a browser reload clears the canvas.
