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

- Four subjects — Physics, Chemistry, Mathematics, Biology — across 15 units,
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
  adjustable curvature on arcs. The circle's side handles pull it into an oval,
  which then turns like anything else
- Logic unit — switching circuits as Mathematical Logic draws them: switches
  open or closed, a lamp plain or crossed, a battery of one to four cells,
  junction dots and arrowed leads. Switches in series read as a conjunction,
  in parallel as a disjunction, and each carries its own bar on the right of
  the selection box for the state it can be in
- The Coordinate Geometry grid is built to order — set how many vertical and
  horizontal lines it has and how far apart they sit, anything from a 2×2 box
  to a full sheet of graph paper
- Thumbnail strip of everything on the canvas, so any element can be selected
  from the Properties panel without hunting for it
- **Select multiple** in that strip ticks several elements and groups them.
  A group moves as one and carries a violet dashed box of its own, drawn
  unlike the gold selection frame so the two are never confused. Double-click
  to step inside and work on one member; the box goes faint while you are in
  there. A group can be duplicated, ungrouped or deleted from the bar above it
- Per-element label, size, rotation, flip and line weight
- Everything on the canvas carries the same selection box — eight handles to
  resize and a knob to turn it. A drawn shape has no scale or rotation of its
  own, so its handles work on the points themselves; its vertex rings stay put
  for reshaping one corner at a time, drawn open so the grid point underneath
  shows through. An arc keeps its curvature under a corner
  drag, and Shift holds the proportions on anything else
- Character palettes under the Label field — subscript, superscript, Greek in
  both cases, and common science and maths symbols, inserted at the caret
- Label can sit above or below a component
- A Label bar under the selection box types the label straight onto the canvas,
  with two arrows to put it above or below and an × to clear it, which appears
  only once there is a label to clear. `_` drops what follows and `^`
  raises it — `H_2O`, `10^24`, `d_{AB}` — taking a braced group, a run of
  digits, or one character
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
- A bar down the right of the selection box carries whatever belongs to that
  kind of element and nothing else — the arc or right angle and the arrowheads
  on an angle mark, the ends of a dimension, the arrow and corner routing on a
  wire, filled or hollow on a point, the mark over a piece of text, open or
  closed on a switch, the number of cells in a battery
- A Style bar down the left of the selection box sets the line style — solid,
  dashed or dotted — on anything with a stroke to break up, components
  included, so a dashed lead or a dotted construction line is one click away
- Duplicate any placed element — the **Copy** button in Properties, or `Ctrl+D`
- Settings popover — grid size (snapping follows at half steps), default line
  width, text size, label size and export margin, remembered between sessions
  along with the theme, grid style, typeface and snap toggles
- Pencil (`P`) — drag to draw any shape by hand. The trail is thinned to the
  points that carry the shape and curved back through them, so what lands is a
  smooth line of a few dozen segments rather than hundreds of tiny facets;
  finishing near where you began closes it. From then on it selects, scales,
  turns, takes a line style and cuts like anything else
- Scissor (`X`) — cut anything where you click and the pieces become separate
  elements, so one can be dashed while the other stays solid. A line, arc,
  polyline or polygon parts at a single click; a circle takes two, one at each
  end of the arc you want. On a component the stroke you click comes away from
  the rest, and any piece can be cut again
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
- Favourites — a heart appears on a palette tile as the pointer crosses it,
  and what you mark collects into a bar along the top of the canvas, reachable
  from any subject or unit. Kept on the device
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
- Cutting a component reduces it to the outlines it draws, so the piece that
  comes away is bare geometry that takes the line style and weight directly.
  It is no longer a resistor or a bulb, which is the price of being able to
  dash one stroke of it. Curves are parted by de Casteljau rather than sampled
  into lines, so a piece of a curve is still a curve and stays exactly on the
  original. Glyph text cannot be divided, so it rides with the piece that keeps
  the rest of the drawing, and a label stays with the largest piece.
- A dot is a short dash left round by the cap, so both the cap and a dash
  length with something in it are load-bearing. The node model carries no
  linecap, so every `<ellipse>` it renders spells the round cap out the way a
  `<path>` does — without it a dotted circle draws nothing at all.
- Exported text is centred with an explicit `dy` rather than
  `dominant-baseline`, which Illustrator ignores.
- The exported SVG pulls DM Sans via a Google Fonts `@import`, which Illustrator and
  InDesign ignore — text falls back to a generic sans. Convert text to outlines if you
  need exact type in print.
- Diagrams are not persisted; a browser reload clears the canvas.
