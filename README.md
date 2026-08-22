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

- Three subjects — Physics, Chemistry, Mathematics — across 13 units,
  including a Trigonometric Functions unit of plotted graphs
- Click to place components, drag to move, snap to the grid and to connection
  points; the grid intersection a click will land on is marked, and Alt places
  freely
- The dashed export outline carries live millimetre dimensions, so the printed
  size of a figure is visible while drawing it
- Library tab beside the palette — save the canvas under a name and load it
  back later; each unit keeps its own shelf, stored on the device, and the
  whole library exports to a JSON file to carry to another machine
- Wire tool: click each corner, double-click to finish; straight or right-angle routing
- Congruence marks for Plane Geometry — one, two and three ticks, a cross, and
  single or double chevrons for parallel sides
- Plane Geometry drawing tools — segment, ray, polygon, arc and circle, all drawn
  by clicking on the canvas, with solid/dashed/dotted line styles and an
  adjustable curvature on arcs
- Thumbnail strip of everything on the canvas, so any element can be selected
  from the Properties panel without hunting for it
- Per-element label, size, rotation, flip and line weight
- Character palettes under the Label field — subscript, superscript, Greek in
  both cases, and common science and maths symbols, inserted at the caret
- Label can sit above or below a component
- Free-standing text placed anywhere on the canvas, rotatable to any angle,
  with vector bar, arrow, hat or dots drawn over it
- Dimension tool — click two points to measure between them, with arrow, dot
  or bar ends and the label above or below the line
- Duplicate any placed element — the **Copy** button in Properties, or `Ctrl+D`
- Settings popover — grid size (snapping follows at half steps), default line
  width, text size, label size and export margin, remembered between sessions
  along with the theme, grid style, typeface and snap toggles
- Undo/redo, zoom, fit-to-view, rulers, light/dark theme, sans/serif diagram text
- Cross-subject component search
- Export the artwork as SVG

## Notes

- The exported SVG pulls DM Sans via a Google Fonts `@import`, which Illustrator and
  InDesign ignore — text falls back to a generic sans. Convert text to outlines if you
  need exact type in print.
- Diagrams are not persisted; a browser reload clears the canvas.
