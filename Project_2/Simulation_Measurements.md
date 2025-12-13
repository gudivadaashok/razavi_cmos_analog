# SPICE Measurement Commands (`.meas`)

This guide explains the measurement commands used in `FOLDED_CASCODE_OPAMP.cir` and provides additional useful commands for analyzing your output.

## 1. Current Measurements (Slew Rate)
Your netlist currently calculates the **Slew Rate** (how fast the output changes).

```spice
.meas TRAN SR_rise DERIV V(out) WHEN time=1.05u
.meas TRAN SR_fall DERIV V(out) WHEN time=6.05u
```

*   **`TRAN`**: Tells SPICE to measure this during the Transient analysis.
*   **`SR_rise` / `SR_fall`**: The name you give to the result.
*   **`DERIV V(out)`**: Calculates the derivative ($\frac{dV}{dt}$) of the output voltage.
*   **`WHEN time=1.05u`**: Takes the measurement at exactly 1.05µs (just after the input steps up).

---

## 2. Other Useful Measurements

### A. Measure Peak-to-Peak Voltage
Check the full swing of your output signal.
```spice
.meas TRAN V_max MAX V(out)
.meas TRAN V_min MIN V(out)
.meas TRAN V_pp PARAM V_max-V_min
```

### B. Measure Average DC Offset
Check if your output is centered around 0V (or your desired common mode).
```spice
.meas TRAN V_avg AVG V(out)
```

### C. Measure Propagation Delay
Measure how long it takes for the output to react to the input.
*   Assumes input steps at 1.0µs.
*   Measures when output crosses 0V.
```spice
.meas TRAN delay_time TRIG time=1.0u TARG V(out) VAL=0 RISE=1
```

### D. Measure Gain (in AC Analysis)
If you run an AC analysis (`.ac dec 10 1 1G`), you can measure the bandwidth.
```spice
.meas AC unity_gain_freq WHEN vdb(out)=0
.meas AC phase_margin FIND vp(out) WHEN vdb(out)=0
```

---

## 3. Simulation Commands (.op, .dc, .ac)
These commands control *what* the simulator calculates. You usually run only one at a time (comment out the others with `*`).

### A. Operating Point Analysis (`.op`)
Calculates the DC bias point of the circuit (all voltages and currents at t=0).
```spice
.op
```
*   **Use for:** Checking if transistors are in saturation (Region 2) and verifying bias voltages (`node_b2`, `node_v`, etc.).
*   **Output:** A list of DC voltages for every node and currents for every device.

### B. DC Sweep Analysis (`.dc`)
Sweeps a voltage source over a range to see the DC transfer characteristic.
```spice
.dc Vin_p -0.02 0.02 0.0001
```
*   **`Vin_p`**: The source to sweep.
*   **`-0.02 0.02`**: Start at -20mV, Stop at +20mV.
*   **`0.0001`**: Step size of 100µV.
*   **Use for:** Plotting $V_{out}$ vs $V_{in}$ to see the **DC Gain** (slope) and output swing limits.

### C. AC Analysis (`.ac`)
Sweeps the frequency to find the frequency response (Gain and Phase vs Freq).
```spice
.ac dec 10 1 100meg
```
*   **`dec 10`**: Take 10 data points per decade (logarithmic scale).
*   **`1`**: Start frequency (1 Hz).
*   **`100meg`**: Stop frequency (100 MHz).
*   **Use for:** Finding Bandwidth, Gain Margin, and Phase Margin.
*   *Note:* Requires `AC 1` to be set in your input source (e.g., `Vin_p ... AC 1`).

---

## 4. How to View Results
1.  **Run Simulation** in LTspice.
2.  Press **Ctrl + L** (or go to `View > SPICE Error Log`).
3.  Scroll to the bottom to see the calculated values.
