# The Beta-Flux Sequencer

### A Semiconductor-Free, Radiation-Powered Architecture for Long-Duration Control in Hostile Environments

---

## Abstract

This paper presents a design study for the Beta-Flux Sequencer, a device
for timing, sequencing, and control of heavy machinery in environments
where semiconductor electronics cannot be relied upon. The device uses
beta particles emitted by nickel-63 to charge an integrator capacitor,
which trips a mechanical threshold switch when a preset charge is
reached. Because the value of the output is defined by the threshold
rather than by the beam current, the device self-calibrates against
source decay for charge-based operations — a property that allows it to
remain accurate over a service life exceeding fifty years. The
architecture is a sealed vacuum sphere containing a free-spinning rotor
that carries a directional source, multiplexing a single source across
four independent lanes. Each lane contains a MEMS shutter, a Faraday
cup, an integrator, a MEMS pull-in switch acting as a comparator, and
an output contact. A magnetic plug provides rotor stabilization and
capacitor reset in a single action, with a spring-loaded fail-safe.
The device contains no semiconductors in its compute path, is resistant
to electromagnetic pulse under a specified threat environment, and
requires no external power to compute.

---

## 1. Introduction

### 1.1 The Problem

Modern digital electronics have two failure modes that cannot be
engineered away.

The first is **total ionizing dose (TID)**. High-energy particles and
gamma rays accumulate trapped charge in transistor gate oxides and
degrade semiconductor performance gradually. Radiation-hardened
(rad-hard) integrated circuits mitigate this, but every part has a
dose at which its parameters drift out of spec — typically 100 krad to
1 Mrad depending on the process — and after enough dose, it fails.

The second is **electromagnetic pulse (EMP)**. A nuclear EMP induces
massive currents in every conductor. It destroys PN junctions, which
means it destroys every semiconductor in its path. Rad-hard and
EMP-hard are different properties; a rad-hard chip is not
automatically EMP-hard.

There is a third, less-discussed problem: **supply chain dependence**.
A rad-hard chip must be manufactured in a fab, shipped through a
distribution network, and soldered onto a board. In a long-duration
mission, a post-disaster scenario, or any situation where resupply is
impossible, the availability of the chip is as important as its
survivability.

### 1.2 The Proposal

This paper proposes a device that sidesteps all three problems by
having no semiconductors anywhere in its compute path. It belongs to
the family of pre-semiconductor controllers — cam timers, stepping
switches, relay logic — but replaces the electric motor or mechanical
clockwork with a radioactive beta source.

The device is called the **Beta-Flux Sequencer**. Its core principle
is simple: beta particles from nickel-63 charge a capacitor; when the
charge reaches a set threshold, a mechanical switch trips; the trip is
the output. For charge-based operations, the output is correct
regardless of how much the source has decayed, how long the operation
took, or how much the beam current has drifted.

### 1.3 Scope and Honesty

This is a **design study**. No prototype has been built. The individual
components have precedents in existing technology, and the energy and
timing budget has been checked against those precedents, but the
integration has not been demonstrated.

The paper is written with three commitments:

- **No overclaiming.** Where a claim requires a calculation, the
  calculation is shown. Where a claim requires evidence that does not
  yet exist, it is identified as an open question.
- **No "EMP-immune."** The device is claimed to be *EMP-resistant
  under a specified threat environment*, to be verified by test.
  Universal immunity is not claimed and would be false.
- **No "better than rad-hard chips."** The device is slower, larger,
  more expensive, and less precise than a rad-hard chip. It is
  proposed for a specific niche where the chip's failure modes are
  unacceptable.

---

## 2. Background

### 2.1 Betavoltaics

Nickel-63 is a pure beta emitter with a half-life of 100.2 years and
a beta endpoint energy of 66.9 keV. It is used commercially in
betavoltaic batteries, which convert beta flux to electricity via a
semiconductor junction. Commercial devices such as the Chinese BV100
and the American NBV series use Ni-63 or tritium.

Enriched Ni-63 for betavoltaic use has a specific activity of
10–50 Ci/g depending on the enrichment level. This gives a raw beta
emission rate of roughly 10⁹ betas per second for a 1 cm² source of a
few micrometers thickness.

The relevance to this paper is the source, not the conversion. The
Beta-Flux Sequencer does not convert betas to electricity; it collects
their charge directly.

### 2.2 Mechanical and MEMS Logic

Mechanical computation for harsh environments is an active field.
Researchers have demonstrated mechanical NAND and NOR gates fabricated
with MEMS processes, and mechanical "hysterons" that perform simple
logic without electricity. The motivation in that literature is
explicitly radiation resistance.

This paper uses two specific MEMS devices: an electrostatic
**shutter**, a known technology for controlling particle beams, and a
**pull-in switch**, a cantilever that snaps into contact at a specific
applied voltage. The pull-in instability is a sharp, hysteresis-rich
threshold — a natural comparator with no semiconductor content.

### 2.3 Analog Charge Integration

Charge integration is one of the oldest techniques in analog
computation. The **dual-slope integrating ADC**, developed in the
1960s, integrates an unknown input for a fixed time and then
integrates a known reference in the opposite direction until the
integrator returns to zero. The result is the ratio of the two times.
The reference is switched mid-operation.

The Beta-Flux Sequencer uses the same principle in an inverted form:
it integrates until a reference is reached, then raises the reference
and integrates again. The final charge is the sum. This is the
mechanism by which addition is performed.

### 2.4 Electrostatic Gyroscopes

The problem of a spinning sphere in a vacuum, stabilized against
precession by a magnetic field, was solved for submarine inertial
navigation in the electrostatic gyroscope (ESG). ESG rotors are
magnetically stabilized, spin in ultra-high vacuum, and coast for
years. Gravity Probe B demonstrated drift rates on the order of
0.1 milliarcseconds per year for its gyroscope system, though the
exact figures depend on configuration. This is direct precedent for
the rotor stability requirement of the Beta-Flux Sequencer.

### 2.5 Low-Leakage Capacitors

For charge integration at the picoamp level, capacitor leakage is the
critical design constraint. Standard ceramic and electrolytic
capacitors leak in the nanoamp to microamp range — comparable to or
larger than the signal. But electrometer-grade capacitors leak in the
femtoamp range.

PTFE (Teflon) and polystyrene film capacitors have insulation
resistance in excess of 10¹⁵ Ω, corresponding to femtoamp-level
leakage at modest voltages. Certain NP0/CG0 ceramic capacitors from
reputable manufacturers also achieve femtoamp leakage under laboratory
conditions.

### 2.6 Prior Art in Radiation-Triggered Mechanisms

Radiation-triggered mechanical devices exist. A 1975 patent describes
a beta source accumulating charge on a plate until the plate deflects
and contacts a ground, with the cycle frequency used to measure
particle intensity. That device measures a *rate*; the Beta-Flux
Sequencer uses the threshold as a *preset quota* to define a value.
The distinction is the basis of the novelty claim.

Direct beta charging of a capacitor in vacuum has been demonstrated
in the energy-harvesting literature. The configuration is physically
plausible and the required component characteristics (low-leakage
capacitors, vacuum environment, precise charge integration) are all
available. Specific external citations in this document should be
verified against original sources before formal publication.

---

## 3. Design Principles

Three principles govern every decision in the architecture.

**Principle 1: The threshold defines the value.** The number of betas
required to trip the output is set by the physical threshold of the
switch, not by the beam current. For charge-based operations, this
makes the device self-calibrating against source decay. A source that
has lost 30% of its activity still produces the correct output; it
simply takes longer to get there. This property does **not** apply to
wall-clock timing operations, which require stable beam current.

**Principle 2: Eliminate every component whose failure mode cannot be
understood by a person with hand tools.** This rules out
semiconductors, which fail in ways that are invisible and require
specialist equipment to diagnose. It favors mechanical switches,
which fail visibly.

**Principle 3: Favor fail-loud over fail-silent behavior where
possible.** A digital system can corrupt a single bit and continue
operating on garbage. The Beta-Flux Sequencer has no program counter,
no register file, and no instruction stream. Its only state is the
charge on a capacitor, which is either below threshold or above it.
A fault typically produces a visible early or late trip rather than a
silent wrong answer. Some failure modes — capacitor dielectric
degradation, slow vacuum leak — can still produce gradual silent
drift, and this is acknowledged as a limitation.

---

## 4. Architecture

### 4.1 Overview

The device is a sealed spherical vacuum chamber roughly 14 cm in
diameter. At its center is a balanced sphere about 3 cm across, which
carries a directional Ni-63 source and a permanent magnet. The rotor
spins freely in the cavity and sweeps its beam around a ring of four
lanes. Each lane is an independent charge-integrating sequencer with
its own gate, collector, integrator, comparator, and output contact.
A removable magnetic plug on the top axis stabilizes the rotor and
resets the machine.

### 4.2 The Sealed Housing

The housing is a shell of non-magnetic metal (titanium, aluminum, or
stainless steel), a few millimeters thick, hermetically sealed. It
serves three functions:

1. Removes air from the beam path. Betas scatter off air molecules
   within a few centimeters at atmospheric pressure. In vacuum they
   travel ballistically for the full chamber dimension.
2. Blocks external beta radiation. Background betas cannot penetrate
   the shell. Gammas can, but mostly pass through without interacting,
   and any secondary electrons they produce are low-energy and
   absorbed by the shell. The interior is a beta-only environment.
3. Contains the radioactive material. The Ni-63 stays inside,
   preventing contamination of the installation.

A getter material inside absorbs residual outgassing over the
device's life.

### 4.3 The Rotor

The rotor is a precision-balanced sphere, roughly 3 cm in diameter,
free-rolling inside a spherical cavity. It carries:

- A thin film of Ni-63 on one face, with the rest of the sphere
  shielded by a thin layer of non-radioactive metal, producing
  directional emission over roughly one hemisphere.
- A permanent magnet aligned with the spin axis, which interacts with
  the external stabilizing field.

The rotor has no bearings, no oil, and no contact surfaces that wear
in normal operation. When the spin axis is vertical, the contact
point with the housing lies on the axis and has zero velocity during
rotation, so wear is negligible over decades. The rotor is spun up
at manufacture and coasts in vacuum.

### 4.4 The Lanes

Four lanes sit at 90° intervals around the rotor. Each lane contains,
in order from the rotor outward:

**The MEMS shutter.** A microscopic mechanical door, actuated
electrostatically, that swings across the beam path. When open, betas
pass. When closed, the beam is blocked. The shutter is a physical
object and contains no semiconductors. A physical shutter is preferred
over an electrostatic deflector because a deflector bends the
low-energy tail of a beta spectrum more than the high-energy tail,
which means a "closed" electrostatic gate still leaks high-energy
betas to the collector.

**The Faraday cup.** A metal cup that absorbs arriving betas and
accumulates their charge. A secondary-electron suppression grid,
biased at approximately −50 V, sits in front to prevent secondary
electrons from escaping and reducing the measured charge. This is
standard practice in electron beam instrumentation.

**The integrator capacitor.** A capacitor with an electrometer-grade
dielectric — PTFE (Teflon), polystyrene, or NP0/CG0 ceramic. Typical
value in the femtofarad to picofarad range, since the device
integrates charge rather than storing energy. The capacitor is the
device's only computational state. Leakage in these dielectrics is
1–10 fA, well below the beam current per lane.

**The MEMS pull-in switch.** A cantilever suspended above an
electrode. As the capacitor voltage rises, electrostatic force pulls
the cantilever down. At a specific voltage — the pull-in voltage —
the cantilever snaps into contact. The transition is sharp and
exhibits natural hysteresis. This is the device's comparator.

The pull-in voltage is given approximately by:

V_pi = √(8kg³ / 27ε₀A)

where k is the cantilever spring constant, g is the gap, ε₀ is the
permittivity of free space, and A is the electrode area. For a soft
cantilever with k = 10 N/m, a gap of 2 µm, and an electrode area of
100 µm × 100 µm, V_pi is approximately 16 V. For a softer cantilever
with k = 2.4 N/m, V_pi is approximately 8 V. Both are compatible with
the voltage range a picofarad capacitor can reach from a picoamp-scale
current.

**The spring-loaded shorting bar.** A brass bar sits across the
capacitor terminals, held away by the magnetic plug when the plug is
inserted. When the plug is removed, a spring pushes the bar down and
the capacitor discharges. This is the reset mechanism, and it is
fail-safe: if the plug falls out, if the operator walks away, if the
machine is knocked over, the spring resets everything.

**The output contact.** The electrical contact made when the pull-in
switch snaps. It is wired to whatever the device controls: a relay, a
valve, a motor starter, a safety interlock, or the enable input of
the next stage. The contact is the machine's output. There is no
display.

### 4.5 The Magnetic Plug

A removable ferrite magnet on the top axis serves two functions:

**Stabilization.** The rotor's embedded magnet interacts with the
plug's field. When the spin axis tilts away from vertical, a restoring
torque pulls it back, preventing precession. Ferrite is used rather
than a rare-earth conductive magnet because a conductive magnet would
experience eddy-current braking from the rotor's spinning field and
would slowly stop the rotor.

**Reset.** Reed switches inside the housing sit near the plug face.
When the plug is inserted, the reeds close and force the gates shut,
the capacitors discharged, and the machine zeroed. When the plug is
removed, the machine is idle.

The plug is a mechanical key. Insert it and the machine is stabilized
and reset. Remove it and the machine idles. There is no electrical
feedthrough and no seal to break.

### 4.6 The Manual Spin-Up

A magnetic coupling on the side or bottom of the housing allows the
rotor to be spun up manually. A hand crank carrying a magnet transmits
torque through the non-magnetic wall to the rotor's embedded magnet.
This is the same coupling principle as the reset plug and requires no
penetration, no seal, and no bearing inside the chamber. It allows
the machine to be restarted after long storage without any powered
equipment.

### 4.7 Output

The device has no display. This is deliberate. A display would require
power (a Nixie tube at 170 V, or an LED at a few volts), and either
option reintroduces a high-voltage conversion problem or a
semiconductor problem. A mechanical counter would work but adds a
subsystem with no computational benefit.

The output is a mechanical contact closure. Whatever is wired to it —
a relay coil, a solenoid, a safety shutter, the next stage — receives
the machine's decision. The state is visible in the state of the load.

---

## 5. Operation

### 5.1 Single Count-to-Threshold Cycle

1. Set the threshold. A reference capacitor or mechanical detent
   establishes the charge level at which the pull-in switch will trip.
   This is the device's operand.
2. Open the gate. The shutter moves aside. Betas reach the collector.
   Charge accumulates. The capacitor voltage rises.
3. Trip. When the voltage reaches the threshold, the pull-in switch
   snaps. The output contact closes. Simultaneously, a feedback wire
   closes the shutter and the beam is dumped.
4. Hold. The capacitor retains its charge and the output contact
   remains closed.
5. Reset. The magnetic plug is removed. The shorting bar drops. The
   charge is dumped, the gates are forced closed, and the machine
   returns to idle.

### 5.2 Addition

To add two operands, the device performs two count-to-threshold
cycles without resetting between them:

1. Set threshold to A. Open gate. Charge until trip. The capacitor
   now holds A.
2. Set threshold to A + B. Open gate. Charge until trip. The
   capacitor now holds A + B.
3. Output fires.
4. Reset.

The capacitor is not cleared between operations; its charge carries
forward. The first cycle fills it to A. The second fills it from A
to A + B. The final state is the sum. The reference is what changes.
The difference between the two references is the second operand.

This is not arithmetic in the conventional sense. It is filling a
bucket to a marked line, then raising the line and filling to the
new mark. The final water level is the answer.

The mechanism is a variant of dual-slope integration, in which the
reference is switched mid-operation and the result is a ratio of
integration times. The principle is established practice in
analog-to-digital conversion.

### 5.3 Sequencing

A single count-to-threshold cycle is one operation. A program is a
chain of them.

Stage 1 trips → its output contact opens stage 2's gate. Stage 2
trips → opens stage 3. Each stage has its own threshold, so each stage
does a different part of the job. Nothing happens out of order,
because the gates are physically in series in the beam path, or the
contacts are wired in series in the output circuit.

This is a state machine with no state register. The state is "which
stage is currently enabled," encoded in which gate is open. There is
no program counter, no instruction register, and no clock. Just a
chain of thresholds, each tripping the next.

---

## 6. Analysis

### 6.1 Energy Budget

**Source assumptions.** Source area 1 cm², source thickness 3 µm
(approximately optimal for self-absorption), nickel density
8.9 g/cm³. Enriched Ni-63 for betavoltaic use has a specific activity
of 10–50 Ci/g depending on enrichment level; the calculation below
uses both ends of that range.

**Source mass.** 1 cm² × 3 µm × 8.9 g/cm³ = 2.67 × 10⁻³ g.

**Activity.**

- At 10 Ci/g: 2.67 × 10⁻³ × 10 = 26.7 mCi
- At 50 Ci/g: 2.67 × 10⁻³ × 50 = 134 mCi

**Total beta emission.**

- Low: 26.7 × 10⁻³ × 3.7 × 10¹⁰ = 9.9 × 10⁸ betas/s
- High: 134 × 10⁻³ × 3.7 × 10¹⁰ = 4.9 × 10⁹ betas/s

**Forward emission.** Roughly 40% of betas escape forward (the rest
are absorbed in the substrate or self-absorbed within the film):

- Low: 4 × 10⁸ betas/s
- High: 2 × 10⁹ betas/s

**Raw beam current.** Each beta carries one electron charge:

- Low: 4 × 10⁸ × 1.6 × 10⁻¹⁹ ≈ 64 pA
- High: 2 × 10⁹ × 1.6 × 10⁻¹⁹ ≈ 320 pA

**Collection efficiency.** A directional source illuminating a
collector at the end of a lane can achieve 30–70% geometric
collection depending on collimation. Assume 50%:

- Low: ~32 pA
- High: ~160 pA

**Per-lane current (four lanes, 25% duty cycle):**

- Low: ~8 pA
- High: ~40 pA

**Charge time.** For a 1 pF integrator at a 16 V threshold,
Q = 16 pC:

- At 8 pA: t ≈ 2 s
- At 40 pA: t ≈ 0.4 s

**Verdict.** The energy budget is workable across the full range.
The device operates on a timescale of roughly 0.4–2 seconds per
count-to-threshold cycle, depending on source enrichment and
collection geometry. Both ends of the range are within the timescale
of heavy machinery control.

### 6.2 Beam Geometry

The spinning rotor distributes flux across the lanes. For four lanes
and a rotor at 50 Hz, each lane receives a pulse every 20 ms. The
integrator's time constant is on the order of seconds, so the pulse
train is smoothed into a continuous average current. The duty cycle
penalty is real (N× slower than a dedicated source per lane) but does
not affect correctness.

### 6.3 Charge Retention

Electrometer-grade capacitors (PTFE, polystyrene, NP0 ceramic) have
leakage currents in the 1–10 fA range. A 1 pF capacitor at 16 V holds
16 pC. At 10 fA leakage, the time constant is approximately 1600 s.
Over the seconds between two addition cycles, the fractional loss is
negligible — under 0.5%.

The physical plausibility of the configuration is supported by the
broader energy-harvesting literature, which has demonstrated direct
beta charging of capacitors in vacuum. Specific figures should be
verified against original sources before formal citation.

**This is the most important target for experimental validation.**

### 6.4 Rotor Stability

Electrostatic gyroscope rotors are magnetically stabilized, spin in
ultra-high vacuum, and coast for years. Gravity Probe B demonstrated
drift rates on the order of 0.1 milliarcseconds per year for its
gyroscope system, though the exact figures depend on which component
and configuration is being cited. This is direct precedent for the
rotor stability requirement of the Beta-Flux Sequencer.

Eddy-current braking is avoided by using a non-conductive external
magnet (ferrite). The rotor's own magnetization may use a conductive
material, but the external field source must not.

**Verdict.** Rotor stability is a solved problem with direct
precedent. The requirement is precision manufacturing, not new
physics.

### 6.5 EMP Resistance

The device is **not** claimed to be universally EMP-immune. Induced
currents will flow in the capacitor plates, wiring, collector,
feedthroughs, and output contacts. These currents can arc across the
shutter gap, jump the pull-in switch contacts, puncture the capacitor
dielectric, flash over insulators, or damage the output contact
through resistive heating.

The design targets **EMP resistance under a specified threat
environment**, verified by test. Mitigations include:

- Minimum conductor loop area in all wiring, to reduce induced EMF.
- Shielded feedthroughs and housing.
- High breakdown margins on all gaps.
- Contact geometry chosen to resist arc welding.
- Fast mechanical response where needed, so induced forces do not
  cause unintended contact.

The correct claim is: *the device is designed to survive a specified
EMP threat, verified by test.*

---

## 7. Capabilities and Limitations

### 7.1 What the Device Can Do

**Charge-based operations (decay-immune):**

- Threshold detection — trips at a preset charge
- Addition by charge accumulation
- Integration — the capacitor literally integrates current over time
- Ratio measurement — two counters off one source
- Dose integration — counting radiation is native
- Event-based sequencing — stage N trips, enables stage N+1
- Boolean AND — shutters in series in the beam path
- Boolean OR — two beams into one collector
- Bang-bang process control on charge-based variables
- Hysteresis — native to the pull-in switch

**Time-based operations (require stable beam current):**

- Wall-clock timing
- Delay
- Scheduled sequencing

These are **not** decay-immune. A timer that fires after 10 seconds
needs a stable beam current. If the source decays 50%, it fires after
20 seconds — a wrong answer. For accurate timing, the source must be
temperature-stabilized and the beam current calibrated against a
reference.

**Measurement:**

- Rate detection — trip interval reflects flux
- Self-dating beacon — pulse interval slows with source decay, with
  caveats around noise floor and leakage

**System properties:**

- Fail-loud behavior in most failure modes (with the acknowledged
  exception of gradual dielectric drift and slow vacuum leaks)
- Self-calibration against source decay for charge-based operations
- No external power required for computation
- Repairability with hand tools in the hand-sized version

### 7.2 What the Device Cannot Do

General computation, floating-point arithmetic, division,
multiplication except as repeated addition, transcendental functions,
reprogramming, anything above hertz-scale speeds, or precision beyond
approximately 0.1–1%.

The device is an instrument, not a processor.

---

## 8. Novelty Assessment

The individual elements of the design are known in separate fields:

- Radiation-induced charge accumulation for threshold detection (1975
  patent).
- Mechanical logic gates for harsh environments (MEMS literature).
- Charge integration with threshold triggering for radiation
  monitoring.
- Betavoltaic power sources (commercial products).
- Magnetically stabilized spinning spheres (electrostatic
  gyroscopes).
- Dual-slope integration with switched reference (ADC technique).
- Direct beta charging of a capacitor in vacuum (energy-harvesting
  literature).

The **combination** appears not to have been identified in the
surveyed literature. Specifically:

- The use of a **pre-set charge threshold to define an operand**, with
  the threshold raised between cycles to perform **addition by charge
  accumulation**, in a **mechanical, radiation-driven** device, does
  not appear in the literature surveyed.
- The **system-level integration** of a sealed vacuum sphere, a
  spinning directional source for time-division multiplexing, MEMS
  shutters, pull-in switches, and a magnetic reset — all for
  **long-duration, semiconductor-free control** — is not a
  configuration the author has found in prior work.

A formal prior-art search has not been performed. The claim is
therefore: *based on a survey of available literature, the individual
components are known but this specific combination has not been
identified.*

---

## 9. Feasibility and Open Questions

### 9.1 What Has Been Resolved

| Question | Status | Basis |
|---|---|---|
| Energy budget | Resolved | 8–40 pA per lane; pF cap charges in 0.4–2 s |
| Beam geometry | Resolved | Directional source + rotor at tens of Hz |
| Addition mechanism | Resolved | Dual-slope precedent |
| Rotor stability | Resolved | ESG precedent; precision balance + ferrite magnet |
| Pull-in voltage | Resolved | ~16 V for typical MEMS geometry |
| Capacitor leakage | Resolved | Electrometer caps at 1–10 fA |
| EMP resistance | Specified | Design against defined threat, test to verify |

### 9.2 What Remains Open

- Charge retention in this specific configuration
- MEMS shutter blocking efficiency at >99%
- Comparator input loading at pA currents
- Vacuum longevity over decades
- Shutter actuation life (~10 million cycles)
- EMP qualification against a specified threat
- Switch type decision: MEMS or thyratron
- Verification of external citations

### 9.3 What Would Need to Be Built

1. Vacuum packaging with a radioactive source inside.
2. Custom MEMS fabrication of shutter and pull-in switch.
3. Precision rotor balancing to sub-micron tolerance.
4. Long-term vacuum stability testing.
5. EMP qualification testing.

None of these is beyond current technology. Together, they represent
a multi-year development project.

---

## 10. Proposed Experimental Validation

The recommended first step is a **single-lane, non-radioactive
laboratory demonstrator** that tests the four core functions of the
compute chain.

### 10.1 Rationale

The physics of charge integration, threshold detection, state
retention, and reset is scale-invariant. The behavior of a microamp
current charging a microfarad capacitor is governed by the same
differential equation as a picoamp current charging a picofarad
capacitor. A macro-scale demonstrator therefore validates the
principle without requiring a radioactive source, a vacuum chamber,
or MEMS fabrication.

### 10.2 Proposed Circuit

A current source (a high-value resistor from a stable voltage supply)
charges a low-leakage capacitor. An op-amp buffer isolates the
capacitor node. A comparator with hysteresis detects the threshold
and latches. A pushbutton shorts the capacitor for reset.

Component values for a breadboard demonstrator: 10 MΩ charging
resistor, 1 µF polystyrene capacitor, LM393 comparator, 9 V supply.
Expected charge time to a 3 V threshold is approximately 3 seconds.

### 10.3 Experiments

1. **Charge time.** Measure time from reset to trip for a known
   threshold. Verify against t = CV/I.
2. **State retention.** Charge to threshold, disconnect the source,
   and measure voltage decay over time. Quantify leakage.
3. **Addition.** Charge to threshold A. Without resetting, raise the
   threshold to A + B and charge again. Verify that the sum of the two
   charge times equals the time to charge from zero to A + B in one
   operation.

The third experiment is the critical one. It validates the mechanism
by which the device performs arithmetic.

### 10.4 Simulation

A circuit simulation (Falstad, LTspice) can be used to model the same
chain and to explore parameter space before physical construction.

---

## 11. Conclusion

The Beta-Flux Sequencer is a design study for a semiconductor-free,
radiation-powered sequencer for long-duration control in environments
where conventional electronics fail. Its core principle — that the
threshold defines the value — makes it self-calibrating against source
decay for charge-based operations and gives it a service life limited
only by its vacuum seal and mechanical wear.

The device is not faster, smaller, cheaper, or more precise than a
rad-hard chip. It is proposed for a specific niche: slow, stubborn
control where the dominant threat is the one it was designed around,
and where resupply is impossible. In that niche it has properties no
semiconductor can match: it requires no external power to compute, it
contains no junctions to destroy, and it fails visibly rather than
silently in most failure modes.

Every subsystem has a precedent in existing technology. The
integration has not been demonstrated. The energy and timing budget
is consistent with those precedents. Charge retention and shutter
blocking efficiency are the open engineering questions and the targets
for the next phase of work.

The device is feasible in principle. It is not easy. It is worth
writing down.

---

## References

The following list identifies source categories and precedents for
the design. Specific figures attributed to external works should be
verified against original sources before formal citation.

1. Ni-63 betavoltaic literature — specific activity range for
   enriched Ni-63 (10–50 Ci/g).
2. Commercial betavoltaic products — Ni-63 and tritium sources.
3. Radiation-triggered mechanical switches — beta source with charge
   accumulation and mechanical trip (1975-era patents).
4. MEMS mechanical logic gate literature — mechanical NAND/NOR and
   hysterons.
5. Dual-slope integrating ADC — standard analog technique, 1960s
   origin.
6. Electrostatic gyroscope development — submarine inertial
   navigation, Gravity Probe B.
7. Faraday cup with secondary-electron suppression — standard
   electron beam instrumentation practice.
8. Direct beta charging of capacitors in vacuum — energy harvesting
   literature.
9. Electrometer-grade capacitor specifications — PTFE, polystyrene,
   NP0 ceramic leakage characteristics.

---

*This document is a design study by Furumiri. It is a defensive
publication establishing prior art as of 2026.*
