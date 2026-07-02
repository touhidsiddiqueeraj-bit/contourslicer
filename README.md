# Contour Slicer

**Turn any 3D model into printable cutting templates for stacked MDF — no CNC, no 3D printer required.**

Contour Slicer takes an STL file and slices it into horizontal cross-sections, then lays those cross-sections out as print-to-paper templates. You print them, spray-mount onto MDF, cut with a jigsaw or scroll saw, and glue the stack together. It's the fastest way to go from a 3D file to a real physical object when you don't have a CNC router or 3D printer — or when you just need a quick mockup and don't want to wait hours for a print.

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

*The stats panel shows everything you need: layer count, page count, MDF area, stack height, **MDF sheets needed** (with 20% waste factor), and **dowel rod length** (when alignment holes are on).*

---

## How it works

1. **Load an STL** — drag in a file, or try the built-in Ring or Box examples (no download needed)
2. **Auto-detect picks the method** — the app analyzes your model's geometry and chooses Stacked Layers or Flat Panels automatically (you can override)
3. **Set the scale** — use the model's native units, or force it to a specific height
4. **Set MDF thickness** — defaults to 6mm (the most common sheet)
5. **Compute slices** — the app calculates every cross-section automatically
6. **Preview the result** — see a 3D mockup of the assembled stack
7. **Print or export** — print to paper (small shapes are packed multiple-per-page to save paper), or download SVG files for a laser cutter

![Assembly preview](screenshots/03-assembly-preview.png)

*The assembly preview shows exactly what your glued-up stack will look like before you cut a single piece.*

---

## Features

### Auto-detect decomposition mode

The app analyzes your model's triangle normals and automatically picks the best method:
- **Stacked Layers** for curved/organic shapes (many distinct surface patches)
- **Flat Panels** for boxy/polyhedral shapes (few distinct flat faces)

You can always override it by picking a mode manually. A note explains the choice.

### Two decomposition modes

- **Stacked Layers** — slices the model into horizontal cross-sections, one per MDF sheet. Best for rounded, organic, or sculptural shapes. Stack and glue them in order for a faithful reproduction.
- **Flat Panels** — extracts each flat face as a separate panel. Best for boxy shapes (crates, housings, geometric forms). Cut and glue edge-to-edge like assembling a box.

### Smart print layout

Small shapes are automatically **packed multiple-per-page** to save paper. A 6-layer ring model that used to need 6 pages now fits on 2. Shapes larger than one page are tiled across pages with alignment crosses for taping together.

### Print scale

Need a miniature? Set the print scale to **50%** and every cut outline shrinks proportionally — the calibration ruler on the cover page scales too, so you can verify it printed correctly. Want something bigger? Crank it to **200%**. The assembly preview and stats update live.

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

### Assembly preview

Before cutting anything, click **Preview assembled result** to see a 3D rendering of the finished stack. Each layer is extruded to the MDF thickness and stacked at its correct height. For flat-panel mode, all panels are laid out so you can see every piece.

### Material estimator

Pick your MDF sheet size (2440×1220mm standard, 1830×915, 1220×610, or custom) and the stats show **how many sheets you need** — calculated with a 20% waste factor. No more guessing at the hardware store.

### Dowel rod length calculator

When alignment holes are enabled, the stats show the exact **dowel rod length** you need — stack height plus 20mm handling margin on each end.

### Alignment holes

Optionally mark alignment holes on each layer — either a single center hole or four corner holes. Drill these before removing the paper template, then thread a dowel rod through the stack to keep everything registered while the glue sets.

### SVG export (single + per-layer ZIP)

Two export options:
- **Download SVG (single)** — one SVG file with all layers as `<g>` groups
- **Download SVGs (ZIP, per layer)** — one SVG file per layer, bundled into a ZIP (built-in ZIP writer, no external library). Each file has the big number and alignment holes. Laser cutter users can load each layer individually.

### Project save/load

Save your entire project — STL model + all settings — to a `.cspj` file. Load it later and you're back exactly where you left off. No more re-importing the STL and re-entering settings every time you close the tab.

### Viewer tools

- **Grid & Ruler** — adaptive ground grid with major/minor lines and numeric labels; graduated scale bar with tick marks
- **Dimensions readout** — real-world W × D × H in mm, always visible
- **Axis gizmo** — colored X/Y/Z tripod in the corner so you never lose orientation
- **View presets** — Front, Top, Isometric (keys 1/2/3)
- **Wireframe** — see through the model to internal structure (key W)
- **Measure** — click two points on the model to get the distance in mm (key M)
- **90° rotation** — rotate the model in-app if it's on its side (no need to go back to Tinkercad)
- **Layer hide/show** — click a thumbnail to toggle a layer's visibility

### Slice-plane preview

Hover any layer thumbnail and a translucent orange plane appears through the 3D model at that slice's Z height, with a "Layer N" badge — so you can see exactly where each cut falls before printing.

![Slice-plane preview](screenshots/04-slice-plane.png)

*Hover a thumbnail to see where that slice falls on the 3D model.*

### Guided walkthrough

First-time users get a 6-step spotlight tour that highlights the key controls. It auto-starts on first visit and can be re-triggered anytime via the **Tour** button.

![Guided walkthrough](screenshots/05-walkthrough.png)

### Light & dark themes

Follows your OS preference by default, with a manual override that persists across sessions. No flash of the wrong theme on reload.

![Light mode](screenshots/06-light-mode.png)

---

## Getting started

### Option A: Download and run (recommended)

1. Download `contour-slicer-6.html`
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
5. **Drill alignment holes** (if enabled) before removing the paper.
6. **Stack in numeric order** (layer 1 = bottom). Thread a dowel rod through the alignment holes to keep things registered.
7. **Glue between layers**, clamp, and let it set.
8. **Sand the stepped edge** smooth, or leave it faceted for a layered look.

For large objects that span multiple pages per layer, trim along the overlap lines and tape adjacent sheets together first, matching the alignment crosses.

**Mirror mode:** If you enabled Mirror half-stack, cut the half-set of layers, stack and glue them in order, then flip the entire stack over and glue the same layers on top in reverse order to form the full symmetric shape.

**Fold lines (panel mode):** If you enabled Fold lines, cut the single unfold piece, score the dashed lines with a utility knife, fold, and glue the one seam to close the box.

---

## Technical details

- **Single HTML file** — everything (HTML, CSS, JS) is in one self-contained file. No build step, no node_modules, no framework.
- **Three.js** for 3D rendering and STL parsing, loaded from CDN.
- **Runs entirely in the browser** — your STL files never leave your computer. No upload, no server, no tracking.
- **Works offline** after first load (CDN is cached).
- **Mobile-friendly** — responsive layout, touch-optimized controls, safe-area insets for notched phones.
- **Built-in ZIP writer** — the per-layer SVG ZIP export uses a from-scratch ZIP implementation (no JSZip dependency).

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
| `Esc` | Close modal / clear measurement |

---

## Screenshots

### Main interface with grid, ruler, and dimensions
![Main interface](screenshots/01-main-ui.png)

### Stats panel — layers, pages, area, height, sheets, dowel length
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
