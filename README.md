# Ion Pairing Bench

A single HTML file for building plant tissue culture media. Drag a cation's charge across its anions and watch the resulting salts, solubility limits, and buffer pH update as you go.

There are no build steps, and no dependencies. Open the file in a browser and it runs.

## What it does

Media recipes aren't really just target ion concentrations — they're specific salts, and the salt you pick matters. CaCl₂ and Ca(NO₃)₂ give you the same calcium but very different chloride and nitrate loads, plus different solubility ceilings. This tool treats the pairing itself as the thing you edit, instead of something you work out afterward from a list of target numbers.

Each cation — Ca²⁺, Mg²⁺, K⁺, NH₄⁺, Na⁺ — gets its own composition plot showing how its charge splits across whichever anions it can pair with (Cl⁻, NO₃⁻, SO₄²⁻, MES⁻, citrate³⁻, H₂PO₄⁻, HPO₄²⁻, depending on the cation). Drag the marker, type an exact value, or use the slider — all three stay in sync with each other. Lock a corner to hold it in place while you adjust the rest, and the plot's own shape changes with it, anywhere from a triangle up to a hexagon depending on how many anions that cation has.

A few other things it does:

- Log-scaled magnitude slider per cation, from ⅛× to 5× a reference concentration
- A solubility cap on CaSO₄, held at half the literature gypsum solubility and enforced live
- Standard medium presets (MS, WPM, B5, DKW), plus a free-text parser for pasting in your own salt list
- A salt rubric listing every recognized name and molar mass, built from the same table the parser reads
- A shift-anions tool that moves charge from one anion to another, pulling first from whichever cation currently supplies the most of it
- A phosphate buffer pH panel that solves the actual triprotic equilibrium rather than leaning on Henderson-Hasselbalch alone
- Two read-only overview plots for the whole cation mix and whole anion mix
- A %/mM toggle that applies to every editable number in the app

## Quick start

Open `ion_pairing_bench.html` in a browser. Nothing else to install.

To share it as a link instead of a file, drop it on any static host — Netlify Drop, Cloudflare Pages, GitHub Pages all work, since nothing in the file depends on a server.

## Supported ions

**Cations:** Ca²⁺, Mg²⁺, K⁺, NH₄⁺, Na⁺
**Anions:** Cl⁻, NO₃⁻, SO₄²⁻, MES⁻, citrate³⁻, H₂PO₄⁻, HPO₄²⁻ — which ones apply to which cation is shown in the app's legend.

The full salt list, molar masses, and aliases live in the "Salt rubric" panel inside the app. It's generated from the same data the parser reads, so it can't quietly go out of sync with what's actually supported.

## Known simplifications

- Citrate is treated as fully ionized (citrate³⁻) for charge accounting. Real speciation at typical working pH runs lower.
- H₂PO₄⁻/HPO₄²⁻ reflect whichever salt you added, not a live pH-dependent equilibrium between the two — except in the phosphate pH panel, which does solve that equilibrium properly.
- That pH solve treats the counter-cation as an inert spectator. Fine for K⁺ or Na⁺; NH₄⁺ is itself a weak acid (pKa≈9.25) and this ignores that, though it only matters if the mix runs alkaline.
- MES also buffers real media (pKa≈6.15) but isn't part of the phosphate pH calculation — that panel only covers phosphate's own equilibrium, nothing else in the medium.
- The pKa₂ ionic-strength correction table stops at 2 M total phosphate. Anything above that just reuses the table's highest value instead of extrapolating further.
- The CaSO₄ cap comes from a literature gypsum solubility figure (~15.3 mM at 25°C), with no further ionic-strength correction on top of that. It clamps the actual composition correctly no matter how you set it — drag, slider, typed number, or the shift tool. What it doesn't do is stop the slider control itself from being dragged past the limit; only the 2D marker snaps back in real time. Drag the SO₄²⁻ slider past the cap and the thumb will overshoot visually until you let go, even though the value underneath was already correct.
- The shift-anions tool's "touch the fewest salts" behavior is a greedy heuristic — it drains the biggest current supplier first — not a guaranteed minimum. There's a rare case where two mid-size cations would beat one large one that it won't catch.
- Cations with more than three anion corners (K's hexagon, Na's and Ca's quads) can't have every mathematically possible composition reached by a 2D drag — only a 2D slice of it. The shift tool edits numbers directly and can occasionally land just off that reachable surface, which shows up as a small visual snap the next time you grab the marker.
- Bar-chart labels are sized to match their segment's width exactly, so a very small segment's label can end up truncated or effectively invisible rather than overlapping its neighbor.
- None of this accounts for chelation (Fe-EDTA/EDDHA), pH drift during autoclaving, or interactions with organics, sucrose, or gelling agent.

## License

Unset. Add one if you want to define how this can be reused.
