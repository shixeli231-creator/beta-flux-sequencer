# Beta-Flux Sequencer

A design study for a semiconductor-free, radiation-powered sequencer for
timing and control of heavy machinery in environments where conventional
electronics fail (EMP, high total ionizing dose, no resupply).

**Status:** v0.1.0-alpha-design.
Design study only. No prototype. All numbers theoretical. Open for feedback and collaboration.

---

## What it is

Ni-63 beta flux charges an integrator capacitor. A mechanical threshold
switch (MEMS pull-in) trips at a preset charge. The trip closes an output
contact and shuts the beam gate.

**The defining property:** the value is defined by the *threshold*, not
by the beam current. If the switch trips at 100 units of charge, the
device has counted to 100 regardless of how long it took. The source can
decay 50% over 100 years and the output is still correct — it just takes
longer to get there.

**Important distinction:** this property applies to *charge-based*
operations. Time-based operations (wall-clock timers, delays, scheduled
sequencing) are a different problem and are not decay-immune.

## Architecture

- Sealed spherical vacuum chamber (~14 cm diameter)
- Central balanced sphere (~3 cm), free-rolling, no bearings
- Directional Ni-63 source on one face, permanent magnet on spin axis
- Four radial lanes, each with: MEMS shutter, Faraday cup, integrator
  capacitor, MEMS pull-in switch (comparator), spring-loaded reset,
  output contact
- Magnetic plug stabilizes rotor and resets capacitors in one action
- Output is a mechanical contact closure. No display.

## What it can do

**Charge-based operations (decay-immune):**

- Threshold detection — trips at a preset charge
- Addition by charge accumulation
- Integration (the capacitor literally integrates current over time)
- Ratio measurement — two counters off one source
- Dose integration — counting radiation is native
- Event-based sequencing — stage N trips, enables stage N+1
- Boolean AND (shutters in series in the beam path)
- Boolean OR (two beams into one collector)
- Bang-bang process control on charge-based variables
- Hysteresis (native to the pull-in switch)

**Time-based operations (require stable beam current):**

- Wall-clock timing
- Delay
- Scheduled sequencing

*These are not decay-immune. A timer that fires after 10 seconds needs a
stable beam current. If the source decays 50%, it fires after 20 seconds
— a wrong answer. For accurate timing, the source must be
temperature-stabilized and the beam current calibrated against a
reference. The device's distinctive self-calibration property applies
only to charge-based operations.*

**Measurement:**

- Rate detection — trip interval reflects flux
- Self-dating beacon — pulse interval slows with source decay, with
  caveats around noise floor and leakage

## What it cannot do

General computation, floating point, division, transcendentals,
reprogramming, anything above Hz speeds, precision beyond ~0.1–1%.

## Why it exists

Rad-hard chips have failure modes that cannot be engineered away: total
ionizing dose degrades them gradually until they drift out of spec, EMP
destroys PN junctions regardless of radiation hardening, and the supply
chain is a dependency. This device has no semiconductors in its compute
path.

## Honest limits

- Not faster, smaller, cheaper, or more precise than a rad-hard chip
- Not claimed to be universally EMP-immune — claimed EMP-resistant
  under a specified threat, verified by test
- Not claimed to be novel until a formal prior-art search is done
- Charge retention and MEMS shutter blocking efficiency are open questions
- No semiconductors in the *compute path* — external interfaces may
  still use them

## Repository contents

- `docs/design-study.md` — full technical writeup
- `docs/technical-brief.md` — shorter version for engineers
- `docs/open-questions.md` — what's unresolved and what would test it
- `hardware/macro-demonstrator.md` — breadboard circuit for validation
- `hardware/beta_flux_sequencer.scad` — OpenSCAD 3D model (in progress)
- `simulation/falstad-import.txt` — browser circuit simulator file
- `images/` — renders and screenshots (in progress)

## Contributing

Feedback welcome. Open an issue or start a discussion. Specific things
I'm looking for:

1. Comparator selection for sub-nA integrator nodes
2. Realistic breadboard leakage at these currents
3. MEMS shutter geometry for >99% blocking efficiency
4. Prior art — has anyone combined these principles before?

## License

CC BY-NC-ND 4.0. See `LICENSE`.

## Citation

See `CITATION.cff`. If you build on this, please cite it.

## Author

Furumiri
Baku, Azerbaijan
2026
