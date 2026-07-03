# Atomicca — Interactive 3D Atomic Builder

A single-file, browser-based teaching tool for building atoms. Set the proton, neutron, and electron counts and watch the model update in real time — as a navigable 3D structure or a flat 2D Bohr-style diagram — with live stability, charge, and electron-configuration feedback.

No installation, no build step, no dependencies beyond one CDN script. Open `index.html` in a browser and it runs.

## Features

**Two view modes**
- **3D** — drag to orbit, scroll/pinch to zoom. Electrons orbit the nucleus on tilted paths; the nucleus glows with a fresnel "forcefield" shader colored by stability (green/amber/red).
- **2D** — a flat, classic Bohr-diagram view. Drag to pan, scroll/pinch to zoom. Electrons sit evenly spaced on flat concentric shell rings; protons and neutrons render as solid flat circles with a darker outline of their own color so they stay visually distinct; the nucleus is ringed by a simple line circle colored by stability instead of the 3D glow.

**Build any atom**
- Sliders *and* editable number boxes for protons, neutrons, and electrons — type a value directly instead of dragging if you prefer.
- A "jump to element" dropdown covering all 118 elements.
- Changing the proton count resets neutrons/electrons to that element's most common (neutral) isotope, but neutrons and electrons can each be overridden afterward to build ions and isotopes.

**Live readouts**
- Element name and symbol (with special-cased names for hydrogen's isotopes: Protium, Deuterium, Tritium).
- Mass number, net charge (cation/anion/neutral), and an estimated stability class.
- A one-line fact about the currently selected element.
- *(2D only)* A collapsible "Electron Configuration" panel showing both the shell notation (e.g. `2, 8, 1`) and the quantum-mechanical orbital notation (e.g. `1s² 2s² 2p⁶ 3s¹`), computed independently from the electron count via the standard Aufbau filling order.

**Playback & export**
- Pause/resume button — freezes electron motion, camera auto-rotate, and the nucleus pulse, and resumes from the exact point it was paused (no jump).
- Snapshot button — exports the current view as a PNG, with a choice of transparent or white background.
- Reset-view button — returns the camera to its default framing (respects whichever mode you're in).

**Mobile support**
- Single-finger drag (orbit in 3D / pan in 2D) and two-finger pinch-to-zoom work identically to mouse + scroll wheel.
- Responsive layout: legend and hint text adapt on small screens.

## How stability is estimated

This is a simplified teaching model, not a lookup of real isotope data:
- Elements from polonium (84) onward, plus technetium (43) and promethium (61), are always flagged as radioactive — accurate to real chemistry, since none of them have stable isotopes.
- For everything else, the tool compares the chosen neutron count against the neutron count implied by the element's standard atomic weight. Close matches are called "stable," larger deviations "unstable." It's a reasonable approximation of the real "band of stability," not a citation-grade isotope table.

## Technical notes

- **Stack**: vanilla HTML/CSS/JS + [Three.js r128](https://threejs.org/) (loaded from cdnjs). No build tooling, no package manager, no framework.
- **Camera**: a small hand-rolled orbit/pan/zoom controller (spherical coordinates in 3D, plane-translation in 2D) — not `THREE.OrbitControls` — so there's exactly one script dependency.
- **Nucleus**: protons and neutrons are packed into a 3D sphere volume via rejection sampling and rendered with `InstancedMesh` for performance, even at element 118 (~300 nucleons). In 2D mode, the same 3D-packed positions are viewed from a fixed front-on camera angle.
- **Electrons**: grouped into shells using the 2n² rule (capacities 2, 8, 18, 32, 32, 18, 8 — sums to exactly 118). In 3D they orbit on randomly tilted great circles; in 2D they're evenly spaced on flat rings.
- **Outlines** (2D only): a classic "inverted hull" trick — a slightly larger, back-face-only copy of each nucleon in a darkened shade, sitting just behind the solid-colored front face.
- **Element data**: all 118 elements with symbol, name, standard atomic weight (rounded, used for the isotope default), and a short fact.

## Known limitations

- The 2D nucleus outlines can occasionally look slightly uneven for very heavy elements — this comes from viewing 3D-packed nucleon positions from a flat angle, and is a known trade-off rather than a bug to be silently masked.
- Snapshot downloads use a standard browser `<a download>` link. Whether that prompts a "Save As" dialog or saves straight to your Downloads folder depends on your browser's own settings, not on this tool.
- Standard atomic weights for the synthetic/superheavy elements (104+) use the mass number of their most stable known isotope, since they have no naturally occurring abundance to average.
- This is an educational simplification of atomic structure (a Bohr-style shell model), not a quantum-mechanically accurate orbital shape visualization.

## Files

- `index.html` — the entire application (markup, styles, and logic in one file).
