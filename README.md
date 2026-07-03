# Contour Slicer

**Turn any 3D model into printable cutting templates for stacked MDF — no CNC, no 3D printer required.**

Contour Slicer takes an STL file and slices it into cross-sections, then lays those cross-sections out as print-to-paper templates. You print them, spray-mount onto MDF, cut with a jigsaw or scroll saw, and glue the stack together. It's the fastest way to go from a 3D file to a real physical object when you don't have a CNC router or 3D printer — or when you just need a quick mockup and don't want to wait hours for a print.

![Contour Slicer main interface](screenshots/01-main-ui.png)

---

## Why this exists

Not everyone has a CNC machine or a 3D printer. But almost everyone has:

- A printer
- A jigsaw or scroll saw
- Sheets of MDF (cheap, available at any hardware store)
- Spray adhesive or glue stick

Contour Slicer bridges the gap between digital 3D models and physical objects using only those basic tools. It's perfect for:

- **Rapid mockups & prototypes** — go from STL to a physical object in under an hour
- **Makers without a 3D printer** — build large objects that wouldn't fit on a printer bed anyway
- **Sculptors & prop makers** — create organic forms by stacking and sanding layered cross-sections
- **Educators** — demonstrate how 3D models can be decomposed into 2D manufacturing steps
- **Big objects** — a 200mm tall model needs 33 sheets of 6mm MDF; good luck printing that on an Ender 3

![Sliced view with stats](screenshots/02-sliced-stats.png)

*The stats panel shows everything you need: layer count, page count, MDF area, stack height, **MDF sheets needed** (with 20% waste factor), **dowel rod length** (when alignment holes are on), and a live count of layers that exceed the current paper size.*

---

## How it works

1. **Load an STL** — drag in a file, or try the built-in Ring or Box examples (no download needed)
2. **Auto-detect picks the method** — the app analyzes your model's geometry and chooses Stacked Layers or Flat Panels automatically (you can override)
3. **Auto-detect units** — the app reads the STL header and bounding-box diagonal to guess whether your model is in mm, cm, m, inches, feet, or microns (you can override)
4. **Set the scale** — use the model's detected units, or force it to a specific height
5. **Set MDF thickness** — defaults to 6mm (the most common sheet)
6. **Pick a slice axis** — X, Y, or Z (no need to rotate the model in Tinkercad just because it loaded on its side)
7. **Compute slices** — the app calculates every cross-section automatically
8. **Preview the result** — see a 3D mockup of the assembled stack
9. **Print or export** — print to paper, download SVGs, or download a multi-page vector PDF directly

![Assembly preview](screenshots/03-assembly-preview.png)

*The assembly preview shows exactly what your glued-up stack will look like before you cut a single piece — and it stacks along the chosen slice axis, so X-sliced models preview standing on their X-face, etc.*

---

## Features

### Auto-detect decomposition mode

The app analyzes your model's triangle normals and automatically picks the best method:
- **Stacked Layers** for curved/organic shapes (many distinct surface patches)
- **Flat Panels** for boxy/polyhedral shapes (few distinct flat faces)

You can always override it by picking a mode manually. A note explains the choice.

### Auto-detect STL units

STL is a unitless format — the numbers are just numbers. Contour Slicer reads up to 80 bytes of the file header (binary STL comment field, or the ASCII `solid <name>` line) looking for unit hints like `mm`, `inch`, `meter`, then falls back to a bounding-box-diagonal heuristic (a model with a 0.5 diagonal is almost certainly in meters; one with a 50 000 diagonal is almost certainly in microns).

The detected unit shows next to the dropdown with the computed model diagonal in mm. You can override the choice manually — pick from mm, cm, m, inches, feet, or microns, and the dimensions readout, grid, and stats all update live. No more importing a model exported in inches and getting a 2-millimetre-tall result.

### Two decomposition modes

- **Stacked Layers** — slices the model into cross-sections, one per MDF sheet. Best for rounded, organic, or sculptural shapes. Stack and glue them in order for a faithful reproduction.
- **Flat Panels** — extracts each flat face as a separate panel. Best for boxy shapes (crates, housings, geometric forms). Cut and glue edge-to-edge like assembling a box.

### Multi-axis slicing

Most STL files load with Z as the vertical axis, but not all of them do. Models exported from some CAD tools come in on their side, and until now you had to either rotate the geometry in-app (which mutates the model) or go back to Tinkercad. Now you can just pick the **Slice axis** (X, Y, or Z) from a dropdown — the slicing engine intersects triangles with the plane perpendicular to that axis and projects the result onto the correct 2D coordinate system. The assembly preview stacks the layers along the chosen axis too, so you see the assembled shape in its real orientation.

### Smart print layout

Small shapes are automatically **packed multiple-per-page** to save paper. A 6-layer ring model that used to need 6 pages now fits on 2. Shapes larger than one page are tiled across pages with alignment crosses for taping together.

### Oversize layer warning

If any layer in your print range is too big to fit on the selected paper (A4 or US Letter), a prominent orange warning appears above the Compute button telling you how many layers are affected, plus a new stat tile in the stats panel shows the count. Click **Auto-fit to paper** and the print scale is automatically shrunk so the largest layer just fits (with a 5% safety margin) — no more guessing at scale percentages.

### Print scale

Need a miniature? Set the print scale to **50%** and every cut outline shrinks proportionally — the calibration ruler on the cover page scales too, so you can verify it printed correctly. Want something bigger? Crank it to **200%**. The assembly preview, stats, and oversize warning all update live.

### Numbered cut-pieces markers

Every cut outline has a **big bold number** at its centroid — large enough to read through spray-mount paper, so you can trace it onto the MDF with a marker before peeling the paper. No more "which layer is this?" confusion at assembly time.

![Print template with numbered markers](screenshots/09-print-numbered.png)

*Each shape gets a big bold number inside the outline, plus a small label below it.*

### Mirror half-stack

For left-right symmetric models (vases, bottles, most sculptural forms), enable **Mirror half-stack** and you only cut half the layers — then flip the stack to form the other half. Cuts your work in half. The cover page explains the assembly: stack in order, flip, stack again in reverse.

### Fold lines (panel mode)

For boxy shapes, enable **Fold lines** and the app generates a single unfold page with all panels connected by dashed fold lines. Cut one piece, score the fold lines, fold it up, glue one seam. Big workflow win for boxes and crates.

![Fold lines unfold](screenshots/10-fold-lines.png)

*Six panels connected by dashed fold lines — cut once, fold, glue one seam.*

### Locking tabs & slots

Enable **Add locking tabs & slots** and each layer gets small rectangular tabs sticking up around its perimeter (in evenly-spaced positions) that fit into matching slots cut into the layer above. This locks the stack against lateral sliding while the glue sets — much more robust than just dowels, and adds significant glue surface area for stronger joints.

You control the tab width, tab height, and number of tabs per layer (2–12). Tabs are drawn as solid rectangles (cut as part of the outline); slots are drawn as dashed rectangles (cut as notches into the layer above). The first layer has no slots below it; the last has no tabs above it. Tab positions are computed by firing N rays from the model's centroid at evenly-spaced angles and finding where each ray hits the outer loop — so tabs land at the same angular positions on every layer and line up vertically when stacked.

### Honeycomb infill (hollow out large layers)

Big solid layers waste MDF and add weight. Enable **Hollow out large layers** and the app generates a grid of regular hexagons inside each large layer's outline (clipped to a border margin from the edge). Each hex is a separate small interior cut-out — drill a starter hole, then jigsaw the hex outline. You control the hex cell size, the border thickness kept around the edge, and the minimum layer area below which hollowing is skipped (so small layers stay solid for strength).

### Assembly preview

Before cutting anything, click **Preview assembled result** to see a 3D rendering of the finished stack. Each layer is extruded to the MDF thickness and stacked at its correct height — along the chosen slice axis. For flat-panel mode, all panels are laid out so you can see every piece.

### Material estimator

Pick your MDF sheet size (2440×1220mm standard, 1830×915, 1220×610, or custom) and the stats show **how many sheets you need** — calculated with a 20% waste factor. No more guessing at the hardware store.

### Dowel rod length calculator

When alignment holes are enabled, the stats show the exact **dowel rod length** you need — stack height plus 20mm handling margin on each end.

### Alignment holes

Optionally mark alignment holes on each layer — either a single center hole or four corner holes. Drill these before removing the paper template, then thread a dowel rod through the stack to keep everything registered while the glue sets.

### SVG export (single + per-layer ZIP)

Two SVG export options:
- **Download SVG (single)** — one SVG file with all layers as `<g>` groups
- **Download SVGs (ZIP, per layer)** — one SVG file per layer, bundled into a ZIP (built-in ZIP writer, no external library). Each file has the big number, alignment holes, locking tabs/slots, and hollow hexes. Laser cutter users can load each layer individually.

### PDF export (direct, no browser print dialog)

Click **Download PDF** and a multi-page vector PDF is generated directly in the browser — no `window.print()` involved, no dependence on browser print settings. The PDF writer is built from scratch (no external libraries — about 300 lines of code) and produces a valid PDF 1.4 file with A4 or Letter pages, embedded Helvetica font, proper xref table, and all the same content as the print view: cover page with calibration ruler, packed pages with layer outlines, alignment holes (rendered as 16-gons), locking tabs/slots, hollow hexes, big layer numbers, and per-page labels.

Open it in any PDF viewer (Acrobat, Preview, browser) and print from there if you prefer — the output is identical to the in-app print templates.

### Undo / redo

Every settings change is captured in a 50-entry undo stack with a 350 ms debounce (so dragging a slider doesn't push 100 snapshots). Use the **Undo** / **Redo** buttons in the header, or the keyboard shortcuts:

- **Ctrl+Z** (or Cmd+Z on Mac) — undo
- **Ctrl+Y** or **Ctrl+Shift+Z** — redo

Works even when you're focused in an input field. Geometry rotations are deliberately not tracked (those are model edits, not settings tweaks).

### Project save/load

Save your entire project — STL model + all settings (including units, slice axis, tab parameters, hollow parameters, etc.) — to a `.cspj` file. Load it later and you're back exactly where you left off. No more re-importing the STL and re-entering settings every time you close the tab. The format is JSON with the STL base64-encoded inside, so it's inspectable and diff-friendly.

### Thumbnail overlays

Each layer chip in the layer strip shows small markers indicating where features land on that layer:
- **Orange filled dots** = locking tab anchors (this layer has tabs sticking up)
- **Teal hollow circles** = slot anchors (this layer has slots cut into it from below)
- **Dim grey dots** = hollow hex centers (or a `⬡N` count badge if there are more than 30)

The bottom layer shows only tab dots (no slots beneath it); the top layer shows only slot dots (no tabs above it). Hovering a thumbnail still shows the slice-plane preview through the 3D model.

### Viewer tools

- **Grid & Ruler** — adaptive ground grid with major/minor lines and numeric labels; graduated scale bar with tick marks
- **Dimensions readout** — real-world W × D × H in mm, always visible, updates when you change units
- **Axis gizmo** — colored X/Y/Z tripod in the corner so you never lose orientation
- **View presets** — Front, Top, Isometric (keys 1/2/3)
- **Wireframe** — see through the model to internal structure (key W)
- **Measure** — click two points on the model to get the distance in mm (key M)
- **90° rotation** — rotate the model in-app if it's on its side (no need to go back to Tinkercad)
- **Layer hide/show** — click a thumbnail to toggle a layer's visibility

### Slice-plane preview

Hover any layer thumbnail and a translucent orange plane appears through the 3D model at that slice's height, with a "Layer N" badge — so you can see exactly where each cut falls before printing.

![Slice-plane preview](screenshots/04-slice-plane.png)

*Hover a thumbnail to see where that slice falls on the 3D model.*

### Proper kerf-aware polygon offset

Set a non-zero **Kerf offset** and every layer outline is shrunk inward by half that distance to compensate for blade width. The offset uses a proper per-edge inward-normal algorithm: for each edge of the loop, the inward normal is computed, the edge is shifted inward by the offset distance, and each new vertex is the intersection of its two adjacent offset edges. This correctly handles convex polygons exactly and falls back to a centroid-shrink only at sharp concave corners (to avoid producing invalid geometry). The old version used a crude centroid-scale approximation that broke on concave shapes — this one doesn't.

### Guided walkthrough

First-time users get a 6-step spotlight tour that highlights the key controls. It auto-starts on first visit and can be re-triggered anytime via the **Tour** button.

![Guided walkthrough](screenshots/05-walkthrough.png)

### Light & dark themes

Follows your OS preference by default, with a manual override that persists across sessions. No flash of the wrong theme on reload.

![Light mode](screenshots/06-light-mode.png)

---

## Getting started

### Option A: Download and run (recommended)

1. Download `index.html`
2. Open it in any modern browser (Chrome, Firefox, Safari, Edge)
3. That's it — no install, no server, no dependencies

The app needs an internet connection **only the first time** it runs, to load the 3D engine (three.js) from a CDN. After that, the browser caches it and you can use the app offline.

### Option B: Try the examples first

Don't have an STL handy? Click **Ring (has a hole)** or **Box (flat faces)** on the model section to load a generated example. The ring is a great test of the stacked-layers mode (it has interior holes); the box is a great test of flat-panel mode.

---

## Where to get STL files

- **Tinkercad** (free, browser-based) — design simple shapes or remix community models, export as STL
- **Thingiverse** / **Printables** / **MakerWorld** — millions of free models, download as STL
- **Scan the World** — museum sculptures and artworks, ready to slice
- **Blender** / **Fusion 360** / **Onshape** — design your own from scratch

---

## Cutting & assembly tips

1. **Print at 100% scale** — make sure "Fit to page" is OFF in your print dialog. The calibration page has a 100mm ruler mark — measure it to verify. (If you set a print scale like 50%, the scaling is already baked in — don't apply it again in the print dialog.)
2. **Spray-mount the template** onto the MDF. Spray adhesive or glue stick both work.
3. **Trace the layer numbers** onto the MDF with a marker before removing the paper — the big bold numbers are visible through the paper.
4. **Cut the outline** with a jigsaw or scroll saw. Cut any interior (hole) outlines too.
5. **Cut the locking tabs** (if enabled) — they're solid rectangles extending outward from the outline, part of the same cut.
6. **Cut the locking slots** (if enabled) — they're dashed rectangles inset into the layer; cut them as notches.
7. **Cut the hollow hexes** (if enabled) — drill a starter hole inside each hex, then jigsaw the hex outline.
8. **Drill alignment holes** (if enabled) before removing the paper.
9. **Stack in numeric order** (layer 1 = bottom). The tabs on layer N slot into the notches on layer N+1 — they should seat with a firm press. Thread a dowel rod through the alignment holes (if any) to keep things registered.
10. **Glue between layers**, clamp, and let it set.
11. **Sand the stepped edge** smooth, or leave it faceted for a layered look.

For large objects that span multiple pages per layer, trim along the overlap lines and tape adjacent sheets together first, matching the alignment crosses.

**Mirror mode:** If you enabled Mirror half-stack, cut the half-set of layers, stack and glue them in order, then flip the entire stack over and glue the same layers on top in reverse order to form the full symmetric shape.

**Fold lines (panel mode):** If you enabled Fold lines, cut the single unfold piece, score the dashed lines with a utility knife, fold, and glue the one seam to close the box.

---

## Technical details

- **Single HTML file** — everything (HTML, CSS, JS) is in one self-contained file (~4400 lines, ~200 KB). No build step, no node_modules, no framework.
- **Three.js** for 3D rendering and STL parsing, loaded from CDN. The only external dependency.
- **Runs entirely in the browser** — your STL files never leave your computer. No upload, no server, no tracking.
- **Works offline** after first load (CDN is cached).
- **Mobile-friendly** — responsive layout, touch-optimized controls, safe-area insets for notched phones.
- **Built-in ZIP writer** — the per-layer SVG ZIP export uses a from-scratch ZIP implementation (no JSZip dependency).
- **Built-in PDF writer** — the PDF export uses a from-scratch minimal PDF 1.4 implementation (no jsPDF or pdfmake dependency). Produces valid multi-page vector PDFs with embedded Helvetica font.
- **Built-in polygon offset** — the kerf compensation uses a proper per-edge inward-normal algorithm rather than relying on a polygon-boolean library.
- **Built-in unit detection** — reads the STL file header and applies a bounding-box-diagonal heuristic to auto-detect units, no user intervention needed.

---

## Keyboard shortcuts

| Key | Action |
|-----|--------|
| `G` | Toggle grid |
| `R` | Toggle ruler |
| `W` | Toggle wireframe |
| `M` | Toggle measure tool |
| `1` | Front view |
| `2` | Top view |
| `3` | Isometric view |
| `Ctrl+Z` | Undo settings change |
| `Ctrl+Y` or `Ctrl+Shift+Z` | Redo settings change |
| `Esc` | Close modal / clear measurement |

---

## Screenshots

### Main interface with grid, ruler, and dimensions
![Main interface](screenshots/01-main-ui.png)

### Stats panel — layers, pages, area, height, sheets, dowel length, oversized count
![Sliced stats](screenshots/02-sliced-stats.png)

### Assembly preview — see the glued-up stack before cutting
![Assembly preview](screenshots/03-assembly-preview.png)

### Slice-plane preview — hover a layer to see where it falls
![Slice-plane preview](screenshots/04-slice-plane.png)

### Guided walkthrough
![Walkthrough](screenshots/05-walkthrough.png)

### Light mode
![Light mode](screenshots/06-light-mode.png)

### Box in flat-panel mode
![Box panels](screenshots/07-box-panels.png)

### Panel layout preview
![Panel layout](screenshots/08-panel-layout.png)

### Print template with numbered cut markers
![Print numbered](screenshots/09-print-numbered.png)

### Fold lines unfold — one piece, score and fold
![Fold lines](screenshots/10-fold-lines.png)

---

## License

MIT — do whatever you want with it. Build things, sell things, learn things.

---

## Credits

Built with [Three.js](https://threejs.org/). Fonts: IBM Plex Sans & Mono.
