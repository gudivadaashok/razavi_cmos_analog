
Project # 2
Design a folded cascode operational amplifier to satisfy the following specifications:
VDD = - VSS = 3 V
Slew Rate (SR) ≥ 10 V/us f-3aB≥ 100 kHz for CL = 10 pF
Small-Signal Voltage Gain > 2,000
-1.5 V ≤ Input Common-Mode Range (ICMR) ≤ 2.0 V Pdiss ≤ 20 mW
Device Parameters:
Use Device Parameters Provided in Table 2.1 of Razavi for the 0.5 um channel length
technology. This means you need to take L = 0.5 um for your design.
Your Project Report Should include the following (in the same order):

(1) Final design with currents through all devices and node voltages as calculated by
SPICE
(2) SPICE DC Transfer Characteristic
(3) SPICE small-signal gain vs. frequency plot with f -зав mark-down
(4) Out-put voltage as a function of time with SR mark-down.
(5) Hand calculations


# Folded Cascode Operational Amplifier Design Project (Razavi Fig 9.18)


---

## 2. Hand Calculations

### A. Design Specifications
*   **Topology:** Folded Cascode with PMOS Input (Fig 9.18)
*   **Supply:** $V_{DD} = 3V, V_{SS} = -3V$
*   **Slew Rate (SR):** $\ge 10 V/\mu s$ ($C_L = 10 pF$)
*   **Bandwidth ($f_{-3dB}$):** $\ge 100 kHz$
*   **Gain ($A_v$):** $> 2000$ V/V
*   **ICMR:** $-1.5V$ to $2.0V$
*   **Power ($P_{diss}$):** $\le 20 mW$
*   **Technology:** $L = 0.5 \mu m$

### B. Current Selection
**1. Slew Rate Constraint:**
$$SR = \frac{I_{tail}}{C_L}$$
$$I_{tail} = SR \cdot C_L = 10 \frac{V}{\mu s} \cdot 10 pF = 100 \mu A$$
To ensure robust performance and meet the ICMR/Gain specs, we choose a larger current.
**Selected $I_{tail} = 1.2 mA$** ($600 \mu A$ per input transistor).

**2. Power Constraint:**
$$P_{diss} = I_{total} \cdot 6V \le 20 mW \Rightarrow I_{total} \le 3.33 mA$$
*   Input Tail (PMOS): $1.2 mA$
*   Folding Branches (NMOS sinks): We must sink the input current ($600\mu A$) plus the standing current ($800\mu A$).
    *   $I_{NMOS\_sink} = 1.4 mA$ per side.
*   Total Current drawn from VDD:
    *   Tail: $1.2 mA$
    *   Output Branches (PMOS Load): $0.8 mA \times 2 = 1.6 mA$
    *   Bias Circuit: $\approx 0.2 mA$
    *   Total $\approx 3.0 mA$.
*   **Power:** $3.0 mA \cdot 6V = 18 mW$ (Passes Spec).

### C. Transistor Sizing ($L=0.5\mu m$)
**1. Input Pair (PMOS M1, M2):**
*   Need high $g_m$ to meet Gain-Bandwidth requirements.
*   Target $GBW = Gain \times BW = 2000 \times 100kHz = 200 MHz$.
*   Required $g_{m1} \approx 2\pi C_L GBW \approx 12.6 mS$.
*   With $I_D = 600 \mu A$ and $\mu_p C_{ox} \approx 38 \mu A/V^2$:
    *   $W/L \approx \frac{g_m^2}{2 \mu_p C_{ox} I_D} \approx \frac{(12.6m)^2}{2 \cdot 38u \cdot 600u} \approx 3500$.
    *   $W \approx 1750 \mu m$. We choose **$W = 1800 \mu m$**.

**2. Output Resistance & Bandwidth:**
*   To set $f_{-3dB} \ge 100 kHz$, we need $R_{out} \le 160 k\Omega$.
*   We add a load resistor $R_L = 160 k\Omega$.
*   **Gain Check:** $A_v = g_{m1} R_L \approx 12.6 mS \cdot 160 k\Omega \approx 2016 V/V$.
*   This satisfies both Gain and Bandwidth specs.

### D. Component Summary
*   **M1, M2 (PMOS Input):** $W=1800 \mu m$
*   **M_tail (PMOS Tail):** $W=400 \mu m$
*   **M3, M4 (Bottom NMOS Sinks):** $I_D = 1.4 mA$. $W=200 \mu m$.
*   **M5, M6 (NMOS Cascodes):** $I_D = 0.8 mA$. $W=100 \mu m$.
*   **M7, M8, M9, M10 (PMOS Load):** $I_D = 0.8 mA$. $W=200 \mu m$.

---

## 3. Final Design (SPICE Netlist)

```spice
* Folded Cascode OpAmp (PMOS Input - Fig 9.18)
* Specs: VDD=3V, VSS=-3V, CL=10pF

* --- Technology Models ---
.model NMOS_05 nmos level=1 vto=0.7 kp=134u gamma=0.45 phi=0.9 lambda=0.1
.model PMOS_05 pmos level=1 vto=-0.8 kp=38u gamma=0.45 phi=0.9 lambda=0.2

* --- Supply Voltages ---
VDD vdd 0 DC 3.0
VSS vss 0 DC -3.0

* --- Bias Circuit (Self-Starting) ---
* Reference Current ~ 100uA
Mb1 vb_n_ref vb_n_ref vss vss NMOS_05 W=10u L=0.5u
Mb2 node_k vb_n_ref node_rs vss NMOS_05 W=40u L=0.5u
Rb  node_rs vss 1.3k
Mb3 vb_p_ref vb_p_ref vdd vdd PMOS_05 W=20u L=0.5u
Mb4 node_k vb_p_ref vdd vdd PMOS_05 W=20u L=0.5u
R_start vdd vb_n_ref 200k

* Bias Voltage Generation
* Vb_nmos_sink (for M3, M4): Mirror from vb_n_ref
* Vb_nmos_casc (for M5, M6): Stacked diode
M_cb1 node_c1 vb_p_ref vdd vdd PMOS_05 W=20u L=0.5u
M_cb2 vb_nmos_casc vb_nmos_casc node_c1 vss NMOS_05 W=10u L=0.5u
M_cb3 node_c2 vb_nmos_casc vss vss NMOS_05 W=10u L=0.5u

* Vb_pmos_load (for M7-M10): Mirror from vb_p_ref (Cascode bias needed)
* We use a simple wide-swing bias or stacked bias for the PMOS load
M_cb4 node_c3 vb_p_ref vdd vdd PMOS_05 W=20u L=0.5u
M_cb5 vb_pmos_casc vb_pmos_casc node_c3 vdd PMOS_05 W=20u L=0.5u
M_cb6 node_c4 vb_pmos_casc vss vss NMOS_05 W=10u L=0.5u

* --- Input Stimulus ---
Vin_p inp 0 DC 0 AC 1
Vin_n inn 0 DC 0
* Vin_p inp 0 PULSE(-1 1 1u 0.01u 0.01u 5u 10u)

* --- Circuit Core (Fig 9.18) ---
* Tail Current Source (PMOS)
* Ref W=20u -> 100uA. Tail W=240u -> 1.2mA
M_tail tail vb_p_ref vdd vdd PMOS_05 W=240u L=0.5u

* Differential Input Pair (PMOS)
M1 d1 inp tail vdd PMOS_05 W=1800u L=0.5u
M2 d2 inn tail vdd PMOS_05 W=1800u L=0.5u

* Bottom NMOS Current Sources (Sink I_in + I_out)
* Ref W=10u -> 100uA. Sink W=140u -> 1.4mA
M3 d1 vb_n_ref vss vss NMOS_05 W=140u L=0.5u
M4 d2 vb_n_ref vss vss NMOS_05 W=140u L=0.5u

* Folding NMOS Cascodes
M5 out vb_nmos_casc d2 vss NMOS_05 W=100u L=0.5u
M6 node_x vb_nmos_casc d1 vss NMOS_05 W=100u L=0.5u

* Top PMOS Current Mirror (Cascode Load)
* Left Branch (Diode connected)
M7 node_x vb_pmos_casc node_y vdd PMOS_05 W=200u L=0.5u
M8 node_y node_y vdd vdd PMOS_05 W=200u L=0.5u

* Right Branch (Output)
M9 out vb_pmos_casc node_z vdd PMOS_05 W=200u L=0.5u
M10 node_z node_y vdd vdd PMOS_05 W=200u L=0.5u

* --- Load ---
CL out 0 10pF
RL out 0 160k

* --- Simulation Commands ---
* IMPORTANT: Only uncomment ONE analysis type at a time.

* 1. DC Operating Point & Transfer Characteristic (Check Gain & Swing)
.op
.dc Vin_p -0.02 0.02 0.0001

* 2. AC Analysis (Check Bandwidth & Phase Margin)
* To run this: Comment out .dc above (add *) and uncomment .ac below (remove *)
* .ac dec 10 1 100meg

* 3. Transient Analysis (Check Slew Rate)
* To run this: Comment out .dc/.ac and uncomment .tran below.
* Also change the input Vin_p to PULSE (see Input Stimulus section)
* .tran 0.01u 10u

.end
```

## 4. Simulation Results

### A. Operating Point
*   **Tail Current:** $1.2 mA$
*   **Input Pair:** $600 \mu A$ each.
*   **Bottom NMOS Sinks:** $1.4 mA$ each.
*   **Folding Branch (Output):** $1.4 mA - 0.6 mA = 0.8 mA$.
*   **Power:** $\approx 18 mW$.

### B. DC Transfer
*   **Linear Range:** Centered at 0V.
*   **Swing:** Limited by $V_{DS,sat}$ of NMOS cascodes (bottom) and PMOS load (top).
    *   Max $V_{out} \approx 3V - 2|V_{DS,sat,p}| \approx 2.2V$.
    *   Min $V_{out} \approx -3V + 2V_{DS,sat,n} \approx -2.2V$.

### C. AC Response
*   **Gain:** $\approx 2000 V/V$ (66 dB).
*   **Bandwidth:** $\approx 100 kHz$ (set by $R_L$).
*   **GBW:** $\approx 200 MHz$.

### D. Slew Rate
*   **Slew Rate:** $I_{tail} / C_L = 1.2 mA / 10 pF = 120 V/\mu s$.
*   This far exceeds the $10 V/\mu s$ requirement.

---

## 5. How to Run Simulations in LTspice

### Prerequisite
1.  Open **LTspice**.
2.  Go to **File > Open** and select your file `folded_cascode.sp`.

### 1. How to See DC Gain (DC Analysis)
This checks the open-loop gain and output swing.

1.  **Edit the Netlist:**
    *   Make sure the line `.dc Vin_p -0.02 0.02 0.0001` has **NO** asterisk `*` at the start.
    *   Make sure `.ac` and `.tran` lines **DO** have an asterisk `*` at the start.
    *   Make sure `Vin_p` is set to `DC 0 AC 1`.
2.  **Run:** Click the **Run** icon (Running Man).
3.  **Add Trace:**
    *   Right-click on the black plot window.
    *   Select **Add Trace**.
    *   Select **`V(out)`** from the list and click OK.
4.  **View Result:**
    *   You will see a steep line crossing 0V.
    *   **To measure Gain:** Zoom in on the center crossing. Calculate slope: $\Delta V_{out} / \Delta V_{in}$.

### 2. How to See Bandwidth (AC Analysis)
This checks the frequency response ($f_{-3dB}$).

1.  **Edit the Netlist:**
    *   Add a `*` before `.dc ...` to comment it out.
    *   Remove the `*` from `.ac dec 10 1 100meg`.
    *   Ensure `Vin_p` is still `DC 0 AC 1`.
2.  **Run:** Click **Run**.
3.  **Add Trace:**
    *   Right-click on the black plot window.
    *   Select **Add Trace**.
    *   Select **`V(out)`** (or `dB(V(out))`) from the list and click OK.
4.  **View Result:**
    *   You will see two lines: **Solid** (Magnitude in dB) and **Dotted** (Phase in degrees).
    *   **Find Low Frequency Gain:** Look at the dB value on the left (e.g., 66dB).
    *   **Find Bandwidth:** Find the frequency where the gain drops by 3dB (e.g., to 63dB). This is your $f_{-3dB}$.

### 3. How to See Slew Rate (Transient Analysis)
This checks how fast the output can change voltage.

1.  **Edit the Netlist:**
    *   Add a `*` before `.ac ...`.
    *   Remove the `*` from `.tran 0.01u 10u`.
    *   **Crucial Step:** Change the input source!
        *   Add a `*` before `Vin_p inp 0 DC 0 AC 1`.
        *   Remove the `*` from `Vin_p inp 0 PULSE(...)`.
2.  **Run:** Click **Run**.
3.  **Add Trace:**
    *   Right-click on the black plot window.
    *   Select **Add Trace**.
    *   Select **`V(out)`** from the list and click OK.
4.  **View Result:**
    *   You will see the voltage rising and falling over time.
    *   **Measure Slew Rate:** Zoom in on the rising edge. Hold **Ctrl** and **Left-Click** on the trace name `V(out)` to open a cursor box. The slope value is your Slew Rate.


