# Beta-Flux Sequencer — Technical Brief

## What it is

A semiconductor-free sequencer for timing and control of heavy machinery
in environments where conventional electronics fail (EMP, high TID, no
resupply). Not a general-purpose computer. A component — a triggered
switch with a built-in delay and threshold — intended to be wired into
larger systems.

## Core principle

Ni-63 beta flux charges an integrator capacitor. A mechanical threshold
switch (MEMS pull-in) trips at a preset charge. The trip closes an output
contact and shuts the beam gate.

**The defining property:** the value is defined by the *threshold*, not by
the beam current. If the switch trips at 100 units of charge, the machine
has counted to 100 regardless of how long it took. The source can decay
50% over 100 years and the output is still correct — it just takes longer.
The device self-calibrates against source decay.

## Architecture

- Sealed spherical vacuum chamber, ~14 cm diameter
- Central balanced sphere, ~3 cm, free-rolling (no bearings, no oil)
- Directional Ni-63 source on one face, permanent magnet on spin axis
- Four radial lanes, each containing:
  - MEMS electrostatic shutter (energy-independent beam gate)
  - Faraday cup with secondary-electron suppression grid
  - Integrator capacitor (PTFE or polystyrene, pF range)
  - MEMS pull-in switch as comparator (sharp snap, natural hysteresis)
  - Spring-loaded shorting bar for reset
  - Output contact
- Magnetic plug on top axis: stabilizes rotor (ferrite, non-conductive →
  no eddy braking) and resets capacitors via reed switches
- Manual spin-up via magnetic coupling through the wall (no penetration,
  no seal)
- Output is a mechanical contact closure. No display.

## The numbers so far

| Parameter | Value |
|---|---|
| Ni-63 half-life | 100.2 years |
| Beta endpoint energy | 66.9 keV |
| Source output (1 cm², 3 µm) | ~16 pA beam current |
| Per lane (4 lanes, 25% duty) | ~4 pA |
| Integrator capacitor | 1 pF |
| Pull-in voltage | ~8 V (2 µm gap, 100 µm × 100 µm electrode) |
| Charge time (MEMS, 10 V) | ~2.5 s |
| Charge time (thyratron, 50 V) | ~12.5 s |

## What's resolved

- **Energy budget** — 16 pA charges a pF cap in seconds. Workable.
- **Addition mechanism** — variant of dual-slope integration (reference
  switched mid-operation). Established technique.
- **Rotor stability** — precedent: electrostatic gyroscopes (Gravity
  Probe B, sub-0.17 marcsec/year drift). Precision balance + ferrite
  magnet.
- **Pull-in voltage** — ~8 V for typical MEMS geometry, compatible with
  cap voltage swing.
- **Capacitor leakage** — electrometer-grade caps (PTFE, polystyrene,
  NP0) have 1–10 fA leakage, 100–1000× below beam current. Precedent:
  Haim et al., arXiv 2014, Ni-63 at 20 pA charging a parallel-plate cap
  in vacuum with Teflon supports.

## What's open — where I need help

**1. Charge retention.** The capacitor holds its value between addition
cycles. Leakage looks negligible based on precedent, but hasn't been
measured in this specific configuration. Most important thing to test.

**2. MEMS shutter blocking efficiency.** The shutter must block the beam
to >99% when closed, or leakage during "closed" periods drifts the
threshold. Needs a specific geometry.

**3. Comparator input loading.** At sub-nA, input bias current could
matter. Looking for guidance on part selection or buffer topology.

**4. Vacuum longevity.** Chamber must hold vacuum for decades. Getter +
bake-out + material selection. Leak rate is the primary life limit.

**5. EMP qualification.** Not claimed immune — claimed resistant under a
specified threat. Mitigations: minimum loop area, shielded feedthroughs,
high breakdown margins, arc-weld-resistant contact geometry.

**6. Switch type.** MEMS (5–30 V, ~2.5 s trip) or thyratron/spark gap
(50–100 V, ~12.5 s trip)? Design decision.

## Macro demonstrator

Microamp/microfarad version of one lane. Same differential equation,
same behavior, buildable on a breadboard for ~$15.

- 10 MΩ charging resistor, 1 µF polystyrene cap, LM393 comparator,
  100k pot for threshold, LED + latch, momentary reset
- Charge rate: ~0.9 V/s
- Three experiments: charge time, state retention, addition (two
  sequential count-to-threshold cycles without reset)

**The addition experiment is the key result.** If two sequential charges
sum to the total, the core novelty is validated at macro scale. Then the
picoamp version is the same physics.

## What I'd like to talk about

1. Does the charge-integration approach seem sound, or is there a simpler
   way?
2. Comparator selection for sub-nA integrator nodes — what part or
   topology?
3. Realistic breadboard leakage at these currents, and how to measure
   around it.
4. If you were building this, what would you test first?
5. Anything obviously wrong that I haven't asked about.
