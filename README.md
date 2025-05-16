# CMOS Band-Gap Reference (OrCAD)

## Introduction  
This repository contains the schematic, simulation decks, and report for a CMOS band-gap reference (BGR) designed and verified in **OrCAD X**.  The circuit is based on a Brokaw-style topology and targets a **1.2 V reference voltage** with **< 100 ppm / °C** temperature drift, making it suitable for on-chip voltage references in data-converters, PLLs, and sensor interfaces. :contentReference[oaicite:1]{index=1}  

---

## Project Goals  

| Target | Specification |
|--------|---------------|
| Nominal V<sub>REF</sub> | 1.20 V ± 10 mV |
| Temp. Coefficient | ≤ 100 ppm / °C (20 °C – 80 °C) |
| Supply Voltage | 2.3 V (simulated) |
| Design Platform | OrCAD X / PSpice |

---

## Design Procedure  

1. **Topology Selection** – Adopted the Brokaw BGR architecture to combine a CTAT V<sub>BE</sub> term with a PTAT ΔV<sub>BE</sub> term, enabling first-order temperature cancellation.  
2. **Device & Model Choice** – Re-used the custom CMOS models characterized in Lab 4 rather than generic library parts for consistency across coursework projects.  
3. **Core Schematic Capture**  
   * **Band-gap core**: Two matched BJTs biased at 18:1 current-density ratio.  
   * **Error amplifier**: Replaces a simple diff-pair to force ΔV<sub>BE</sub> accurately.  
   * **MOS current mirrors**: Generate PTAT current and bias all branches.  
   * **On-chip startup circuit**: Injects a pulse of current at power-up and turns itself off once normal bias is reached.  
4. **Initial Simulation (–40 °C → 80 °C)**  
   * Tuned resistor R<sub>2</sub> (PTAT) and left-hand BJT sizing (CTAT).  
   * Achieved 16 ppm / °C **but** V<sub>REF</sub> ≈ 1.37 V – outside spec. :contentReference[oaicite:3]{index=3}  
5. **Operating-Range Refinement**  
   * Restricted design spec to **20 °C → 80 °C** per TA guidance.  
   * Re-optimised R-ratios and device areas until the ΔV<sub>BE</sub>/V<sub>BE</sub> intersection produced a flat-topped V-T curve centred on 1.2 V.  
6. **Final Verification**  
   * Performed temperature-sweep, bias-point, and supply-sensitivity analyses.  
   * Computed temperature coefficient and total power directly from PSpice probes.  

---

## Results  

| Metric | Simulation Result | Requirement |
|--------|------------------|-------------|
| V<sub>REF</sub> (25 °C) | **1.200 V** | 1.20 V ± 0.01 V |
| Temp. Coefficient | **≈ 72 ppm / °C** (20 °C – 80 °C) | ≤ 100 ppm / °C |
| V<sub>MAX</sub>/V<sub>MIN</sub> | 1.2034 V / 1.1982 V | — |
| Supply Current | 566.7 µA @ 2.3 V | — |
| Power | 1.30 mW | — | :contentReference[oaicite:5]{index=5}  

The design meets both the reference-voltage target and the sub-100 ppm / °C drift spec while maintaining modest power dissipation.  Curvature outside the 20 °C – 80 °C band is primarily due to second-order V<sub>BE</sub> effects and resistor mismatch, which can be addressed in future silicon layout with common-centroid techniques. :contentReference[oaicite:7]{index=7}  

---

## Repository Contents  

