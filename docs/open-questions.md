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
NP0 ceramic) have leakage in the 1–10 fA range. At the corrected beam
current per lane of 8–40 pA, this is 1000–4000× below the signal. A
1 pF cap at 16 V holds 16 pC; at 10 fA leakage the time constant is
~1600 s. Over the seconds between cycles, loss is under 0.5%.

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

**Why it matters:** At the pA level, input bias current can be a
significant fraction of the signal current.

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

## 6. Switch type decision

**Question:** MEMS pull-in switch (8–16 V) or thyratron/spark gap
(50–100 V)?

**Why it matters:** This determines charge time. A MEMS switch at 16 V
trips in ~0.4–2 s depending on beam current. A thyratron at 50 V trips
in ~3–12 s. Both are valid; they lead to different designs.

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

## 8. Citation verification

**Question:** Are the specific figures attributed to external sources
correct?

**Why it matters:** Several claims in the design study reference
external papers and specifications. Some of these were paraphrased
from memory during drafting and need verification before formal
citation.

**What we think:** The general principles are correct. Specific
numbers (beam currents, drift rates, insulation resistance) may need
adjustment.

**How to test:** Verify each citation against the original source
before formal publication or competition submission.
