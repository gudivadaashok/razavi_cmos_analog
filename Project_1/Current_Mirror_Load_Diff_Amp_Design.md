# Project Assignment: Current-Mirror Load Differential Amplifier

## Design Specifications
Design a current-mirror load differential amplifier to satisfy the following specifications:

*   **Supply Voltage:** $V_{DD} = -V_{SS} = 2.5 V$
*   **Slew Rate (SR):** $\ge 10 V/\mu s$ for $C_L = 5 pF$
*   **Bandwidth ($f_{-3dB}$):** $\ge 100 kHz$ for $C_L = 5 pF$
*   **Small-Signal Differential Voltage Gain:** $200$
*   **Input Common-Mode Range (ICMR):** $-1.2 V \le ICMR \le 1.5 V$
*   **Power Dissipation ($P_{diss}$):** $\le 1 mW$

### Device Parameters
*   $\mu_n C_{ox} = 110 \mu A/V^2$, $\mu_p C_{ox} = 50 \mu A/V^2$
*   $V_{TN} = 0.5 V$, $V_{TP} = -0.5 V$
*   $\lambda_n = 0.05 V^{-1}$, $\lambda_p = 0.2 V^{-1}$
*   Channel Length ($L$) = $0.2 \mu m$

## Project Report Requirements
Your Project Report Should Include the following (in the same order):

1.  **Final design with currents through all devices and node voltages as calculated by SPICE**
2.  **SPICE DC Transfer Characteristic**
3.  **SPICE small-signal gain vs. frequency plot with $f_{-3dB}$ marked down**
4.  **Output voltage as a function of time with SR marked down**
5.  **Hand calculations**

### Hand Calculations
This section, addressed previously, must provide the theoretical foundation for the amplifier design and sizing, resulting in determined currents and device dimensions necessary to meet the specifications (gain, bandwidth, slew rate, ICMR, and power dissipation). Hand calculations should be presented in a logical, orderly, and neat manner. Formulas and equations must be centered, numbered, and explained in English, with all factors and symbols defined.

### SPICE Simulation Requirements
The subsequent four sections require verifying the design using a SPICE simulator (such as HSPICE, LTSpice, or TINA-TI). These simulations confirm the static bias points, linearity, frequency characteristics, and speed constraints.

#### 1. Final design with currents through all devices and node voltages as calculated by SPICE
This requirement verifies that the calculated DC operating point is correctly realized in the simulated circuit using the actual device models.
*   **Analysis Type:** DC Operating Point Analysis (`.op`).
*   **Purpose:** To compute the steady-state bias voltages and currents of all active components (transistors). This confirms that all transistors are correctly biased in the saturation region and that the design meets the power constraint ($P_{diss} \le 1 mW$).
*   **Procedure:** Run a DC operating point analysis (`.op`). The simulator output generates a text file listing all node voltages and branch currents.
*   **Output Requirement:** The report must include the resulting SPICE output (or a table derived from it) detailing the drain current ($I_D$) for every transistor ($M_1$ through $M_4$ and $M_{tail}$) and the DC voltage at critical nodes (e.g., input source node, output node, gates of current mirrors).

#### 2. SPICE DC Transfer Characteristic
This plot demonstrates the amplifier's input-output relationship, confirming the specified linearity and the operational range of the input stage.
*   **Analysis Type:** DC Sweep Analysis (`.dc`).
*   **Purpose:** To measure the output voltage as the differential input voltage is swept across its operational range. This confirms that the amplifier operates linearly within the specified Input Common-Mode Range (ICMR), defined as $-1.2 V \le ICMR \le 1.5 V$.
*   **Procedure:** Sweep one input voltage (e.g., $V_{in,1}$) while holding the other input voltage (e.g., $V_{in,2}$) constant at the input common-mode voltage ($V_{CM}$), or sweep the differential input voltage ($V_{in,diff}$).
*   **Output Requirement:** The report must include a plot showing the differential output voltage ($V_{out}$) versus the differential input voltage ($V_{in,diff}$).

#### 3. SPICE small-signal gain vs. frequency plot with $f_{-3dB}$ marked down
This plot confirms the amplifier's gain and bandwidth performance by measuring the frequency response.
*   **Analysis Type:** AC Analysis (`.ac`).
*   **Purpose:** To measure the open-loop gain ($A_{OL}$) versus frequency. This verifies the specified gain of 200 and the required bandwidth ($f_{-3dB} \ge 100 kHz$).
*   **Procedure:** Configure the simulation command (`.ac`) to sweep the frequency over a desired range (e.g., starting at 1 Hz up to a frequency beyond the expected unity-gain bandwidth). The input source must be set with an AC amplitude (e.g., 1 V).
    *   The open-loop gain analysis plots the magnitude (usually in dB) and phase versus frequency. The magnitude is typically plotted as $dB(V_{out}/V_{in})$.
*   **Output Requirement:** The report must include the magnitude plot (Gain in dB vs. Frequency in Hz) with the -3dB frequency ($f_{-3dB}$) marked. This frequency is where the gain drops to 0.707 times the low-frequency gain (or 3 dB down from the mid-band gain).

#### 4. Output voltage as a function of time with SR marked down
This simulation verifies the large-signal speed limitation of the amplifier, known as the slew rate.
*   **Analysis Type:** Transient Analysis (`.tran`).
*   **Purpose:** To determine how quickly the output voltage can change in response to a large input step, confirming the Slew Rate constraint ($SR \ge 10 V/\mu s$).
*   **Procedure:** Configure the amplifier (e.g., in a unity-gain configuration if necessary, or simply applying a step input to the differential input). Apply a large input step or square wave signal, ensuring the input signal's rise time is shorter than the amplifier's expected slew rate. Use the appropriate SPICE transient command (`.tran`).
    *   The Slew Rate (SR) is calculated by measuring the slope of the output signal's rising or falling edge during the non-linear large-signal transition: $SR = \Delta V_{out} / \Delta t$.
*   **Output Requirement:** The report must include the plot of output voltage ($V_{out}$) versus time, clearly showing a large-signal transition and specifying the resulting Slew Rate (SR).

---
# Current-Mirror Load Differential Amplifier Design

## 1. Design Specifications

*   **Supply Voltage:** $V_{DD} = 2.5 V$, $V_{SS} = -2.5 V$
*   **Slew Rate (SR):** $\ge 10 V/\mu s$ ($C_L = 5 pF$)
*   **Bandwidth ($f_{-3dB}$):** $\ge 100 kHz$ ($C_L = 5 pF$)
*   **Small-Signal Differential Voltage Gain ($A_v$):** $\ge 200$
*   **Input Common-Mode Range (ICMR):** $-1.2 V \le V_{in,CM} \le 1.5 V$
*   **Power Dissipation ($P_{diss}$):** $\le 1 mW$

**Device Parameters:**
*   NMOS: $\mu_n C_{ox} = 110 \mu A/V^2$, $V_{THN} = 0.5 V$, $\lambda_n = 0.05 V^{-1}$
*   PMOS: $\mu_p C_{ox} = 50 \mu A/V^2$, $V_{THP} = -0.5 V$, $\lambda_p = 0.2 V^{-1}$
*   Channel Length ($L$): $0.2 \mu m$

---

## 6. Hand Calculations

### A. Bias Current ($I_{SS}$)
The Slew Rate is determined by the tail current charging the load capacitor:
$$SR = \frac{I_{SS}}{C_L}$$
$$I_{SS} = SR \cdot C_L = 10 \frac{V}{\mu s} \cdot 5 pF = 10 \cdot 10^6 \cdot 5 \cdot 10^{-12} = 50 \mu A$$

To provide a design margin, we choose:
**$I_{SS} = 60 \mu A$**

**Power Check:**
$$P_{diss} = (V_{DD} - V_{SS}) \cdot I_{total} \approx 5 V \cdot 60 \mu A = 0.3 mW$$
This is well below the $1 mW$ limit.

### B. Transistor Sizing

#### 1. Input Pair ($M_1, M_2$)
The voltage gain is given by:
$$A_v = g_{m1} (r_{o2} || r_{o4})$$
where $r_{o2} = \frac{1}{\lambda_n I_{D1}}$ and $r_{o4} = \frac{1}{\lambda_p I_{D1}}$.
With $I_{D1} = I_{D2} = I_{SS}/2 = 30 \mu A$:
$$r_{o2} = \frac{1}{0.05 \cdot 30 \mu A} \approx 667 k\Omega$$
$$r_{o4} = \frac{1}{0.2 \cdot 30 \mu A} \approx 167 k\Omega$$
$$R_{out} = r_{o2} || r_{o4} = \frac{667 \cdot 167}{667 + 167} k\Omega \approx 133 k\Omega$$

Required $g_{m1}$:
$$g_{m1} = \frac{A_v}{R_{out}} = \frac{200}{133 k\Omega} \approx 1.5 mS$$

Calculate $(W/L)_1$:
$$g_{m1} = \sqrt{2 \mu_n C_{ox} (W/L)_1 I_{D1}}$$
$$(1.5 \cdot 10^{-3})^2 = 2 \cdot 110 \cdot 10^{-6} \cdot (W/L)_1 \cdot 30 \cdot 10^{-6}$$
$$2.25 \cdot 10^{-6} = 6.6 \cdot 10^{-9} (W/L)_1$$
$$(W/L)_1 \approx 341$$

With $L = 0.2 \mu m$:
$$W_1 = 341 \cdot 0.2 \mu m \approx 68.2 \mu m$$
**We choose $W_1 = W_2 = 68 \mu m$.**

*Check Overdrive Voltage:*
$$V_{OV1} = \frac{2 I_{D1}}{g_{m1}} = \frac{60 \mu A}{1.5 mS} = 0.04 V$$
(Note: This low $V_{OV}$ is required to achieve the high gain with the given channel length modulation parameters).

#### 2. Current Mirror Load ($M_3, M_4$)
To satisfy the upper ICMR limit ($1.5 V$):
$$V_{in,max} \le V_{DD} - V_{SG3} + V_{THN}$$
$$1.5 \le 2.5 - (|V_{THP}| + |V_{OV3}|) + 0.5$$
$$1.5 \le 3.0 - 0.5 - |V_{OV3}|$$
$$|V_{OV3}| \le 1.0 V$$

We choose a conservative $|V_{OV3}| = 0.2 V$ to ensure strong inversion and good matching.
$$I_{D3} = \frac{1}{2} \mu_p C_{ox} (W/L)_3 V_{OV3}^2$$
$$30 \mu A = \frac{1}{2} \cdot 50 \mu A/V^2 \cdot (W/L)_3 \cdot (0.2)^2$$
$$30 = 25 \cdot (W/L)_3 \cdot 0.04$$
$$(W/L)_3 = 30$$

With $L = 0.2 \mu m$:
$$W_3 = 30 \cdot 0.2 \mu m = 6 \mu m$$
**We choose $W_3 = W_4 = 6 \mu m$.**

#### 3. Tail Current Source ($M_5$)
To satisfy the lower ICMR limit ($-1.2 V$):
$$V_{in,min} \ge V_{SS} + V_{DS5,sat} + V_{GS1}$$
$$-1.2 \ge -2.5 + V_{OV5} + (V_{THN} + V_{OV1})$$
$$-1.2 \ge -2.5 + V_{OV5} + 0.5 + 0.04$$
$$-1.2 \ge -1.96 + V_{OV5}$$
$$V_{OV5} \le 0.76 V$$

We choose $V_{OV5} = 0.2 V$.
$$I_{SS} = \frac{1}{2} \mu_n C_{ox} (W/L)_5 V_{OV5}^2$$
$$60 \mu A = \frac{1}{2} \cdot 110 \mu A/V^2 \cdot (W/L)_5 \cdot (0.2)^2$$
$$60 = 2.2 \cdot (W/L)_5$$
$$(W/L)_5 \approx 27.3$$

With $L = 0.2 \mu m$:
$$W_5 = 27.3 \cdot 0.2 \mu m \approx 5.5 \mu m$$
**We choose $W_5 = 5.5 \mu m$.**

### C. Bandwidth Check
$$f_{-3dB} = \frac{1}{2 \pi R_{out} C_L} = \frac{1}{2 \pi \cdot 133 k\Omega \cdot 5 pF}$$
$$f_{-3dB} \approx 239 kHz$$
This satisfies the $\ge 100 kHz$ requirement.

---

## 2. Final Design Summary

### Device Dimensions and Calculated Currents
| Device | Width ($W$) | Length ($L$) | $I_D$ (Calc) |
| :--- | :--- | :--- | :--- |
| M1, M2 | $68 \mu m$ | $0.2 \mu m$ | $30 \mu A$ |
| M3, M4 | $6 \mu m$ | $0.2 \mu m$ | $30 \mu A$ |
| M5 | $5.5 \mu m$ | $0.2 \mu m$ | $60 \mu A$ |
| **Total** | | | **$60 \mu A$** |
| **Load** | $C_L = 5 pF$ | | |

### SPICE Simulation Results (Currents and Voltages)
*To be filled with data from the `.op` simulation.*

| Parameter | Simulated Value |
| :--- | :--- |
| $I_{D1}, I_{D2}$ | |
| $I_{D3}, I_{D4}$ | |
| $I_{D5}$ ($I_{SS}$) | |
| $V_{out}$ (DC) | |
| $V_{Source,1-2}$ | |
| $V_{Gate,5}$ | |

---

## 3, 4, 5. SPICE Simulation Plan

To verify the design, create a SPICE netlist with the following structure.

### SPICE Netlist (`diff_amp.sp`)

```spice
* Current-Mirror Load Differential Amplifier
* Technology Models (Level 1 for hand calc verification)
.model NMOS NMOS level=1 kp=110u vto=0.5 lambda=0.05
.model PMOS PMOS level=1 kp=50u vto=-0.5 lambda=0.2

* Supply Voltages
VDD vdd 0 2.5
VSS vss 0 -2.5

* Bias Voltage for M5 (Calculated for Vov=0.2V -> VGS=0.7V)
* V_gate_M5 = VSS + VGS5 = -2.5 + 0.7 = -1.8V
VBIAS vb 0 -1.8

* Input Sources
* Vicm is Common Mode, Vid is Differential
* Vin+ = Vicm + Vid/2
* Vin- = Vicm - Vid/2
Vicm vicm 0 0
Vid_ac vid 0 0 AC 1
Vp inp vicm 0
Vm inm vicm 0

* Circuit
* M1 D G S B
M1 2 inp 1 vss NMOS W=68u L=0.2u
M2 out inm 1 vss NMOS W=68u L=0.2u
M3 2 2 vdd vdd PMOS W=6u L=0.2u
M4 out 2 vdd vdd PMOS W=6u L=0.2u
M5 1 vb vss vss NMOS W=5.5u L=0.2u

* Load Capacitor
CL out 0 5p

* Analyses

* 1. Operating Point (For Currents and Node Voltages)
.op

* 2. DC Transfer Characteristic (Sweep Vid)
* Sweep Vid from -10mV to 10mV to see the transition
.dc Vp -0.01 0.01 0.0001

* 3. AC Analysis (Gain vs Frequency)
.ac dec 10 1 100meg

* 4. Transient Analysis (Slew Rate)
* Apply a pulse to Vin
* Pulse from -1V to 1V (Large signal)
* Redefine Vp for transient:
* Vp_pulse inp 0 PULSE(-0.5 0.5 1u 1n 1n 5u 10u)
* (Comment out the DC/AC sources above and uncomment pulse for Tran)
.tran 10n 10u

.end
```

### Expected Results

1.  **Operating Point:**
    *   Current in M5 should be approx $60 \mu A$.
    *   Current in M1, M2, M3, M4 should be approx $30 \mu A$.
    *   Output DC voltage should be close to 0V (if perfectly balanced, though systematic offset may exist).

2.  **DC Transfer Characteristic:**
    *   The slope of $V_{out}$ vs $V_{id}$ at $V_{id}=0$ is the DC Gain.
    *   Expected Slope $\approx 200 V/V$.

3.  **AC Analysis:**
    *   Low frequency magnitude should be $\approx 46 dB$ ($20 \log_{10} 200$).
    *   The -3dB frequency should be $\approx 240 kHz$.

4.  **Transient (Slew Rate):**
    *   Apply a large step input.
    *   Measure slope of $V_{out}$.
    *   Expected slope $\approx 12 V/\mu s$ ($60 \mu A / 5 pF$).


```
* =========================================================
* DEVICE MODEL DEFINITIONS 
* =========================================================
 
.model NMOS1 NMOS (LEVEL=1 VTO=0.5V KP=200u LAMBDA=0.2 L=0.2u W=4.7672u GAMMA=0)
.model NMOS5 NMOS (LEVEL=1 VTO=0.5V KP=200u LAMBDA=0.2 L=0.2u W=1u GAMMA=0)
 

.model PMOS3 PMOS (LEVEL=1 VTO=-0.5V KP=50u LAMBDA=0.2 L=0.2u W=0.2u GAMMA=0)
```