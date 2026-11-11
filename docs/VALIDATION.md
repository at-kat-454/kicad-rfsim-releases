# Validation and known limits

What RF Sim has been checked against, how well it agrees, and what it does not model. Version 0.26.4.

## Solver and method

* Field solver: openEMS (EC-FDTD on a rectilinear mesh), v0.37.0. RF Sim builds the model from the
  KiCad board (copper, vias, stackup, parts) and evaluates the results; it does not change the solver.
* RF chain: S-parameters of the layout with ports on the parts to tune, then circuit math with the
  part models (ideal L / C with Q and ESL, or vendor Touchstone files). Antenna: feed impedance,
  efficiency and the 3D far field from one transient run.
* Every result carries plausibility checks (passivity, reciprocity, energy balance, copper dropped
  by the solver) and a *How this was solved* page with the mesh and run statistics. The mesh
  convergence check runs a setup on two or three meshes and reports how much the results still move.

## Test bench with known answers

`verification/verify.py` builds small KiCad boards whose answers are known without a field
simulation and runs them through the whole plugin. Results of version 0.25.0:

| Case | Reference | Result | |
|---|---|---|---|
| 50 Ω microstrip (0.2 mm FR-4) | Hammerstad-Jensen Z0 / ε_eff | 51.6 Ω (+3.2 %), 3.28 (−1.6 %) | ok |
| Grounded CPW, 0.2 mm gap (4-layer) | conformal mapping | 60.6 Ω (−0.6 %), 3.20 (+3.9 %) | ok |
| Grounded CPW, 1.27 / 0.2 mm on 1.6 mm FR-4 (2-layer) | conformal mapping | 48.4 Ω (−3.0 %), 2.85 (+2.4 %) | ok |
| Open λ/4 stub, 2 GHz | λ/4 with open-end extension | 2.046 GHz (+2.3 %) | ok |
| Series / shunt C and L (0402), 868 MHz and 2 GHz | ideal circuit with the same part model | within 0.04-0.7 dB | ok |
| Parts inside the field solver vs. ports + circuit math, 868 MHz | the two must agree | within 0.22 dB | ok |
| C-L-C low-pass, -3 dB cutoff: parts inside the solver vs. ports | the two must agree within 2 % | 884 vs 927 MHz (−4.6 %) | open |
| Pi network 1 pF / 2.7 nH / 1 pF on 1.6 mm FR-4, 2.4 GHz: parts inside the solver vs. ports | the two must agree within 0.5 dB | 0.75-1.06 dB | open |
| 71 mm strip dipole | thin-dipole theory | 0.469 λ, 71.8 Ω, 2.07 dBi, 98 % | ok |
| Dipole in an ABS box | small downward shift (0 to −3 %) | −0.51 % | ok |
| Patch, 2.45 GHz, 1.6 mm FR-4 | transmission-line model | 2.377 GHz (−3.0 %), 6.0 dBi | ok |
| 2.4 GHz project board (U.FL, coplanar line, pi network, inverted F) | design window 2.32-2.56 GHz, mesh convergence, reachable match | resonance 2.366 GHz; Automatic → Accurate +0.57 %; efficiency 89 %; E24 match −14.4 dB over 2400-2483.5 MHz | ok |
| same, directivity on three meshes | converged within 0.3 dB | 0.6 / 1.6 / 2.3 dBi (Fast / Automatic / Accurate) | open |

Open points:

* **Parts inside the solver vs. ports + circuit math** differ by up to 1.1 dB on the 2.4 GHz pi network
  (1.6 mm board) and by 4.6 % in a low-pass cutoff at 868 MHz. The tuning viewer uses ports + circuit
  math; the wave view uses the parts inside the solver.
* **Directivity of a small board** (44 x 30 mm inverted F) still moves by 0.7 dB from the Automatic to
  the Accurate mesh; resonance, impedance and efficiency are converged. Use Accurate for directivity of
  small boards.
* The ideal pi network (no layout) gives −0.27 dB; the layout gives −3 to −3.7 dB at 2.4-2.48 GHz. The
  0402 pads, the 1.6 mm vias of the shunt parts and the line widths probably detune it (a shunt path of
  about 2 nH turns 1 pF into about 1.9 pF at 2.44 GHz). There is no closed-form reference for that, so
  only a measurement can confirm it.

## Comparison with measured antennas: TI CC-Antenna-DK2

Eight PCB antenna boards of TI's antenna kit (433 MHz to 2.4 GHz), simulated from the board files
with the bill of materials, no tuning to the measurement, compared with TI's measurements
(application report SWRA496A: SWR, impedance, CTIA over-the-air efficiency and 3D pattern).
Full report with all tables and pattern comparisons: `rfsim_ti_dk2_verification.pdf`; scripts and
results: `verification/ti_dk2/` (the KiCad projects are built from TI's public reference design zip).

Agrees:

* resonance frequencies: sub-GHz boards within 0-4 %; band edges of the 2.4 GHz boards within 1-7 %;
* total efficiency: 14 of 19 values within TI's +-3 dB chamber uncertainty;
* reactance of the bare antennas (without matching parts) within 5-15 ohm;
* 3D pattern shape where the measurement is free of fixture effects (correlation 0.6-0.9; one board at
  2.44 GHz: 0.90 and the same directivity).

Does not agree, the same way on all boards:

* the 2.4 GHz resonances of the meander and inverted-F boards are 3-9 % high;
* the feed resistance is lower than measured (about half at 2.4 GHz, more on electrically small
  sub-GHz boards), so the simulated bandwidth is narrower;
* TI's directivity (4-6 dBi on every board) is higher than simulated on the small boards.

The measurement includes the coax cable and the positioner arm. On small boards the cable shield
is part of the radiator: a model with a 100 mm cable raised one board's efficiency at 868 MHz from
0.02 % to 36 % (TI: 52 %). TI's own pattern tables vary by up to 11 dB at the poles, where the value
cannot depend on the positioner rotation. The simulation describes the board without the cable.

## Not modelled

* **Solder mask.** Lowers the resonance of a printed antenna by about 1-2 % at 2.4 GHz.
* **Cables and fixtures.** A coax cable or a wire on the ground plane changes small antennas
  strongly (see above). Measure with ferrite chokes on the cable to compare with the simulation.
* **Part self-resonance and frequency-dependent Q.** Parts are ideal L / C with a constant Q at
  the band centre (default Q 30 for L, 150 for C), unless a vendor Touchstone file is used.
* **Material dispersion.** One permittivity and loss tangent per dielectric (from the KiCad stackup);
  FR-4 varies by +-5 % between fabs and with frequency.
* Copper roughness; curved copper is staircased on the mesh; ports are lumped.

## Run time

On a 4-core desktop: a 2.4 GHz antenna 5-15 min (Automatic), a sub-GHz module 1-3 h (Automatic);
Fast is 3-6x quicker, Accurate 3-5x slower. RF chain setups take minutes.
