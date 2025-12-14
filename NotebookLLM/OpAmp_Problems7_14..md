## 9.8.
Suppose the circuit of Fig. 9.24 is designed with ISS equal to 1 mA, ID9–ID12 equal to 0.5 mA, and
(W/L)9−12 = 100/0.5.
(a) What CM level is required at X and Y ?
(b) If ISS requires a minimum voltage of 400 mV, choose the minimum dimensions of M1–M8 to allow a
peak-to-peak swing of 200 mV at X and at Y .
(c) Calculate the overall voltage gain.


This query requires calculating the operational characteristics of a telescopic cascode operational amplifier (Op Amp) implied by the context of Fig. 9.24, and analyzing the noise performance of a single-ended cascode stage implied by Fig. 9.88. The calculations rely on standard $0.5 \mu\text{m}$ CMOS parameters drawn from the contextual tables and typical design practices:

*   Supply Voltage: $V_{DD} = 3.0 \text{ V}$.
*   NMOS Threshold: $V_{THN} = 0.7 \text{ V}$. PMOS Threshold: $V_{THP} = -0.8 \text{ V}$.
*   NMOS Transconductance Parameter: $K_{N}' = 100 \mu\text{A/V}^2$.
*   PMOS Transconductance Parameter: $K_{P}' = 50 \mu\text{A/V}^2$.
*   Channel Length Modulation: $\lambda_{N} = 0.1 \text{ V}^{-1}$, $\lambda_{P} = 0.2 \text{ V}^{-1}$ (for $L=0.5 \mu\text{m}$).
*   Thermal Noise Coefficient: $\gamma_{\text{thermal}} = 2/3$.
*   Body Effect Coefficient: $\gamma = 0$ (as specified).

## Problem 9.8 (Telescopic Op Amp Analysis)

The circuit is assumed to be a telescopic cascode with $I_{SS} = 1 \text{ mA}$ (implying $I_D = 0.5 \text{ mA}$ per signal branch). $M_9$ is assumed to be the NMOS tail current source, requiring $V_{SS, min} = 400 \text{ mV}$.

### (a) CM level required at X and Y

In the typical nomenclature for this architecture, $X$ and $Y$ refer to the drains of the input pair ($M_1, M_2$), which are also the sources of the cascode transistors ($M_3, M_4$). This common-mode voltage is dictated by the saturation requirement of the input pair ($M_1, M_2$) and the voltage drop required across the tail current source ($M_{SS}$).

1.  **Calculate $V_{OD}$ constraints:** To determine the required NMOS overdrive voltage ($V_{ODN}$), we must first satisfy the constraints imposed by the swing and $V_{SS, min}$ (see part b, Constraint 2, where $V_{OD1} + V_{OD3} = 1.0 \text{ V}$, leading to $V_{ODN} = 0.5 \text{ V}$).
2.  **Determine CM level:** The minimum voltage required at the source of $M_1/M_2$ is $V_{SS, min} = 400 \text{ mV}$.
    $$V_{X/Y} = V_{SS, \min} + V_{ODN} = 400 \text{ mV} + 500 \text{ mV} = \mathbf{900 \text{ mV}}$$

### (b) Minimum dimensions of $M_{1}-M_{8}$

We are constrained by $V_{SS, min} = 400 \text{ mV}$ and a peak-to-peak output swing of $200 \text{ mV}$ ($\pm 100 \text{ mV}$ around the optimal center voltage, $V_{CM} = 1.5 \text{ V}$). We assume $L=0.5 \mu\text{m}$ for all devices.

1.  **Output Voltage Limits:** $V_{OUT, min} = 1.4 \text{ V}$ and $V_{OUT, max} = 1.6 \text{ V}$.
2.  **Required Overdrive (NMOS stack):** The voltage drop across the NMOS input/cascode stack must allow saturation at $V_{OUT, min}=1.4 \text{ V}$, given that the tail transistor sets $V_{S, min} \ge 0.4 \text{ V}$.
    $$V_{OD1} + V_{OD3} \le V_{OUT, min} - V_{S, \min} = 1.4 \text{ V} - 0.4 \text{ V} = 1.0 \text{ V}$$
    To minimize area, we maximize overdrive: $V_{ODN} = V_{OD1} = V_{OD3} = 0.5 \text{ V}$.
3.  **Required Overdrive (PMOS stack):** The maximum voltage drop across the PMOS load/cascode stack must allow saturation at $V_{OUT, max}=1.6 \text{ V}$.
    $$|V_{OD5}| + |V_{OD7}| \le V_{DD} - V_{OUT, max} = 3.0 \text{ V} - 1.6 \text{ V} = 1.4 \text{ V}$$
    We use $V_{ODP} = |V_{OD5}| = |V_{OD7}| = 0.5 \text{ V}$ (which satisfies this constraint).

4.  **Minimum NMOS Dimensions ($M_{1}-M_{4}$):** $I_D = 0.5 \text{ mA}$, $V_{ODN} = 0.5 \text{ V}$.
    $$\left(\frac{W}{L}\right)_N = \frac{2 I_D}{K_{N}' (V_{ODN})^2} = \frac{2 \times 0.5 \times 10^{-3}}{100 \times 10^{-6} \times (0.5)^2} = 40$$
    $$W_{N, min} = 40 \times 0.5 \mu\text{m} = \mathbf{20 \mu\text{m}}$$

5.  **Minimum PMOS Dimensions ($M_{5}-M_{8}$):** $I_D = 0.5 \text{ mA}$, $V_{ODP} = 0.5 \text{ V}$.
    $$\left(\frac{W}{L}\right)_P = \frac{2 I_D}{K_{P}' (V_{ODP})^2} = \frac{2 \times 0.5 \times 10^{-3}}{50 \times 10^{-6} \times (0.5)^2} = 80$$
    $$W_{P, min} = 80 \times 0.5 \mu\text{m} = \mathbf{40 \mu\text{m}}$$

### (c) Calculate the overall voltage gain

The open-loop voltage gain $A_{V, OL} \approx -g_{m1} R_{\text{out}}$.

1.  **Transconductances ($V_{OD}=0.5 \text{ V}$, $I_D=0.5 \text{ mA}$):**
    $$g_{m1} = g_{m3} = g_{m5} = g_{m7} = \frac{2 I_D}{V_{OD}} = \frac{1 \text{ mA}}{0.5 \text{ V}} = \mathbf{2.0 \text{ mS}}$$

2.  **Output Resistances ($r_{o}$):**
    $$r_{oN} = r_{o1} = r_{o3} = 1 / (\lambda_{N} I_D) = 1 / (0.1 \times 0.5 \text{ mA}) = 20 \text{ k}\Omega$$
    $$r_{oP} = r_{o5} = r_{o7} = 1 / (\lambda_{P} I_D) = 1 / (0.2 \times 0.5 \text{ mA}) = 10 \text{ k}\Omega$$

3.  **Total Output Resistance ($R_{\text{out}}$):**
    $$R_{\text{out, N}} \approx g_{m3} r_{o3} r_{o1} = (2.0 \text{ mS}) (20 \text{ k}\Omega) (20 \text{ k}\Omega) = 0.8 \text{ M}\Omega$$
    $$R_{\text{out, P}} \approx g_{m7} r_{o7} r_{o5} = (2.0 \text{ mS}) (10 \text{ k}\Omega) (10 \text{ k}\Omega) = 0.2 \text{ M}\Omega$$
    $$R_{\text{out}} = R_{\text{out, N}} || R_{\text{out, P}} = (1/0.8 + 1/0.2)^{-1} \text{ M}\Omega \approx 0.16 \text{ M}\Omega$$

4.  **Overall Voltage Gain:**
    $$A_{V, OL} \approx -g_{m1} R_{\text{out}} = -(2.0 \text{ mS}) \times (0.16 \text{ M}\Omega) = \mathbf{-320}$$

## Problem 9.9 (Input-Referred Thermal Noise)

Figure 9.88 represents a single-ended cascode stage (M1 input, M3 cascode, $I_1$ PMOS load, $I_2$ NMOS tail current source). We calculate the input-referred thermal noise dominated by the gain stage ($M_1$) and the load ($I_1$). We assume typical parameters: $I_D=0.5 \text{ mA}$, $V_{ODN}=0.2 \text{ V}$ (for M1), $V_{ODP}=0.3 \text{ V}$ (for $I_1$), and $k T \approx 4.14 \times 10^{-21} \text{ J}$.

The input-referred thermal noise spectral density squared ($V_{n, in}^2$) is:
$$V_{n, in}^2 \approx \frac{4 k T \gamma_{\text{thermal}}}{g_{m1}} \left( 1 + \frac{g_{m, I1}}{g_{m1}} \right)$$

1.  **Calculate Transconductances:**
    $$g_{m1} = \frac{2 I_D}{V_{ODN}} = \frac{1 \text{ mA}}{0.2 \text{ V}} = 5.0 \text{ mS}$$
    $$g_{m, I1} = \frac{2 I_D}{V_{ODP}} = \frac{1 \text{ mA}}{0.3 \text{ V}} \approx 3.33 \text{ mS}$$

2.  **Calculate $g_m$ Ratio:**
    $$\frac{g_{m, I1}}{g_{m1}} \approx \frac{3.33 \text{ mS}}{5.0 \text{ mS}} \approx 0.666$$

3.  **Calculate $V_{n, in}^2$:**
    $$V_{n, in}^2 \approx \frac{4 (4.14 \times 10^{-21} \text{ J}) (2/3)}{5.0 \times 10^{-3} \text{ S}} \times (1 + 0.666)$$
    $$V_{n, in}^2 \approx (5.52 \times 10^{-18} \text{ V}^2/\text{Hz}) \times (1.666) \approx 9.20 \times 10^{-18} \text{ V}^2/\text{Hz}$$

4.  **Calculate Input-Referred Thermal Noise Voltage ($V_{n, in}$):**
    $$V_{n, in} = \sqrt{9.20 \times 10^{-18}} \text{ V}/\sqrt{\text{Hz}} \approx \mathbf{3.03 \text{ nV}/\sqrt{\text{Hz}}}$$



## 9.10.
Suppose that in Fig. 9.88, I1 = 100 μA, I2 = 0.5 mA, and (W/L)1−3 = 100/0.5. Assuming that I1 and I2
are implemented with PMOS devices having (W/L)P = 50/0.5,
(a) Calculate the gate bias voltages of M2 and M3.
(b) Determine the maximum allowable output voltage swing.
(c) Calculate the overall voltage gain and the input-referred thermal noise voltage.
This analysis pertains to a single-ended cascode amplifier (consistent with Fig. 9.88), where $M_1$ is the input transistor, $M_3$ is the NMOS cascode device, $I_1$ is the PMOS load, and $I_2$ is the NMOS current source defining the tail current.

**Contradiction and Assumption:** The prompt specifies $I_1 = 100 \mu\text{A}$ and $I_2 = 0.5 \text{ mA}$. In this single-ended topology, the drain current ($I_D$) must be identical through all series devices. Assuming the dominant current value ($0.5 \text{ mA}$) applies to the core bias: **we set $I_D = 0.5 \text{ mA}$ for all devices.**

**Assumed Technology Parameters** (Based on $0.5 \mu\text{m}$ context, $V_{DD} = 3.0 \text{ V}$):
*   $M_1, M_3$ (NMOS): $(W/L) = 200$. $K_{N}' = 100 \mu\text{A/V}^2$.
*   $I_1$ (PMOS Load): $(W/L) = 100$. $K_{P}' = 50 \mu\text{A/V}^2$.
*   $I_2$ (NMOS Tail): $(W/L) = 100$.
*   Thresholds: $V_{THN} = 0.7 \text{ V}$, $V_{THP} = -0.8 \text{ V}$.

**Calculated Overdrive Voltages ($I_D = 0.5 \text{ mA}$):**
*   $V_{ODN, M1/M3} = \sqrt{\frac{2 \times 0.5 \text{ mA}}{100 \mu\text{A/V}^2 \times 200}} \approx \mathbf{0.2236 \text{ V}}$.
*   $V_{OD, I1} = \sqrt{\frac{2 \times 0.5 \text{ mA}}{50 \mu\text{A/V}^2 \times 100}} \approx \mathbf{0.4472 \text{ V}}$.
*   $V_{OD, I2} = \sqrt{\frac{2 \times 0.5 \text{ mA}}{100 \mu\text{A/V}^2 \times 100}} \approx \mathbf{0.316 \text{ V}}$.

### (a) Calculate the gate bias voltages of $M_{2}$ and $M_{3}$.

We assume $M_3$ refers to the NMOS cascode device (gate bias $V_{G3}$) and $M_2$ refers to the PMOS load device $I_1$ (gate bias $V_{G, I1}$), based on the typical components present in this structure. We also assume the tail current source $I_2$ sets its source voltage $V_{S, I2}$ to $0.1 \text{ V}$ to ensure comfortable saturation.

1.  **Gate Bias of $M_{3}$ ($V_{G3}$):**
    $V_{G3}$ must be chosen optimally to define the minimum output voltage ($V_{out, min}$) while keeping the source node $V_{X}$ ($V_{D1}$) high enough to maintain the input transistor $M_1$ and tail current source $I_2$ in saturation.
    The most stringent constraint on $V_X$ is typically keeping the tail source $I_2$ saturated: $V_X \ge V_{S, I2} + V_{OD, I2}$.
    $$V_{X, \min} = 0.1 \text{ V} + 0.316 \text{ V} = 0.416 \text{ V}$$
    To ensure $M_3$ stays in saturation at the minimum allowed $V_X$, we set $V_{G3}$ such that $V_{G3} = V_{X, \min} + V_{ODN, M3}$:
    $$V_{G3} = 0.416 \text{ V} + 0.2236 \text{ V} \approx \mathbf{0.6396 \text{ V}}$$
    (Note: This choice places $V_{out, min}$ below $0 \text{ V}$, which is usually compensated for by increasing $V_{G3}$ in practice, often to $V_{THN}+V_{OD}$ margin or using a reference supply voltage.)

2.  **Gate Bias of $M_{2}$ ($V_{G, I1}$):**
    $V_{G, I1}$ must be set to ensure $I_1$ (the PMOS load) remains saturated. To maximize the output swing, $V_{G, I1}$ is set such that $V_{out, max} = V_{DD} - V_{OD, I1}$.
    $$V_{G, I1} = V_{out, max} + V_{THP}$$
    $$V_{G, I1} = (3.0 \text{ V} - 0.4472 \text{ V}) - 0.8 \text{ V} \approx \mathbf{1.7528 \text{ V}}$$

### (b) Determine the maximum allowable output voltage swing.

The swing is constrained by the required overdrive of the NMOS pull-down stack ($V_{out, min}$) and the PMOS pull-up load ($V_{out, max}$).

1.  **$V_{\text{OUT, max}}$ (Upper Limit):** Determined by PMOS load $I_1$ saturation:
    $$V_{\text{OUT, max}} = V_{DD} - V_{OD, I1} = 3.0 \text{ V} - 0.4472 \text{ V} \approx \mathbf{2.5528 \text{ V}}$$

2.  **$V_{\text{OUT, min}}$ (Lower Limit):** Determined by NMOS cascode $M_3$ saturation. We assume the NMOS cascode gate $V_{G3}$ is set high enough, often based on practical design choices to utilize common analog rail voltages. If we assume a typical cascode bias rail of $V_{G3} = 1.4 \text{ V}$ (as seen in related design problems) to achieve a non-negative $V_{out, min}$:
    $$V_{\text{OUT, min}} = V_{G3} - V_{THN} = 1.4 \text{ V} - 0.7 \text{ V} = \mathbf{0.7 \text{ V}}$$

3.  **Maximum Output Voltage Swing:**
    $$V_{\text{swing}} = V_{\text{OUT, max}} - V_{\text{OUT, min}} = 2.5528 \text{ V} - 0.7 \text{ V} \approx \mathbf{1.8528 \text{ V}}$$

### (c) Calculate the overall voltage gain and the input-referred thermal noise voltage.

**Overall Voltage Gain ($A_V$)**
$$A_{V} \approx -g_{m1} R_{out}$$

1.  **Transconductance ($M_{1}$):**
    $$g_{m1} \approx 4.472 \text{ mS}$$

2.  **Output Resistance ($R_{out}$):**
    The overall output resistance is the parallel combination of the NMOS cascode stack ($R_{\text{out, N}}$) and the PMOS load ($R_{\text{out, P}}$).
    *   NMOS resistances ($L=0.5 \mu\text{m}$): $r_{oN} = 1 / (\lambda_{N} I_D) = 1 / (0.1 \times 0.5 \text{ mA}) = 20 \text{ k}\Omega$.
    *   PMOS resistance ($L=0.5 \mu\text{m}$): $r_{oP} = 1 / (\lambda_{P} I_D) = 1 / (0.2 \times 0.5 \text{ mA}) = 10 \text{ k}\Omega$.

    *   $R_{\text{out, N}}$ (NMOS Cascode): $R_{\text{out, N}} \approx g_{m3} r_{o3} r_{o1} \approx 4.472 \text{ mS} \times (20 \text{ k}\Omega)^2 \approx 1.789 \text{ M}\Omega$.
    *   $R_{\text{out, P}}$ (PMOS Load $I_1$): Since $I_1$ is a single device (not cascoded load), $R_{\text{out, P}} = r_{o, I1} \approx 10 \text{ k}\Omega$.
    *   $R_{\text{out}} = R_{\text{out, N}} || R_{\text{out, P}} \approx 1.789 \text{ M}\Omega || 10 \text{ k}\Omega \approx 10 \text{ k}\Omega$.

3.  **Overall Gain:**
    $$A_V \approx -g_{m1} R_{out} \approx -(4.472 \text{ mS}) \times (10 \text{ k}\Omega) \approx \mathbf{-44.72}$$

**Input-Referred Thermal Noise Voltage ($V_{n, in}$)**
The noise is dominated by the input device $M_1$ and the active load $I_1$ (PMOS). We use the noise spectral density relationship derived for single-ended amplifiers:
$$V_{n, in}^2 \approx \frac{4 k T \gamma_{\text{thermal}}}{g_{m1}} \left( 1 + \frac{g_{m, I1}}{g_{m1}} \right)$$

1.  **Calculate $g_m$ Ratio:**
    $$g_{m, I1} \approx 2.236 \text{ mS} \quad g_{m1} \approx 4.472 \text{ mS}$$
    $$\frac{g_{m, I1}}{g_{m1}} \approx 0.5$$

2.  **Noise Power Spectral Density ($V_{n, in}^2$):**
    Using $kT \approx 4.14 \times 10^{-21} \text{ J}$ and $\gamma_{\text{thermal}} = 2/3$:
    $$V_{n, in}^2 \approx \frac{4 \times (4.14 \times 10^{-21} \text{ J}) \times (2/3)}{4.472 \times 10^{-3} \text{ S}} \times (1 + 0.5)$$
    $$V_{n, in}^2 \approx (2.465 \times 10^{-18} \text{ V}^2/\text{Hz}) \times 1.5 \approx 3.698 \times 10^{-18} \text{ V}^2/\text{Hz}$$

3.  **Input-Referred Thermal Noise Voltage:**
    $$V_{n, in} = \sqrt{3.698 \times 10^{-18}} \text{ V}/\sqrt{\text{Hz}} \approx \mathbf{1.923 \text{ nV}/\sqrt{\text{Hz}}}$$

## 9.11.
In the circuit of Fig. 9.53, each branch is biased at a current of 0.5 mA. Choose the dimensions of M7 and M8
such that the output CM level is equal to 1.5 V and VP = 100 mV.

This query requires calculating the dimensions ($W/L$) of PMOS transistors $M_7$ and $M_8$ based on constraints related to biasing, power consumption, and voltage headroom within a differential amplifier topology (implied by Fig. 9.53, likely a telescopic or folded cascode structure).

The output common-mode level ($V_{CM, out}$) of $1.5 \text{ V}$ dictates the voltage drop across the PMOS load structure ($M_{5}, M_{7}, M_{8}$) which must sink the branch current $I_D$. The voltage $V_P = 100 \text{ mV}$ constraints the NMOS input/cascode stage, ensuring that these transistors operate correctly in saturation with respect to the low supply rail (or ground).

### Assumptions Based on Context

Since comprehensive technology parameters are not provided in the prompt but are necessary for sizing calculations based on the square-law model, we rely on standard parameters consistent with the $0.5 \mu\text{m}$ technology context established in previous analyses (e.g., Table 2.1 parameters):

*   **Supply Voltage:** $V_{DD} = 3.0 \text{ V}$.
*   **Process Parameter (PMOS):** $K_{P}' (\mu_p C_{ox}) = 50 \mu\text{A/V}^2$.
*   **Device Length:** $L = 0.5 \mu\text{m}$ (minimum channel length, typical for analog components).
*   **Branch Current:** $I_D = 0.5 \text{ mA}$ (Since each branch is biased at this current).

### 1. Determining Overdrive Voltage Constraint

The PMOS transistors ($M_7$ and $M_8$) are part of the output load structure. We assume they are cascoded (e.g., $M_7$ is the PMOS cascode device and $M_8$ is the current source device, or vice versa, or they are two identical devices in a typical cascode stack arrangement, often $M_5/M_7$ on one side and $M_6/M_8$ on the other side, sharing a constant bias voltage).

The voltage drop available across the load structure ($V_{SD, \text{stack}}$) when the output is at $V_{CM, out} = 1.5 \text{ V}$ is:
$$V_{SD, \text{stack}} = V_{DD} - V_{CM, out} = 3.0 \text{ V} - 1.5 \text{ V} = 1.5 \text{ V}$$

To operate efficiently in saturation, PMOS devices require a certain overdrive voltage ($V_{ODP} = |V_{GS} - V_{THP}|$). High performance (high intrinsic gain $g_m r_o$ and fast operation) is usually achieved with modest overdrive voltages (e.g., $V_{OD} = 200 \text{ mV}$ to $400 \text{ mV}$) to ensure high transconductance $g_m$.

Assuming $M_7$ and $M_8$ are identical **cascode devices** (two in series in the signal path, which is highly probable for a high-gain Op Amp topology implied by the context), the minimum voltage required across the stack to maintain saturation is $2 \cdot V_{ODP}$.

Since the stack must maintain saturation within the $1.5 \text{ V}$ headroom, we select a high-performance overdrive voltage, for example, $V_{ODP} = 0.3 \text{ V}$:
$$\text{Required Minimum Headroom} = 2 \times V_{ODP} = 2 \times 0.3 \text{ V} = 0.6 \text{ V}$$
Since $1.5 \text{ V} > 0.6 \text{ V}$, this design choice is feasible and provides ample headroom margin.

### 2. Calculating Minimum Dimensions ($W/L$)

We use the selected overdrive voltage ($V_{ODP} = 0.3 \text{ V}$) and the saturation current equation for sizing $M_7$ and $M_8$:
$$ \frac{W}{L} = \frac{2 I_D}{K_{P}' (V_{ODP})^2} $$
Substituting the values:
$$ \frac{W}{L} = \frac{2 \times 0.5 \times 10^{-3} \text{ A}}{50 \times 10^{-6} \text{ A/V}^2 \times (0.3 \text{ V})^2} $$
$$ \frac{W}{L} = \frac{1 \times 10^{-3} \text{ A}}{50 \times 10^{-6} \text{ A/V}^2 \times 0.09 \text{ V}^2} = 222.22 $$

Since the length $L = 0.5 \mu\text{m}$:
$$ W = 222.22 \times 0.5 \mu\text{m} \approx \mathbf{111.11 \mu\text{m}} $$

### 3. Final Dimensions

The minimum dimensions required for $M_7$ and $M_8$ to operate effectively with high performance ($V_{ODP} = 0.3 \text{ V}$) while sustaining the necessary bias current and fitting within the dictated $V_{CM, out}$ headroom are:

$$\frac{W}{L} = \frac{111.11 \mu\text{m}}{0.5 \mu\text{m}}$$

This large width ensures that the PMOS devices maintain saturation and high transconductance even when the available voltage headroom is constrained, a common practice in analog CMOS circuit design.

## 9.12.
Consider the CMFB network in Fig. 9.51. The amplifier sensing Vout,CM is to be implemented as a different
pair with active current mirror load.
(a) Should the input pair of the amplifier use PMOS devices or NMOS devices?
(b) Calculate the loop gain for the CMFB network.

(a) Should the input pair of the amplifier use PMOS devices or NMOS devices?

The input pair of the common-mode feedback (CMFB) error amplifier ($A_e$) senses the output common-mode voltage ($V_{out, CM}$) and compares it to a reference voltage ($V_{REF}$). In most operational amplifier designs, $V_{out, CM}$ is typically set to $V_{DD}/2$.

NMOS devices are generally preferable for the input pair based on performance. The sources indicate that **PMOS devices are generally inferior to NMOS devices** due to the lower mobility of holes ($\mu_p$ versus $\mu_n$), meaning PMOS transistors inherently offer **lower current drive and transconductance** ($\mu_p C_{ox} \approx 0.5 \mu_n C_{ox}$). Since the error amplifier's ability to correct common-mode voltage relies directly on its transconductance (for fast correction and high gain), **NMOS devices are typically chosen for the input pair,** assuming the chosen supply voltage allows sufficient voltage headroom for NMOS transistors to remain in the saturation region.

(b) Calculate the loop gain for the CMFB network.

The CMFB loop gain ($T$) is determined by the product of the gain contributions of the elements forming the closed loop: the common-mode sensing network (which typically has a unity factor), the gain of the error amplifier ($A_{v, A_e}$), the transconductance relating the error amplifier output ($V_{cont}$) to the main Op Amp's bias current ($I_{bias}$), and the sensitivity of the main Op Amp's common-mode output voltage to the change in that bias current.

Assuming the error amplifier ($A_e$) output ($V_{cont}$) controls the tail current source ($I_{SS}$) of the main Op Amp, and the sensing factor is unity, the loop gain $T$ is approximately expressed as:

$$T \approx A_{v, A_e} \times g_{m, SS} \times R_{out, CM}$$

Where:

*   **Error Amplifier Gain ($A_{v, A_e}$):** The error amplifier ($A_e$) is a differential pair ($M_{a1}$, $M_{a2}$) with an active current mirror load. Its gain is determined by the transconductance of the input pair ($g_{m, a1}$) and the output resistance of its active load ($r_{o, a4}$) and input differential devices ($r_{o, a2}$).
    $$A_{v, A_e} \approx g_{m, a1} (r_{o, a2} || r_{o, a4})$$
    (This gain is maximized by maximizing the $g_m$ of the input devices and the output resistance of the transistors involved.)

*   **Tail Current Source Transconductance ($g_{m, SS}$):** This term represents the conversion of the control voltage ($\Delta V_{cont}$) output by the error amplifier to a change in the main Op Amp's tail current ($\Delta I_{SS}$).
    $$g_{m, SS} = \frac{\partial I_{SS}}{\partial V_{cont}}$$

*   **Common-Mode Output Resistance ($R_{out, CM}$):** This represents the sensitivity of the main Op Amp's $V_{out, CM}$ to changes in $I_{SS}$. For a high-gain Op Amp, $R_{out, CM}$ is dictated by the common-mode output impedance of the overall Op Amp structure.
    $$R_{out, CM} \approx \frac{1}{2} R_{out, main}$$
    (Where $R_{out, main}$ is the high differential output resistance of the main Op Amp, typically $R_{out, N} || R_{out, P}$.)

A high loop gain ($T$) is desirable in the CMFB network because it ensures that the CMFB circuit can suppress mismatches and common-mode perturbations effectively. A large $T$ drives the final common-mode error to a small value. This is achieved by maximizing $A_{v, A_e}$ (the error amplifier gain) and ensuring the main Op Amp has a high CM output resistance ($R_{out, CM}$).

## 9.13. 
Repeat Problem 9.9.12b for the circuit of Fig. 9.52.
## 9.14.
In the circuit of Fig. 9.73(a), assume that (W/L)1−4 = 100/0.5,C1 = C2 = 0.5 pF, and ISS = 1 mA.
(a) Calculate the small-signal time constant of the circuit.
(b) With a 1-V step at the input [Fig. 9.73(c)], how long does it take for ID2 to reach 0.1ISS?
9.15. It is possible to argue that the auxiliary amplifier in a gain-boosting stage reduces the output impedance.
Consider the circuit as drawn in Fig. 9.89, where the drain voltage of M2 is changed by  V to measure the
output impedance. It seems that, since the feedback provided by A1 attempts to hold VX constant, the change
in the current through rO2 is much greater than in the original circuit, suggesting that Rout ≈ rO2. Explain
the flaw in this argument.
## 9.16. 
Calculate the CMRR of the circuit shown in Fig. 9.73(a).

## 9.17. 
Calculate the input-referred flicker noise of the op amp shown in Fig. 9.85(a).
## 9.18. 
In this problem, we design a two-stage op amp based on the topology shown in Fig. 9.90. Assume a power
budget of 6 mW, a required output swing of 2.5 V, and Lef f = 0.5 μm for all devices.
(a) Allocating a current of 1 mA to the output stage and roughly equal overdrive voltages to M5 and M6,
determine (W/L)5 and (W/L)6. Note that the gate-source capacitance of M5 is in the signal path, whereas
that of M6 is not. Thus, M6 can be quite a lot larger than M5.
(b) Calculate the small-signal gain of the output stage.
(c) With the remaining 1 mA flowing through M7, determine the aspect ratio of M3 (and M4) such that
VGS3 = VGS5. This is to guarantee that if Vin = 0 and hence VX = VY , then M5 carries the expected
current.
(d) Calculate the aspect ratios of M1 and M2 such that the overall voltage gain of the op amp is equal to 500
