# Open Questions

Honest list of what is unresolved. These are the things that need
answers before the design can be called validated.

## 1. Charge retention

**Question:** How long does the integrator capacitor hold its value
between addition cycles?

**Why it matters:** The addition mechanism depends on holding charge
across two count-to-threshold cycles. If leakage is too high, the
second operand is wrong.

**What we think:** Electrometer-grade capacitors (PTFE, polystyrene,
NP0 ceramic) have leakage in the 1–10 fA range — 100–1000× below the
4 pA beam current per lane. A 1 pF cap at 50 V should hold its value
for minutes. Over the seconds between cycles, loss is negligible.

**Precedent:** Haim et al., "An experimental apparatus for study of
direct β-radiation conversion for energy harvesting" (arXiv, 2014)
demonstrated a Ni-63 source producing 20 pA charging a parallel-plate
capacitor in vacuum, with ultra-low leakage achieved using Teflon
support structures.

**How to test:** Macro demonstrator, experiment 2. Charge to threshold,
disconnect source, measure voltage decay.

## 2. MEMS shutter blocking efficiency

**Question:** Can a MEMS shutter block the beam to >99% when closed?

**Why it matters:** Leakage during "closed" periods accumulates charge
on the integrator and drifts the threshold over long operations.

**What we think:** A physical shutter with sufficient overlap and
proper actuation should achieve this. Specific geometry needs design.

**How to test:** Prototype shutter in a vacuum chamber with a known
beam source, measure transmission in open and closed states.

## 3. Comparator input loading

**Question:** Does comparator input bias current load the integrator
node at sub-nA currents?

**Why it matters:** At sub-nA, input bias current can be a significant
fraction of the signal current.

**What we think:** Need a low-bias part (LMC6001, LMP7721) or a buffer
topology. The LM393 in the macro demonstrator may not scale down.

**How to test:** Macro demonstrator with different comparators, compare
charge time and linearity.

## 4. Vacuum longevity

**Question:** How long can the chamber hold vacuum?

**Why it matters:** Vacuum life is the primary limit on device life.
Leak rate determines whether this is a 5-year or 50-year device.

**What we think:** With proper bake-out, material selection, and an
internal getter, 20 years is plausible. Actual number requires test.

**How to test:** Sealed test chambers with different materials,
measure pressure vs. time over months.

## 5. EMP qualification

**Question:** Does the device survive a specified EMP threat?

**Why it matters:** We claim resistance under a defined threat,
not universal immunity. The claim needs to be verified.

**What we think:** Minimum loop area, shielded feedthroughs, high
breakdown margins, and arc-weld-resistant contacts should provide
protection. Specific threat level needs definition.

**How to test:** Build a non-radioactive prototype, expose to a
specified EMP environment, verify function.

## 6. Threshold voltage and switch type

**Question:** MEMS pull-in switch (5–30 V) or thyratron/spark gap
(50–100 V)?

**Why it matters:** This determines charge time. A MEMS switch at 10 V
trips in ~2.5 s. A thyratron at 50 V trips in ~12.5 s. Both are valid;
they lead to different designs.

**What we think:** MEMS for compactness, thyratron for higher voltage
margin and simpler output driving.

**How to test:** Design decision, not an experiment.

## 7. Prior art

**Question:** Has anyone combined these principles before?

**Why it matters:** The novelty claim depends on this. We believe
the individual components are known but the combination is not.

**What we think:** No prior art found in the surveyed literature.

**How to test:** Formal search — patent databases (Google Patents,
USPTO, EPO), academic literature (IEEE, Google Scholar), with terms
covering betavoltaics, MEMS logic, charge integration, and mechanical
computing.
