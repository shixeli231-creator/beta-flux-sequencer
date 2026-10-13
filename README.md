# Beta-Flux Sequencer

A design study for a semiconductor-free, radiation-powered sequencer for 
timing and control of heavy machinery in environments where conventional 
electronics fail (EMP, high total ionizing dose, no resupply).

**Status:** Design study. No prototype built. Open for feedback and collaboration.

---

## What it is

Ni-63 beta flux charges an integrator capacitor. A mechanical threshold 
switch (MEMS pull-in) trips at a preset charge. The trip closes an output 
contact and shuts the beam gate.

**The defining property:** the value is defined by the *threshold*, not by 
the beam current. The source can decay 50% over 100 years and the output 
is still correct — it just takes longer. The device self-calibrates 
against source decay.

## Architecture

- Sealed spherical vacuum chamber (~14 cm diameter)
- Central balanced sphere (~3 cm), free-rolling, no bearings
- Directional Ni-63 source on one face, permanent magnet on spin axis
- Four radial lanes, each with: MEMS shutter, Faraday cup, thin-film 
  integrator, MEMS pull-in switch (comparator), spring-loaded reset, 
  output contact
- Magnetic plug stabilizes rotor and resets capacitors in one action
- Output is a mechanical contact closure. No display.

## What it can do

Threshold detection, addition by charge accumulation, integration,
ratio measurement, dose integration, event-based sequencing,
Boolean AND/OR, bang-bang process control, rate detection,
self-dating beacon.

## What it cannot do

General computation, floating point, division, transcendentals, 
reprogramming, anything above Hz speeds, precision beyond ~0.1–1%.

## Why it exists

Rad-hard chips have failure modes that cannot be engineered away: total 
ionizing dose has a ceiling, EMP destroys PN junctions regardless of 
radiation hardening, and the supply chain is a dependency. This device 
has no semiconductors in its compute path.

## Honest limits

- Not faster, smaller, cheaper, or more precise than a rad-hard chip
- Not claimed to be universally EMP-immune — claimed EMP-resistant 
  under a specified threat, verified by test
- Not claimed to be novel until a formal prior-art search is done
- Charge retention and MEMS shutter blocking efficiency are open questions

## Repository contents

- `docs/design-study.md` — full technical writeup
- `docs/technical-brief.md` — shorter version for engineers
- `docs/open-questions.md` — what's unresolved and what would test it
- `hardware/beta_flux_sequencer.scad` — OpenSCAD 3D model
- `hardware/macro-demonstrator.md` — breadboard circuit for validation
- `simulation/falstad-import.txt` — browser circuit simulator file
- `images/` — renders and screenshots

## How to run the model

Open `hardware/beta_flux_sequencer.scad` in OpenSCAD. Press F5 for 
preview. Set View > Animate, FPS = 20, Steps = 400.

## How to run the circuit

Open https://falstad.com/circuit, go to File > Import from Text, paste 
`simulation/falstad-import.txt`.

## Contributing

Feedback welcome. Open an issue or start a discussion. Specific things 
I'm looking for:

1. Comparator selection for sub-nA integrator nodes
2. Realistic breadboard leakage at these currents
3. MEMS shutter geometry for >99% blocking efficiency
4. Prior art — has anyone combined these principles before?

## License

BY-NC-ND 4.0. See `LICENSE`.

## Citation

See `CITATION.cff`. If you build on this, please cite it.

## Author

Furumiri
Baku, Azerbaijan
2026
