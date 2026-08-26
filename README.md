# Diagram Maker

A single-page web app for drawing science and maths diagrams — circuits, lab apparatus,
optics, geometry — and exporting them as clean SVG for print and study material.

## Contents

| File | Purpose |
| --- | --- |
| `public/` | Everything that gets published, and nothing else |
| `public/index.html` | The whole app: markup, styles and component logic |
| `public/support.js` | Runtime the template is built against |
| `public/icon.svg` | App icon; the source the other two are rendered from |
| `public/favicon.ico` | 32px fallback, also serves the bare `/favicon.ico` request |
| `public/apple-touch-icon.png` | 180px icon for iOS home screens |
| `wrangler.toml` | Cloudflare deploy config |
| `.thumbnail` | App thumbnail |
| `uploads/` | Reference images |

The icon links live in the `<helmet>` block, not the document `<head>` — the
runtime rebuilds `<head>` from `<helmet>`, so anything placed in the outer head
is discarded.

## Running locally

Serve the folder over HTTP (opening the file directly with `file://` will not load
`support.js` reliably):

```bash
python -m http.server 8765 --directory public
```

Then visit <http://localhost:8765/>.

## Features

- Four subjects — Physics, Chemistry, Mathematics, Biology — across 14 units,
  including a Trigonometric Functions unit of plotted graphs and an Inheritance
  unit for Mendelian cross charts whose Punnett square takes its rows, columns
  and cell contents from the Properties panel
- Click to place components, drag to move, snap to the grid and to connection
  points; the grid intersection a click will land on is marked, and Alt places
  freely
- The dashed export outline carries live millimetre dimensions, so the printed
  size of a figure is visible while drawing it
- Library tab beside the palette — save the canvas under a name and load it
  back later; each unit keeps its own shelf, stored on the device, and the
  whole library exports to a JSON file to carry to another machine
- Wire tool: click each corner, double-click to finish; straight or right-angle routing
- Angle marks — pick two drawn lines and the mark lands at their intersection,
  as an arc or a square right-angle corner, optionally arrowed at one end or both
- Congruence marks for Plane Geometry — one, two and three ticks, a cross, and
  single or double chevrons for parallel sides
- Plane Geometry drawing tools — segment, ray, polyline, polygon, arc and circle, all drawn
  by clicking on the canvas, with solid/dashed/dotted line styles and an
  adjustable curvature on arcs
- The Coordinate Geometry grid is built to order — set how many vertical and
  horizontal lines it has and how far apart they sit, anything from a 2×2 box
  to a full sheet of graph paper
- Thumbnail strip of everything on the canvas, so any element can be selected
  from the Properties panel without hunting for it
- Per-element label, size, rotation, flip and line weight
- Character palettes under the Label field — subscript, superscript, Greek in
  both cases, and common science and maths symbols, inserted at the caret
- Label can sit above or below a component
- Text typed straight onto the canvas — the editor opens where you place it, and
  double-clicking any label reopens it
- Free-standing text placed anywhere on the canvas, rotatable to any angle,
  with vector bar, arrow, hat or dots drawn over it
- Equation tool (`Q`) — mathematical notation typeset on the canvas from a
  LaTeX subset: fractions, roots, scripts, sums and integrals with limits,
  Greek, accents and brackets that grow with what they hold. A chip row inserts
  the common fragments, and double-clicking an equation edits it in place
- Dimension tool — click two points to measure between them, with arrow, dot
  or bar ends and the label above or below the line
- Selecting anything floats a small bar over its box — flip, rotate 90°,
  duplicate and delete, without crossing to the Properties panel
- Duplicate any placed element — the **Copy** button in Properties, or `Ctrl+D`
- Settings popover — grid size (snapping follows at half steps), default line
  width, text size, label size and export margin, remembered between sessions
  along with the theme, grid style, typeface and snap toggles
- Scissor (`X`) — cut a drawn shape where you click and the halves become
  separate elements, so one can be dashed while the other stays solid. A line,
  arc, polyline or polygon parts at a single click; a circle takes two, one at
  each end of the arc you want out of it
- Select, Text and Dimension tools in the Properties panel, on V, T and D
- Icons shelf beside those tools — person, car, earth, satellite, the
  right-hand rule fist, thumbs up, eye, tree, ship, sun, moon, aeroplane and
  house, placed and sized like any other component
- **Upload SVG…** in that shelf adds your own artwork. The file is flattened
  into the same geometry everything else uses — groups collapsed, transforms
  baked in, shapes rewritten as paths — so an uploaded icon snaps, scales,
  rotates, takes the line-weight slider and exports exactly like a built-in.
  Uploads are kept on the device and travel with **Export Library**
- Undo/redo, zoom, fit-to-view, rulers, light/dark theme, sans/serif diagram text
- Cross-subject component search
- Export the artwork as SVG, or copy it to the clipboard to paste straight into
  a vector editor

## Equations

Written as LaTeX and laid out into the same primitives as everything else —
text runs for glyphs, drawn paths for fraction rules, radicals and brackets —
so an exported equation needs no maths font at the other end.

| | |
| --- | --- |
| Scripts | `x^2`, `a_i`, `x_1^2`, `\sum_{i=1}^{n}` |
| Fractions | `\frac{a}{b}`, nested |
| Roots | `\sqrt{x}`, `\sqrt[3]{x}` |
| Large operators | `\sum \prod \int \oint \lim` — limits set above and below, or on the shoulder for integrals |
| Accents | `\vec{v} \bar{x} \hat{n} \dot{x} \ddot{x} \tilde{a}` |
| Brackets | `\left( … \right)`, also `[ ] \| \{ \}`, grown to the content |
| Upright | `\text{…}`, `\mathrm{…}`, and function names like `\sin \log \ln \max` |
| Greek | `\alpha` … `\omega`, `\Gamma` … `\Omega` |
| Symbols | `\times \div \pm \cdot \le \ge \ne \approx \equiv \propto \infty \partial \nabla \to \rightleftharpoons \therefore \degree` and more |
| Spacing | `\,` `\:` `\;` `\quad` `\qquad` |

Single Latin letters are set in italic as variables; digits, operators and
anything inside `\text{}` stay upright. An unrecognised command prints its own
name rather than vanishing, so a typo is visible on the canvas.

## Notes

- Equation glyph runs are positioned by their measured ink extents, not a
  nominal em box — stacking a fraction off a flat constant leaves digits and
  letters colliding, since the two sit at quite different heights.
- Uploaded SVGs are redrawn in the diagram ink rather than their own colours,
  which is what keeps them themed, weight-adjustable and consistent in a
  monochrome print export. Fills are preserved as fills; a white fill under a
  dark outline is treated as a knockout and dropped, unless the whole drawing
  is light, in which case it is taken as ink.
- Nothing from an uploaded file reaches the live document. It is parsed with
  `DOMParser`, which runs no scripts, and only geometry is carried across —
  the markup that renders is rebuilt from that geometry.
- The scissor works on drawn geometry — segments, rays, wires, polylines,
  polygons, arcs and circles. A palette component is one baked mark with no
  notion of where along itself it might be divided, so it cannot be cut.
- Exported text is centred with an explicit `dy` rather than
  `dominant-baseline`, which Illustrator ignores.
- The exported SVG pulls DM Sans via a Google Fonts `@import`, which Illustrator and
  InDesign ignore — text falls back to a generic sans. Convert text to outlines if you
  need exact type in print.
- Diagrams are not persisted; a browser reload clears the canvas.
