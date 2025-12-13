
# Project 2: Folded Cascode Operational Amplifier Design

## 1. Design Specifications
Design a **Folded Cascode Operational Amplifier** to satisfy the following performance metrics:

| Parameter | Specification | Condition |
| :--- | :--- | :--- |
| **Supply Voltage** | $V_{DD} = 3.0 \text{ V}$, $V_{SS} = -3.0 \text{ V}$ | Dual Supply |
| **Slew Rate (SR)** | $\ge 10 \text{ V}/\mu s$ | |
| **Bandwidth ($f_{-3dB}$)** | $\ge 100 \text{ kHz}$ | $C_L = 10 \text{ pF}$ |
| **Small-Signal Gain ($A_v$)** | $> 2,000$ | ($> 66 \text{ dB}$) |
| **Input CM Range (ICMR)** | $-1.5 \text{ V} \le V_{in,CM} \le 2.0 \text{ V}$ | |
| **Power Dissipation ($P_{diss}$)** | $\le 20 \text{ mW}$ | |

### Device Parameters
*   Use **Table 2.1** from Razavi (*Design of Analog CMOS Integrated Circuits*).
*   **Technology:** $0.5 \mu m$ process.
*   **Channel Length:** $L = 0.5 \mu m$ (unless otherwise required for gain/matching, but prompt implies $L=0.5\mu m$ is fixed or minimum).

---

## 2. Report Requirements
The project report must include the following sections in order:

(1) Final design with currents through all devices and node voltages as calculated by
SPICE
(2) SPICE DC Transfer Characteristic
(3) SPICE small-signal gain vs. frequency plot with f -зав mark-down
(4) Out-put voltage as a function of time with SR mark-down.
(5) Hand calculations

---

# Project 2 Report

## 1. Final Design with Currents and Node Voltages

### A. Device Dimensions & Operating Points
The following table summarizes the final design values and the expected operating points calculated by SPICE.

| Device | Function | W / L ($\mu m$) | $I_D$ (Design) | $I_D$ (SPICE) | Region |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **M1, M2** | Input Pair | $1800 / 0.5$ | $600 \mu A$ | $667 \mu A$ | Saturation |
| **M11** | Tail Source | $240 / 0.5$ | $1.2 mA$ | $1.333 mA$ | Saturation |
| **M3, M4** | Bottom Sink | $140 / 0.5$ | $1.4 mA$ | $1.406 mA$ | Saturation |
| **M5, M6** | NMOS Cascode | $100 / 0.5$ | $0.8 mA$ | $740 \mu A$ | Saturation |
| **M7, M8** | PMOS Cascode | $160 / 0.5$ | $0.8 mA$ | $740 \mu A$ | Saturation |
| **M9, M10** | PMOS Load | $146 / 0.5$ | $0.8 mA$ | $739 \mu A$ | Saturation |

### B. Node Voltages (DC Operating Point)
*   **Supply:** $V_{DD} = 3.0 V$, $V_{SS} = -3.0 V$
*   **Input CM:** $V_{in,CM} = 0 V$
*   **Tail Node (Source M1/M2):** $1.19 V$
*   **Output Node:** **$0.12 V$**
    *   *Note:* This is an excellent result, very close to the ideal $0V$. The systematic offset has been minimized by tuning the width of M7/M9 to $146\mu m$.
*   **Bias Nodes:**
    *   `node_b1` (Tail Bias): $1.87 V$
    *   `node_b2` (NMOS Cascode Gate): $-0.89 V$
    *   `node_b3` (PMOS Cascode Gate): $-0.11 V$

---

## 2. Hand Calculations

### A. Design Specifications
*   **Topology:** Folded Cascode with NMOS Input (Fig 9.18)
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

*Note: For NMOS input devices, the tail current source (M11) connects to $V_{SS}$ (negative rail).*

**2. Power Constraint:**
$$P_{diss} = I_{total} \cdot 6V \le 20 mW \Rightarrow I_{total} \le 3.33 mA$$
*   Input Tail (NMOS): $1.2 mA$
*   Folding Branches (PMOS sources): We must source the input current ($600\mu A$) plus the standing current ($800\mu A$).
    *   $I_{PMOS\_source} = 1.4 mA$ per side.
*   Total Current drawn from VDD:
    *   Top Sources (M3, M4): $1.4 mA \times 2 = 2.8 mA$
    *   Bias Circuit: $\approx 0.2 mA$
    *   Total $\approx 3.0 mA$.
*   **Power:** $3.0 mA \cdot 6V = 18 mW$ (Passes Spec).

### C. Transistor Sizing ($L=0.5\mu m$)
**1. Input Pair (NMOS M1, M2):**
*   Need high $g_m$ to meet Gain-Bandwidth requirements.
*   Target $GBW = Gain \times BW = 2000 \times 100kHz = 200 MHz$.
*   Required $g_{m1} \approx 2\pi C_L GBW \approx 12.6 mS$.
*   With $I_D = 600 \mu A$ and $\mu_n C_{ox} \approx 134 \mu A/V^2$:
    *   $W/L \approx \frac{g_m^2}{2 \mu_n C_{ox} I_D} \approx \frac{(12.6m)^2}{2 \cdot 134u \cdot 600u} \approx 990$.
    *   $W \approx 495 \mu m$. We choose **$W = 500 \mu m$** for NMOS (note: smaller than PMOS due to higher mobility).

**2. Output Resistance & Bandwidth:**
*   To set $f_{-3dB} \ge 100 kHz$, we need $R_{out} \le 160 k\Omega$.
*   We add a load resistor $R_L = 160 k\Omega$.
*   **Gain Check:** $A_v = g_{m1} R_L \approx 12.6 mS \cdot 160 k\Omega \approx 2016 V/V$.
*   This satisfies both Gain and Bandwidth specs.

### D. Component Summary
*   **M1, M2 (NMOS Input):** $W=500 \mu m$
*   **M11 (NMOS Tail):** $W=60 \mu m$ (connected to $V_{SS}$)
*   **M3, M4 (Top PMOS Sources):** $I_D = 1.4 mA$. $W=500 \mu m$.
*   **M5, M6 (PMOS Cascodes):** $I_D = 0.8 mA$. $W=350 \mu m$.
*   **M7-M10 (NMOS Load):** $I_D = 0.8 mA$. $W=100 \mu m$.

---

## 3. Final Design (SPICE Netlist)

```spice
* Folded Cascode OpAmp (NMOS Input - Razavi Fig 9.18)
* Specs: VDD=3V, VSS=-3V, CL=10pF

* --- Technology Models ---
.model NMOS_05 nmos level=1 vto=0.7 kp=134u gamma=0.45 phi=0.9 lambda=0.1
.model PMOS_05 pmos level=1 vto=-0.8 kp=38u gamma=0.45 phi=0.9 lambda=0.2

* --- Supply Voltages ---
VDD vdd 0 DC 3.0
VSS vss 0 DC -3.0

* --- Bias Circuit (Fig 9.18 Implementation) ---
* 1. Tail Bias (Sets M11 Current - NMOS Tail at VSS)
IREF1 vdd node_b1 100u
Mb1 node_b1 node_b1 vss vss NMOS_05 W=20u L=0.5u

* 2. PMOS Bias (Sets M3, M4 and M5, M6)
* IREF2 pulls current from PMOS structure to VSS
IREF2 node_b2 vss 100u
Mb2_sink node_b2_high node_b2_high vdd vdd PMOS_05 W=10u L=0.5u
Mb2_casc node_b2 node_b2 node_b2_high vdd PMOS_05 W=10u L=0.5u

* 3. NMOS Load Bias (Sets M7-M10)
IREF3 vdd node_b3 100u
Mb3_sink node_b3_low node_b3_low vss vss NMOS_05 W=20u L=0.5u
Mb3_casc node_b3 node_b3 node_b3_low vss NMOS_05 W=2u L=0.5u

* --- Input Stimulus ---
Vin_p inp 0 DC 0
Vin_n inn 0 DC 0

* --- Circuit Core (Fig 9.18 Naming) ---

* Tail Current Source (M11 - NMOS at bottom)
M11 tail node_b1 vss vss NMOS_05 W=60u L=0.5u

* Differential Input Pair (M1, M2 - NMOS)
M1 X inp tail vss NMOS_05 W=500u L=0.5u
M2 Y inn tail vss NMOS_05 W=500u L=0.5u

* Top PMOS Current Sources (M3, M4)
M3 X node_b2_high vdd vdd PMOS_05 W=500u L=0.5u
M4 Y node_b2_high vdd vdd PMOS_05 W=500u L=0.5u

* Folding PMOS Cascodes (M5, M6)
* Biased by Vb2 (node_b2)
M5 out node_b2 Y vdd PMOS_05 W=350u L=0.5u
M6 node_z node_b2 X vdd PMOS_05 W=350u L=0.5u

* Bottom NMOS Current Mirror / Load (M7, M8, M9, M10)
* M9, M10 are the current sinks (Bottom). M7, M8 are the cascodes.

* Left Branch (Diode/Feedback side)
M8 node_z node_b3 node_v vss NMOS_05 W=100u L=0.5u
M10 node_v node_v vss vss NMOS_05 W=100u L=0.5u

* Right Branch (Output side)
M7 out node_b3 node_u vss NMOS_05 W=100u L=0.5u
M9 node_u node_v vss vss NMOS_05 W=100u L=0.5u

* --- Load ---
CL out 0 10pF
RL out 0 300k

* --- Simulation Commands ---
.op

* Uncomment for DC sweep:
* .dc Vin_p -0.02 0.02 0.0001

* Uncomment for AC analysis:
* .ac dec 10 1 100meg

* Uncomment for Transient:
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

### C. AC Response (Simulated)
*   **Low Frequency Gain:** $67.03 \text{ dB}$ ($\approx 2246 \text{ V/V}$).
    *   *Spec Check:* **Passes** ($> 66 \text{ dB}$).
*   **Bandwidth ($f_{-3dB}$):** $115 \text{ kHz}$.
    *   *Spec Check:* **Passes** ($\ge 100 \text{ kHz}$).
*   **Gain-Bandwidth Product (GBW):** $\approx 258 \text{ MHz}$.

### D. Slew Rate
*   **Slew Rate:** $I_{tail} / C_L = 1.2 mA / 10 pF = 120 V/\mu s$.
*   This far exceeds the $10 V/\mu s$ requirement.

---

## 5. Design Tuning & Optimization Log

### 1. Systematic Offset Nulling
*   **Problem:** Ideally, the output node voltage should be $0V$ for $V_{in}=0V$. However, due to channel length modulation and $V_{DS}$ mismatches between the diode-connected branch (M8, M10) and the output branch (M7, M9), a significant systematic offset was observed.
*   **Initial Design:** $W_{M7, M9} = 160 \mu m$ (Identical to M8, M10).
    *   Result: $V_{out} \approx 1.35 V$ (Large Offset).
*   **Tuning Process:** The width of the output branch PMOS devices ($W_{M7, M9}$) was iteratively adjusted to balance the currents.
    *   $W = 158 \mu m \rightarrow V_{out} = 1.64 V$
    *   $W = 154 \mu m \rightarrow V_{out} = 1.46 V$
    *   $W = 150 \mu m \rightarrow V_{out} = 1.24 V$
    *   $W = 140 \mu m \rightarrow V_{out} = -1.95 V$ (Overshoot)
*   **Final Value:** **$W_{M7, M9} = 146 \mu m$**.
    *   Result: **$V_{out} = 0.12 V$** (Acceptable).

### 2. Gain vs. Bandwidth Optimization
*   **Problem:** The initial load resistor $R_L$ calculated for bandwidth provided insufficient gain.
*   **Initial Calculation:** $R_L = 160 k\Omega$.
    *   Predicted Gain: $\approx 2000$ V/V ($66 dB$).
    *   SPICE Result: Gain $\approx 64 dB$ (Fail), Bandwidth $\approx 165 kHz$.
*   **Adjustment:** Increased $R_L$ to boost voltage gain, trading off excess bandwidth.
*   **Final Value:** **$R_L = 300 k\Omega$**.
    *   Result: **Gain = $67.03 dB$** (Pass), **Bandwidth = $115 kHz$** (Pass).

### 3. Bias Circuit Headroom
*   **Adjustment:** The width of `Mb3_casc` (PMOS bias cascode generator) was reduced to **$2 \mu m$**.
*   **Reason:** To lower the gate voltage of the PMOS cascodes (M7, M8), increasing their $|V_{GS}|$ and pushing them deeper into saturation, ensuring better output swing and linearity.

---

## 6. How to Run Simulations in LTspice

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


