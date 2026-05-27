# OrcaSlicer Optimizer — Claude Skill

A Claude skill that acts as an expert assistant for FDM 3D printing optimization
in OrcaSlicer. It fetches live documentation from the official OrcaSlicer wiki
before answering, so paths and setting names are always current.

---

## What it does

- **Diagnoses print defects** — stringing, warping, ghosting, underextrusion,
  elephant foot, blobs, bad bridges, overhangs, delamination, and more
- **Optimizes prints toward a goal** — reduce print time, reduce material usage,
  maximize quality, or maximize strength — with concrete before/after estimates
- **Gives exact UI paths** — always specifies which profile (Printer / Filament /
  Process), which tab, and which mode (Simple / Advanced / Expert) is required
- **Calibration guidance** — temperature tower, pressure advance, flow ratio,
  max volumetric speed, retraction, VFA — in the correct order
- **Time and material estimates** — if you share your current slice data
  (time + grams from OrcaSlicer's Prepare tab), it calculates concrete savings

---

## Example prompts

**Defect diagnosis:**
> I'm printing PLA on my Bambu A1 and getting a lot of stringing between parts.
> What do I change in OrcaSlicer and where exactly?

**Print optimization:**
> I'm printing a decorative vase — 120mm tall, PLA, current settings: 3 walls,
> 15% gyroid, 0.2mm layer height. OrcaSlicer shows 2h 45min and 85g.
> I want to reduce material usage without losing surface quality. What should I change?

**Speed optimization (different printer):**
> Prusa MK4, PETG, 0.4mm nozzle. Functional bracket, 4 walls, 25% gyroid,
> 0.2mm layers, outer wall 40mm/s. Current time 1h 50min. Goal: finish in ~1 hour
> while keeping strength. What do I change?

**Calibration:**
> I want to calibrate pressure advance on my Bambu A1 in OrcaSlicer for the
> first time. Walk me through it step by step.

---

## Features

- Works with **any FDM printer** — Bambu Lab, Prusa, Creality, Voron, etc.
- Works with **any filament** — PLA, PETG, ABS, ASA, TPU, Nylon, PC
- Default context is **Bambu Lab A1 + PLA** when not specified, but always
  overrides with what you provide
- Fetches live wiki pages from `orcaslicer.com/wiki` before stating any path
- Full parameter coverage: walls, infill, bridges, overhangs, seam, ironing,
  layer height, speeds, acceleration, cooling, retraction, pressure advance,
  supports, and more
- Provides **before/after time and material estimates** when you share slice data

---

## Installation

### Option 1 — Install `.skill` file directly

1. Download `orcaslicer-optimizer.skill` from the
   [Releases](../../releases) page
2. Open Claude desktop app (Cowork mode)
3. Click **Save skill** when prompted, or drag the file into the skills panel

### Option 2 — Clone and use the SKILL.md

```
git clone https://github.com/a-nasstrom/orcaslicer-optimizer-skill.git
```

The skill is in `orcaslicer-optimizer/SKILL.md`.

---

## How it works

The skill instructs Claude to:

1. Identify the request type (optimization goal / defect diagnosis / settings question)
2. Fetch the relevant OrcaSlicer wiki page to verify current setting names and paths
3. Apply user-provided printer and material data (overrides built-in defaults)
4. Structure the response with exact paths, modes, start values, ranges, effects,
   and trade-offs for every recommended setting
5. Estimate time and material impact in concrete numbers when slice data is available

---

## Response format

Every optimization or diagnosis answer follows this structure:

```
### Summary
What's happening / what needs to change.

### Check first
2–4 physical/mechanical checks before touching slicer settings.

### What to change in OrcaSlicer
Parameter: [exact setting name]
Path:       [Profile] → [Tab] → [Section] → [Setting name]
Mode:       Simple / Advanced / Expert
Start:      [safe starting value]
Range:      [min – max]
Effect:     [what changes and why]
Trade-off:  [what you give up]

### Estimated impact
📊 Time: 2h 45min → ~2h 00min (≈ −27%)
   Material: 85g → ~62–68g (≈ −20–27%)

### Test print
Which built-in calibration or test model to run.

### Success criteria
What to look for to confirm improvement.

### Next step
What to do after the test.
```

---

## Covered parameters

| Category | Parameters |
|---|---|
| Quality | Layer height, Adaptive layer height, Line width, Wall generator (Arachne/Classic), Seam position, Ironing, Z contouring |
| Walls | Wall loops, Alternate extra wall, Wall/infill overlap |
| Infill | Density, Pattern (Gyroid, Cubic, Lightning, etc.), Infill wall overlap, Extra solid infill, Fill multiline |
| Bridges | Bridge flow ratio, Bridge density, Thick bridges, Extra bridge layers, Filter internal bridges, Counterbore bridging |
| Overhangs | Overhang speed, Overhang fan |
| Speed | Outer/inner wall, Infill, Top surface, Gap fill, Travel, Initial layer |
| Acceleration | Per-feature acceleration, Jerk XY |
| Cooling | Min/Max fan speed, Min layer time, Auxiliary fan |
| Filament | Temperatures, Flow ratio, Pressure Advance, Max Volumetric Speed |
| Retraction | Length, Speed, Wipe while retracting (Printer profile) |
| Supports | Normal supports, Tree supports, Support interface, Support density |
| Others | Brim, Skirt, Special mode (Vase/Spiral), Z-hop |

---

## Calibration workflow

The skill recommends this sequence for any new printer or filament profile:

1. Mechanics check (belts, bed, nozzle)
2. Z-offset + bed leveling
3. Temperature Tower
4. Max Volumetric Speed
5. Pressure Advance (Adaptive PA recommended)
6. Flow Ratio
7. Retraction
8. VFA (optional, for high-speed ringing)

All via the built-in **Calibration tab** in OrcaSlicer.

---

## License

MIT — use freely, modify as needed, contributions welcome.
