# Diagram Maker

A single-page web app for drawing science and maths diagrams — circuits, lab apparatus,
optics, geometry — and exporting them as clean SVG for print and study material.

## Contents

| File | Purpose |
| --- | --- |
| `Diagram Maker.dc.html` | The whole app: markup, styles and component logic |
| `support.js` | Runtime the `.dc.html` template is built against |
| `.thumbnail` | App thumbnail |
| `uploads/` | Reference images |

## Running locally

Serve the folder over HTTP (opening the file directly with `file://` will not load
`support.js` reliably) and open `Diagram Maker.dc.html`:

```bash
python -m http.server 8765
```

Then visit <http://localhost:8765/Diagram%20Maker.dc.html>.

## Features

- Three subjects — Physics, Chemistry, Mathematics — across 12 units
- Click to place components, drag to move, snap to grid and to connection points
- Wire tool: click each corner, double-click to finish; straight or right-angle routing
- Per-element label, size, rotation, flip and line weight
- Undo/redo, zoom, fit-to-view, rulers, light/dark theme, sans/serif diagram text
- Cross-subject component search
- Export the artwork as SVG

## Notes

- The exported SVG pulls DM Sans via a Google Fonts `@import`, which Illustrator and
  InDesign ignore — text falls back to a generic sans. Convert text to outlines if you
  need exact type in print.
- Diagrams are not persisted; a browser reload clears the canvas.
