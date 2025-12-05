Razavi CMOS Analog Simulation Workflow (LTspice)
================================================

Overview
--------
This document provides a safe, practical, concept-focused workflow for simulating Razavi-style CMOS analog circuits in LTspice when exact device sizes, process parameters, or foundry models are not available.

1) Choosing reasonable W/L values
---------------------------------
- Start with a practical minimum channel length: `L = 0.18–0.35 µm` (educational CMOS nodes).
- Use width `W` to set required transconductance `gm` or branch current. A commonly used analytic approximation is:

$$I_D \approx \tfrac{1}{2} \mu C_{ox} \tfrac{W}{L} (V_{GS}-V_T)^2$$

- Practical starting points:
  - Input differential pair: `L = 0.18–0.35 µm`, `W = 10–20 µm`.
  - Current mirror transistors: `W/L = 5–20`.
  - Output/gain-stage transistors: larger `W` (20–40 µm) to maximize `gm`.

- Simple scaling rules:
  - Increase `W` to increase current.
  - Increase `L` (e.g., 2× minimum) to increase output resistance and gain.

2) Selecting MOSFET models in LTspice
------------------------------------
- Option A: LTspice generic MOSFETs
  - `NMOS level=1` or `NMOS level=7 (BSIM3)`
  - `PMOS level=1` or `PMOS level=7 (BSIM3)`

- Option B: Use simple .model statements (Razavi Level 1 style)
  - Instead of hunting for external library files, you can define a generic process directly in your netlist or schematic.
  - Example for a generic 180nm-like process:

```
.model N_180n NMOS level=1 kp=200u Vto=0.4 lambda=0.1
.model P_180n PMOS level=1 kp=100u Vto=-0.4 lambda=0.1
```

3) Setting simulation conditions
--------------------------------
- Bias currents: use ideal current sources (Razavi examples commonly use 50–200 µA per branch). Example:

```
I1 node_plus node_minus 100u
```

- Power supply: use `1.2–1.8 V` for deep-submicron; `±1.5 V` for older processes. `1.8 V` is a safe default.
- Useful analyses:
  - Operating point: `.op` to read `gm`, `gds`, `Vov`, node voltages.
  - Small-signal AC: `.ac dec 100 1 1e9` to sweep frequency and find poles/zeros and gain.

4) Matching Razavi’s textbook intent
-----------------------------------
- Focus on device operating point (Vov), saturation, and device ratios more than exact process parameters.
- Recommended targets:
  - Keep `Vov = 100–200 mV` for analog operation.
  - Keep devices in saturation.
  - Use longer `L` (0.35–0.5 µm) for high-gain stages.
  - Use ratioing (W/L scaling) for mirrors and sizing (e.g., `1:1`, `1:4`, `1:10`).

5) Example: Differential pair (starting point)
--------------------------------------------
- Devices
  - Input pair: `L = 0.18 µm`, `W = 10 µm`.
  - Tail device: `W = 5 µm`.
- Tail current: `I_tail = 100 µA`.
- MOS model: use the generic .model statements defined above.

Expected textbook-like results: `gm ≈ 1–2 mA/V`, differential gain ~ `15–25 dB` (depends on load and bias).

6) Practical tips and verification
----------------------------------
- Always check `.op` to confirm saturation and gm/gds values.
- If you see unexpected oscillation, inspect phase margin and the feedback path — delays and device parasitics can change phase and cause instability.
- Use longer channel lengths and lower currents to increase gain and reduce parasitic poles for hand-calculated comparisons.

7) What you can ask me to add next
----------------------------------
- Example LTspice netlists for specific Razavi circuits (differential pair, cascode mirror, two-stage amplifier).
- A reusable `razavi_template.asc` (LTspice schematic) and a simple `.cir` netlist.
- gm/ID sizing assistance and a tiny script to sweep `W/L` vs. `gm/ID`.

Quick example snippets
----------------------
- Example `.model` usage in your netlist:

```
.model N_180n NMOS level=1 kp=200u Vto=0.4 lambda=0.1
M1 out in vss vss N_180n L=0.18u W=10u
```

- Example AC statement for gain/poles:

```
.ac dec 100 1 1e9
.op
```

Summary
-------
- Use generic CMOS models (180 nm or 350 nm), pick representative W/L ratios, bias with ideal currents and simple supplies, verify operating point, and run AC analysis. Focus on device regime and ratios rather than exact foundry parameters to reproduce Razavi’s conceptual results.

Next steps
----------
- Tell me which Razavi circuit or chapter you want simulated.
- I can add example LTspice netlists, a reusable simulation template, and a small gm/ID sizing helper.


