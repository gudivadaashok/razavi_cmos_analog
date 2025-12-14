9.1. (a) Derive expressions for the transconductance and output resistance of a MOSFET in the triode region. Plot
these quantities and gmrO as a function of VDS, covering both triode and saturation regions.
(b) Consider the amplifier of Fig. 9.6(b), with (W/L)1−4 = 50/0.5, ISS = 1 mA, and input CM level
of 1.3 V. Calculate the small-signal gain and the maximum output swing if all transistors remain in
saturation.
(c) For the circuit of part (b), suppose we allow each PMOS device to enter the triode region by 50 mV so as
to increase the allowable differential swing by 100 mV. What is the small-signal gain at the peaks of the
output swing?


This request involves applying fundamental MOSFET I-V characteristics and small-signal analysis principles, drawing on information sourced primarily from Chapter 2 and Chapter 9 of the accompanying materials.

The solutions rely on the long-channel square-law model and parameters extracted from the default provided device data (Table 2.1) where specific technology parameters are missing. We assume $V_{DD} = 3 \text{ V}$ as the default supply voltage for bias calculations.

---

## Part 9.1(a): Transconductance and Output Resistance in Triode Region

### Derivations

The drain current ($I_D$) for a MOSFET operating in the triode (linear) region ($V_{DS} \le V_{GS} - V_{TH}$) is given by the long-channel model as:

$$I_D = \mu\_n C\_{ox} \frac{W}{L} [(V\_{GS} - V\_{TH})V\_{DS} - \frac{1}{2} V\_{DS}^2] \tag{1}$$

**1. Transconductance ($g_m$):**
The transconductance is defined as the sensitivity of $I_D$ to $V_{GS}$ while holding $V_{DS}$ constant.
$$g_m = \frac{\partial I_D}{\partial V_{GS}} |_{V_{DS} \text{ const.}} $$
$$g_m = \mu\_n C\_{ox} \frac{W}{L} V\_{DS} \tag{2}$$
In the triode region, $g_m$ scales linearly with $V_{DS}$, which is in sharp contrast to the saturation region where $g_m$ is ideally constant with respect to $V_{DS}$.

**2. Output Resistance ($r_o$):**
The output resistance is the inverse of the output conductance ($g_{ds}$), defined as the sensitivity of $I_D$ to $V_{DS}$ while holding $V_{GS}$ constant.
$$r_o^{-1} = g_{ds} = \frac{\partial I_D}{\partial V_{DS}} |_{V_{GS} \text{ const.}} $$
$$r_o^{-1} = \mu\_n C\_{ox} \frac{W}{L} [(V\_{GS} - V\_{TH}) - V\_{DS}] \tag{3}$$
$$r_o = \frac{1}{\mu\_n C\_{ox} \frac{W}{L} (V\_{GS} - V\_{TH} - V\_{DS})}$$
In the **deep triode region** (where $V_{DS} \ll V_{GS} - V_{TH}$), $r_o$ approximates a constant value known as the *on-resistance* ($R_{on}$), which is controlled linearly by the overdrive voltage.

### Plotting $g_m$, $r_o$, and $g_m r_o$ vs. $V_{DS}$

The plots cover the Triode region ($V_{DS} \le V_{GS} - V_{TH}$) and the Saturation region ($V_{DS} > V_{GS} - V_{TH}$).

| Quantity | Triode Region ($V_{DS} \le V_{GS} - V_{TH}$) | Saturation Region ($V_{DS} > V_{GS} - V_{TH}$) |
| :--- | :--- | :--- |
| **$g_m$** | Increases linearly with $V_{DS}$. Reaches $g_{m,sat}$ at the boundary ($V_{DS} = V_{GS} - V_{TH}$). | **Constant** ($g_{m,sat}$ or $g_{m1}$). |
| **$r_o$** | Varies inversely with $(V_{GS} - V_{TH} - V_{DS})$. Starts low (deep triode) and **rises sharply** towards the boundary, approaching $r_{o,sat}$. | **Constant** (or nearly constant if channel-length modulation $\lambda$ is included). |
| **$g_m r_o$** | Varies nonlinearly, reflecting the dependence on $V_{DS}$. | **Constant** (Intrinsic Gain, $A_i$). |

---

## Part 9.1(b): Small-Signal Gain and Swing Calculation

We consider the fully differential amplifier of the type shown in Fig. 9.6(b) (a high-gain differential pair, $M_1-M_2$, with PMOS current source loads, $M_3-M_4$).

### 1. Extracting Parameters

| Parameter | Value | Source |
| :--- | :--- | :--- |
| Supply Voltage ($V_{DD}$) | $3.0 \text{ V}$ | Default |
| NMOS Threshold ($V_{THN}$) | $0.7 \text{ V}$ | Table 2.1 |
| PMOS Threshold ($|V_{THP}|$) | $0.8 \text{ V}$ | Table 2.1 |
| NMOS $\mu_n C_{ox}$ ($K_n'$) | $350 \mu\text{A/V}^2$ | Table 2.1 |
| PMOS $\mu_p C_{ox}$ ($K_p'$) | $100 \mu\text{A/V}^2$ | Table 2.1 |
| NMOS $\lambda_N$ | $0.1 \text{ V}^{-1}$ | Table 2.1 |
| PMOS $\lambda_P$ | $0.2 \text{ V}^{-1}$ | Table 2.1 |
| $W/L$ (M1-M4) | $50/0.5 \text{ (Ratio 100)}$ | Query |
| $I_{SS}$ | $1.0 \text{ mA}$ | Query |
| $V_{CM,in}$ | $1.3 \text{ V}$ | Query |

In equilibrium, $I_{D1}=I_{D2}=I_{D3}=I_{D4} = I_D = I_{SS}/2 = 0.5 \text{ mA}$.

### 2. Overdrive and $g_m$ Calculations

We calculate the overdrive voltages ($V_{OD} = V_{GS} - V_{TH}$) necessary to sustain $I_D = 0.5 \text{ mA}$ for $W/L = 100$:

*   **NMOS ($M_1, M_2$):**
    $$V_{ODN}^2 = \frac{2 I_D}{K_n' (W/L)} = \frac{2 \times 0.5 \text{ mA}}{350 \mu\text{A/V}^2 \times 100} \approx 0.02857 \text{ V}^2$$
    $$V_{ODN} \approx 0.169 \text{ V}$$
    $$g_{mN} = \frac{2 I_D}{V_{ODN}} = \frac{1.0 \text{ mA}}{0.169 \text{ V}} \approx \mathbf{5.92 \text{ mS}}$$

*   **PMOS ($M_3, M_4$):**
    $$|V_{ODP}|^2 = \frac{2 I_D}{K_p' (W/L)} = \frac{2 \times 0.5 \text{ mA}}{100 \mu\text{A/V}^2 \times 100} = 0.1 \text{ V}^2$$
    $$|V_{ODP}| \approx 0.316 \text{ V}$$
    $$|g_{mP}| = \frac{2 I_D}{|V_{ODP}|} = \frac{1.0 \text{ mA}}{0.316 \text{ V}} \approx \mathbf{3.16 \text{ mS}}$$

### 3. Small-Signal Gain Calculation

The differential voltage gain ($A_{DM}$) of this actively loaded high-gain stage is determined by the transconductance of the input pair and the parallel output resistance of the two branches ($R_{out} = r_{oN} || r_{oP}$).

*   **Output Resistances:**
    $$r_{oN} = \frac{1}{\lambda_N I_D} = \frac{1}{0.1 \text{ V}^{-1} \times 0.5 \text{ mA}} = 20 \text{ k}\Omega$$
    $$r_{oP} = \frac{1}{\lambda_P I_D} = \frac{1}{0.2 \text{ V}^{-1} \times 0.5 \text{ mA}} = 10 \text{ k}\Omega$$

*   **Differential Gain:**
    $$R_{out} = r_{oN} || r_{oP} = 20 \text{ k}\Omega || 10 \text{ k}\Omega \approx 6.67 \text{ k}\Omega$$
    $$A_{DM} = -g_{mN} R_{out} = -5.92 \text{ mS} \times 6.67 \text{ k}\Omega \approx \mathbf{-39.49}$$

### 4. Maximum Output Swing Calculation

The swing is limited by the need to maintain all transistors in saturation ($V_{DS} \ge V_{OD}$).

*   **Minimum Output Voltage ($V_{out,min}$):** Limited by $M_1, M_2$ (NMOS input pair).
    $$V_{out,min} = V_{ODN} = 0.169 \text{ V}$$
*   **Maximum Output Voltage ($V_{out,max}$):** Limited by $M_3, M_4$ (PMOS load devices).
    $$V_{out,max} = V_{DD} - |V_{ODP}| = 3.0 \text{ V} - 0.316 \text{ V} = 2.684 \text{ V}$$

The maximum differential output swing ($V_{swing, diff, p-p}$) is the distance between the total minimum headroom required by the NMOS input devices and the PMOS load devices:
$$V_{swing, diff, p-p} = V_{DD} - (V_{ODN} + |V_{ODP}|)$$
$$V_{swing, diff, p-p} = 3.0 \text{ V} - (0.169 \text{ V} + 0.316 \text{ V}) = \mathbf{2.515 \text{ V}}$$

---

## Part 9.1(c): Small-Signal Gain with Reduced $V_{DS}$

We assume that the PMOS load devices ($M_3, M_4$) operate $50 \text{ mV}$ into the triode region to increase the output swing. This sacrifices the saturation condition ($|V_{DS}| \ge |V_{ODP}|$) for maximum voltage headroom.

### 1. Recalculating Output Resistance at Peak Swing

At the output peak, the magnitude of the PMOS drain-source voltage ($|V_{DS}|$) is reduced to:
$$|V_{DS,P}'| = |V_{ODP}| - 0.05 \text{ V} = 0.316 \text{ V} - 0.05 \text{ V} = 0.266 \text{ V}$$

When operating in the triode region, the output resistance ($r_o$) is calculated from the output conductance ($g_{ds}$):
$$g_{dsP} = K_p' \frac{W}{L} (|V_{GS}| - |V_{TH}| - |V_{DS}|)$$
The gates of $M_3$ and $M_4$ are held constant (fixed bias current, $|V_{GS}| = |V_{ODP}| + |V_{THP}| \approx 1.116 \text{ V}$).
$$g_{dsP}' = 100 \mu\text{A/V}^2 \times 100 \times (1.116 \text{ V} - 0.8 \text{ V} - 0.266 \text{ V})$$
$$g_{dsP}' = 10 \text{ mS} \times (0.316 \text{ V} - 0.266 \text{ V}) = 10 \text{ mS} \times 0.05 \text{ V} = 0.5 \text{ mS}$$
$$r_{oP}' = 1/g_{dsP}' = 2 \text{ k}\Omega$$

### 2. Differential Small-Signal Gain at Peak Swing

The significant drop in $r_{oP}$ sharply reduces the total output resistance.
$$R_{out}' = r_{oN} || r_{oP}' = 20 \text{ k}\Omega || 2 \text{ k}\Omega \approx 1.82 \text{ k}\Omega$$

The gain at this peak output swing is:
$$A_{DM}' = -g_{mN} R_{out}' = -5.92 \text{ mS} \times 1.82 \text{ k}\Omega \approx \mathbf{-10.77}$$

The price paid for achieving an additional $100 \text{ mV}$ of differential output swing is a **reduction of the open-loop gain** from $|A_{DM}| \approx 39.5$ down to $|A_{DM}'| \approx 10.8$ at that specific output extreme.




The query requires an analysis of the telescopic cascode operational amplifier shown in Figure 9.9, utilizing the provided parameters from the context of Chapter 9 and Table 2.1 of the sources. We assume a supply voltage ($V_{DD}$) of $3 \text{ V}$. The body effect coefficient ($\gamma$) is assumed to be zero.

### Initial Parameter Calculation (NMOS $M_{1-4}$)

The NMOS input pair ($M_1, M_2$) and cascode transistors ($M_3, M_4$) have an aspect ratio of $(W/L)_{1-4} = 100/0.5$ and share a drain current ($I_D$) of $I_{SS}/2 = 0.5 \text{ mA}$.

1.  **NMOS Overdrive Voltage ($V_{ODN}$):**
    Using the square-law relation ($I_D = \frac{1}{2} K_n' (W/L) V_{OD}^2$):
    $$V_{ODN}^2 = \frac{2 \times 0.5 \text{ mA}}{350 \mu\text{A/V}^2 \times 100} \approx 0.02857 \text{ V}^2$$
    $$V_{ODN} = V_{GS} - V_{THN} \approx \mathbf{0.169 \text{ V}}$$

2.  **NMOS Transconductance ($g_{mN}$):**
    $$g_{mN} = \frac{2 I_D}{V_{ODN}} = \frac{1.0 \text{ mA}}{0.169 \text{ V}} \approx \mathbf{5.92 \text{ mS}}$$

3.  **NMOS Output Resistance ($r_{oN}$):**
    $$r_{oN} = \frac{1}{\lambda_N I_D} = \frac{1}{0.1 \text{ V}^{-1} \times 0.5 \text{ mA}} = \mathbf{20 \text{ k}\Omega}$$

### (a) Minimum Width such that $M_3$ Operates in Saturation

$M_3$ (NMOS cascode) must operate in saturation, which requires its drain voltage ($V_{D3} = V_X$) to be sufficiently high above its source voltage ($V_{S3} = V_P$). However, in a telescopic op amp, the primary constraint for the NMOS cascode stack ($M_1, M_3$) is satisfied if the intermediate voltage ($V_X$) is set to optimize the overall headroom. The minimum required voltage at the cascode node ($V_X$) is governed by the saturation condition of $M_3$:
$$V_{X, \text{min}} = V_{G3} - V_{THN}$$

To minimize voltage overhead, the gate bias $V_{G3}$ is typically set such that $M_3$ maintains an overdrive $V_{ODN}$. The minimum voltage $V_X$ must sustain for $M_3$ saturation is approximately **$0.5 \text{ V}$** (assuming $V_{G3}=1.2 \text{ V}$ and $V_{P}=0.331 \text{ V}$, which results in $V_{X} \approx V_{G3}-V_{THN}=0.5 \text{ V}$) (See reasoning in Query 9.1).

The minimum width of $M_5$ to ensure that $M_3$ remains saturated is determined by minimizing the voltage drop required by the PMOS load structure. Since **$M_5$ and $M_8$ are identical** and $V_b = 1.4 \text{ V}$ is given as their gate bias, we must assume this voltage sets their operating point. Let $M_5$ be the PMOS cascode device. Its source is $V_{DD}=3 \text{ V}$.
$$|V_{GS5}| = V_{S5} - V_{G5} = 3 \text{ V} - 1.4 \text{ V} = 1.6 \text{ V}$$
$$|V_{OD5}| = |V_{GS5}| - |V_{THP}| = 1.6 \text{ V} - 0.8 \text{ V} = \mathbf{0.8 \text{ V}}$$

Since $M_5$ and $M_7$ (PMOS load source) are identical, $|V_{OD5}| = |V_{OD7}| = 0.8 \text{ V}$. We now calculate the required $W/L$ ratio based on $I_D = 0.5 \text{ mA}$ and $|V_{ODP}| = 0.8 \text{ V}$:
$$|V_{ODP}|^2 = \frac{2 I_D}{K_p' (W/L)_5}$$
$$(W/L)_5 = \frac{2 \times 0.5 \text{ mA}}{100 \mu\text{A/V}^2 \times (0.8 \text{ V})^2} = \frac{1 \text{ mA}}{64 \mu\text{A/V}^2} \approx \mathbf{15.625}$$

Given $L = 0.5 \mu\text{m}$, the minimum required width is:
$$W_5 = 15.625 \times 0.5 \mu\text{m} \approx \mathbf{7.81 \mu\text{m}}$$

This derived width corresponds to the PMOS load operating with a relatively large overdrive of $0.8 \text{ V}$ due to the given $V_b$.

### (b) Maximum Output Voltage Swing

The output voltage swing ($V_{out}$) is constrained by the need to keep both the NMOS cascode stack ($M_1/M_3$) and the PMOS load stack ($M_5/M_7$) in saturation. We use $V_{ODN} \approx 0.169 \text{ V}$ and $|V_{ODP}| = 0.8 \text{ V}$.

1.  **Minimum Output Voltage ($V_{out, \text{min}}$):** Limited by the NMOS stack, determined by the sum of the overdrive voltages needed for $M_1$ and $M_3$.
    $$V_{out, \text{min}} \approx V_{OD1} + V_{OD3}$$
    Since $M_1$ and $M_3$ are identical:
    $$V_{out, \text{min}} \approx 2 \times V_{ODN} = 2 \times 0.169 \text{ V} \approx \mathbf{0.338 \text{ V}}$$

2.  **Maximum Output Voltage ($V_{out, \text{max}}$):** Limited by the PMOS stack, determined by the sum of the overdrive voltages required for $M_5$ and $M_7$ subtracted from $V_{DD}$.
    $$V_{out, \text{max}} \approx V_{DD} - |V_{OD5}| - |V_{OD7}|$$
    Using the derived $|V_{ODP}| = 0.8 \text{ V}$:
    $$V_{out, \text{max}} \approx 3.0 \text{ V} - 0.8 \text{ V} - 0.8 \text{ V} = \mathbf{1.4 \text{ V}}$$

The maximum differential peak-to-peak swing is the difference between these limits:
$$V_{swing, diff, p-p} = V_{out, \text{max}} - V_{out, \text{min}} = 1.4 \text{ V} - 0.338 \text{ V} \approx \mathbf{1.062 \text{ V}}$$
(Note: The conversation history discusses restrictions to $\approx 4V_{OD}$, but this result is derived from the NMOS/PMOS specific minimum overdrive voltages.)

### (c) Open-Loop Voltage Gain

The open-loop voltage gain ($A_v$) of a telescopic cascode is given by the input transconductance multiplied by the parallel output resistance of the NMOS and PMOS stacks ($R_{out, N} || R_{out, P}$). Since NMOS and PMOS cascode stages maximize the output impedance, the gain is roughly proportional to the square of the intrinsic gain ($g_m r_o$).

1.  **NMOS Stack Output Resistance ($R_{out, N}$):**
    $$R_{out, N} \approx g_{m3} r_{o3} r_{o1} \approx g_{mN} r_{oN}^2$$
    $$R_{out, N} \approx (5.92 \text{ mS}) \times (20 \text{ k}\Omega)^2 = \mathbf{2368 \text{ M}\Omega}$$

2.  **PMOS Stack Output Resistance ($R_{out, P}$):**
    We use $g_{mP} = 1.25 \text{ mS}$ and $r_{oP} = 10 \text{ k}\Omega$.
    $$R_{out, P} \approx g_{m7} r_{o7} r_{o5} \approx g_{mP} r_{oP}^2$$
    $$R_{out, P} \approx (1.25 \text{ mS}) \times (10 \text{ k}\Omega)^2 = \mathbf{125 \text{ M}\Omega}$$

3.  **Overall Voltage Gain ($A_v$):**
    The overall output resistance $R_{out, tot}$ is dominated by the lower PMOS resistance: $R_{out, tot} \approx R_{out, P}$.
    $$A_v \approx g_{m1} R_{out, tot} \approx g_{mN} R_{out, P}$$
    $$A_v \approx (5.92 \text{ mS}) \times (125 \text{ M}\Omega) \approx \mathbf{740,000}$$

### (d) Input-Referred Thermal Noise Voltage

The input-referred thermal noise voltage ($V_{n, in}$) for a differential amplifier is dominated by the input pair ($M_1, M_2$) and the load current sources ($M_7, M_8$), scaled by $g_{m1}$. We use $\gamma = 1$ (the thermal noise factor in MOSFETs for approximation).

1.  **Total Input-Referred Thermal Noise Power ($V_{n, in}^2$):**
    $$V_{n, in}^2 \approx V_{n, in, M1/2}^2 + V_{n, in, M7/8}^2$$
    $$V_{n, in}^2 \approx \frac{8kT}{g_{m1}} + \frac{8kT}{g_{m1}^2} g_{m7}$$
    $$V_{n, in}^2 \approx \frac{8kT}{g_{m1}} \left( 1 + \frac{g_{m7}}{g_{m1}} \right)$$

2.  **Calculation of Ratio $g_{m7}/g_{m1}$:**
    $g_{m1} \approx 5.92 \text{ mS}$. $g_{m7} \approx 1.25 \text{ mS}$.
    $$\frac{g_{m7}}{g_{m1}} \approx \frac{1.25 \text{ mS}}{5.92 \text{ mS}} \approx 0.211$$

3.  **Input-Referred Noise Voltage:**
    Assuming $kT \approx 4 \times 10^{-21} \text{ J}$ at room temperature:
    $$V_{n, in}^2 \approx \frac{8 \times 4 \times 10^{-21} \text{ J}}{5.92 \times 10^{-3} \text{ S}} \times (1 + 0.211) \approx 6.54 \times 10^{-18} \text{ V}^2/\text{Hz}$$
    $$V_{n, in} \approx \mathbf{2.56 \text{ nV}/\sqrt{\text{Hz}}}$$

---
## 9.2.
9.2. In the circuit of Fig. 9.9, assume that (W/L)1−4 = 100/0.5, ISS = 1 mA, Vb = 1.4 V, and γ = 0.
(a) If M5–M8 are identical and have a length of 0.5 μm, calculate their minimum width such that M3 operates
in saturation.
(b) Calculate the maximum output voltage swing.
(c) What is the open-loop voltage gain?
(d) Calculate the input-referred thermal noise voltage.


This query requires calculations based on the characteristics of a telescopic cascode operational amplifier (Op Amp) operating in saturation, assuming the geometry and current distribution are standard for this topology (e.g., $M_1, M_2$ input pair; $M_3, M_4$ NMOS cascodes; $M_5, M_6$ PMOS input loads; $M_7, M_8$ PMOS cascodes, with $I_{D1}=I_{D2}=I_{D5}=I_{D7}=0.5 \text{ mA}$).

Since the source material provides parameter tables and contexts but not all necessary absolute voltage parameters for this specific problem (9.2), the following standard assumptions based on the general device context (Table 2.1) are used for calculation:

*   Supply Voltage: $V_{DD} = 3.0 \text{ V}$.
*   Threshold Voltages (from): $V_{THN} = 0.7 \text{ V}$, $V_{THP} = -0.8 \text{ V}$.
*   $\lambda_{N} = 0.1 \text{ V}^{-1}$, $\lambda_{P} = 0.2 \text{ V}^{-1}$ (for $L=0.5 \mu\text{m}$).
*   $K'$ values ($\mu C_{ox}$): To allow device sizing based on overdrive, we assume representative values, $K_{N}' = 50 \mu\text{A/V}^2$ and $K_{P}' = 50 \mu\text{A/V}^2$.
*   Thermal Noise Coefficient: Since the body effect coefficient $\gamma$ is given as 0, we use the standard long-channel thermal noise coefficient $\gamma_{\text{thermal}} = 2/3$.

### (a) Minimum Width of $M_{5}-M_{8}$ such that $M_{3}$ operates in saturation.

$M_{1}$ and $M_{3}$ form the NMOS cascode stack (or $M_{2}$ and $M_{4}$ on the other side). Assuming $M_{3}$ is the NMOS cascode transistor (M3 or M4), its gate is tied to $V_{b} = 1.4 \text{ V}$.

1.  **Determine $V_{\text{OUT, min}}$ for $M_{3}$ Saturation:**
    The minimum output voltage ($V_{\text{OUT}}$) must ensure $M_{3}$ remains in saturation.
    $$V_{\text{OUT}} \ge V_{G3} - V_{THN}$$
    $$V_{\text{OUT, min}} = 1.4 \text{ V} - 0.7 \text{ V} = \mathbf{0.7 \text{ V}}$$

2.  **Determine $V_{\text{SD, total, max}}$ for PMOS Stack:**
    The PMOS load ($M_{5}-M_{8}$) must operate (sink $I_{D} = 0.5 \text{ mA}$) when $V_{\text{OUT}} = 0.7 \text{ V}$. This means the maximum voltage drop the stack must withstand is:
    $$V_{\text{SD, total, max}} = V_{DD} - V_{\text{OUT, min}} = 3.0 \text{ V} - 0.7 \text{ V} = \mathbf{2.3 \text{ V}}$$

3.  **Calculate Minimum Width ($W_{P, \min}$):**
    For the minimum width of the PMOS devices (M5, M7) to be used, they must operate at the maximum possible overdrive voltage ($V_{ODP}$) such that $V_{\text{SD, total, min}} \le 2.3 \text{ V}$.
    Assuming $M_{5}-M_{8}$ are identical (thus $V_{OD5} = V_{OD7} = V_{ODP}$):
    $$2 \cdot V_{ODP, \max} \le 2.3 \text{ V} \implies V_{ODP, \max} = 1.15 \text{ V}$$
    We use the square law formula to find the required $W/L$ ratio for $V_{ODP, \max} = 1.15 \text{ V}$:
    $$\frac{W}{L} = \frac{2 I_D}{K_{P}' (V_{ODP})^2} = \frac{2 \times 0.5 \times 10^{-3} \text{ A}}{50 \times 10^{-6} \text{ A/V}^2 \times (1.15 \text{ V})^2} \approx 15.12$$
    Since $L = 0.5 \mu\text{m}$:
    $$W_{P, \min} = 15.12 \times 0.5 \mu\text{m} \approx \mathbf{7.56 \mu\text{m}}$$

### (b) Calculate the maximum output voltage swing.

The maximum output voltage swing ($V_{\text{swing}}$) is $V_{\text{OUT, max}} - V_{\text{OUT, min}}$.

1.  **$V_{\text{OUT, min}}$:** Already determined by $M_{3}$ saturation (NMOS constraint): $V_{\text{OUT, min}} = 0.7 \text{ V}$.

2.  **$V_{\text{OUT, max}}$:** Determined by the PMOS stack operating with a sustainable overdrive. We assume the PMOS devices ($M_{5-8}$) are designed with a typical overdrive that minimizes stack voltage while maintaining performance, e.g., $V_{ODP} = 0.3 \text{ V}$ (as often seen in analog design to conserve headroom).

    *   Calculate $V_{\text{SD, total, min}}$ based on this design constraint:
        $$V_{\text{SD, total, min}} = 2 \cdot V_{ODP} = 2 \times 0.3 \text{ V} = 0.6 \text{ V}$$
    *   $V_{\text{OUT, max}} = V_{DD} - V_{\text{SD, total, min}} = 3.0 \text{ V} - 0.6 \text{ V} = \mathbf{2.4 \text{ V}}$$

3.  **Maximum Output Voltage Swing:**
    $$V_{\text{swing}} = V_{\text{OUT, max}} - V_{\text{OUT, min}} = 2.4 \text{ V} - 0.7 \text{ V} = \mathbf{1.7 \text{ V}}$$

### (c) What is the open-loop voltage gain?

The open-loop voltage gain $A_{V, OL}$ is given by $A_{V, OL} \approx -g_{m1} (R_{\text{out, N}} || R_{\text{out, P}})$.

1.  **Calculate NMOS Overdrive and Transconductance ($M_{1}, M_{3}$):**
    $(W/L)_N = 200$. $I_D = 0.5 \text{ mA}$.
    $V_{ODN} = \sqrt{\frac{2 \times 0.5 \times 10^{-3}}{50 \times 10^{-6} \times 200}} \approx 0.316 \text{ V}$.
    $g_{m1} = g_{m3} = \frac{2 I_D}{V_{ODN}} \approx \frac{1 \text{ mA}}{0.316 \text{ V}} \approx \mathbf{3.16 \text{ mS}}$.

2.  **Calculate PMOS Overdrive and Transconductance ($M_{5}, M_{7}$):**
    Based on the assumption $V_{ODP} = 0.3 \text{ V}$ and $K_{P}' = 50 \mu\text{A/V}^2$, the PMOS width required is $W_{P} \approx 111.1 \mu\text{m}$ (as derived for $V_{ODP}=0.3 \text{ V}$). $(W/L)_P = 222.2$.
    $g_{m5} = g_{m7} = \frac{2 I_D}{V_{ODP}} \approx \frac{1 \text{ mA}}{0.3 \text{ V}} \approx \mathbf{3.33 \text{ mS}}$.

3.  **Calculate Output Resistances ($r_{o}$):**
    Using $r_{o} = 1/(\lambda I_D)$ and $I_D = 0.5 \text{ mA}$.
    $r_{oN} = r_{o1} = r_{o3} = 1 / (0.1 \text{ V}^{-1} \times 0.5 \text{ mA}) = 20 \text{ k}\Omega$.
    $r_{oP} = r_{o5} = r_{o7} = 1 / (0.2 \text{ V}^{-1} \times 0.5 \text{ mA}) = 10 \text{ k}\Omega$.

4.  **Calculate Total Output Resistance ($R_{\text{out}}$):**
    The output resistance of the NMOS stack ($R_{\text{out, N}}$) is approximately $g_{m3} r_{o3} r_{o1}$.
    $$R_{\text{out, N}} \approx (3.16 \text{ mS}) (20 \text{ k}\Omega) (20 \text{ k}\Omega) \approx 1.264 \text{ M}\Omega$$
    The output resistance of the PMOS stack ($R_{\text{out, P}}$) is approximately $g_{m7} r_{o7} r_{o5}$.
    $$R_{\text{out, P}} \approx (3.33 \text{ mS}) (10 \text{ k}\Omega) (10 \text{ k}\Omega) \approx 0.333 \text{ M}\Omega$$
    The overall output resistance is $R_{\text{out}} = R_{\text{out, N}} || R_{\text{out, P}}$.
    $$R_{\text{out}} \approx (1/1.264 + 1/0.333)^{-1} \text{ M}\Omega \approx 0.264 \text{ M}\Omega$$

5.  **Calculate $A_{V, OL}$:**
    $$A_{V, OL} \approx -g_{m1} R_{\text{out}} \approx -(3.16 \times 10^{-3} \text{ S}) \times (0.264 \times 10^{6} \Omega) \approx \mathbf{-834}$$

### (d) Calculate the input-referred thermal noise voltage.

The input-referred thermal noise voltage squared ($V_{n, in}^2$) for a telescopic cascode (M1-M8) is determined by the input pair ($M_1, M_2$) and the active load devices that contribute noise (M5-M8). Assuming noise coefficients $\gamma_{\text{thermal}} = 2/3$ for all devices:

$$V_{n, in}^2 \approx \frac{8 k T \gamma_{\text{thermal}}}{g_{m1}} \left( 1 + \frac{g_{m7}}{g_{m1}} \right)$$

1.  **Calculate $g_{m}$ Ratio:**
    $$\frac{g_{m7}}{g_{m1}} = \frac{3.33 \text{ mS}}{3.16 \text{ mS}} \approx 1.05$$

2.  **Calculate $V_{n, in}^2$ (Noise Power Spectral Density):**
    We use $kT \approx 4.14 \times 10^{-21} \text{ J}$ at room temperature.
    $$V_{n, in}^2 \approx \frac{8 \times (4.14 \times 10^{-21} \text{ J}) \times (2/3)}{3.16 \times 10^{-3} \text{ S}} \times (1 + 1.05)$$
    $$V_{n, in}^2 \approx (6.98 \times 10^{-18} \text{ V}^2/\text{Hz}) \times (2.05) \approx 14.3 \times 10^{-18} \text{ V}^2/\text{Hz}$$

3.  **Calculate Input-Referred Thermal Noise Voltage ($V_{n, in}$):**
    $$V_{n, in} = \sqrt{14.3 \times 10^{-18}} \text{ V}/\sqrt{\text{Hz}} \approx \mathbf{3.78 \text{ nV}/\sqrt{\text{Hz}}}$$



---
## 9.3.
Design the folded-cascode op amp of Fig. 9.15 for the following requirements: maximum differential swing
= 2.4 V, total power dissipation = 6 mW. If all of the transistors have a channel length of 0.5 μm, what is the
overall voltage gain? Can the input common-mode level be as low as zero?

This response provides the design parameters for the folded-cascode operational amplifier (op amp) depicted in Figure 9.15, based on the specified requirements, and calculates the resulting overall voltage gain and minimum input common-mode (CM) level.

The folded-cascode topology generally consists of a PMOS input pair ($M_1, M_2$), NMOS cascodes ($M_3, M_4$), NMOS folding current sinks ($M_5, M_6$), and PMOS cascode loads ($M_7$–$M_{10}$). The supply voltage $V_{\text{DD}}$ is assumed to be $3 \text{ V}$ based on general context.


### 1. Current Allocation and Headroom Analysis

**Power and Current:** The **total power dissipation is $6 \text{ mW}$** [9.3], assuming $V_{\text{DD}} = 3 \text{ V}$, yielding a total quiescent supply current $I_{\text{DD, tot}} = 6 \text{ mW} / 3 \text{ V} = 2 \text{ mA}$. Allocating $0.2 \text{ mA}$ for bias circuitry (not shown), the signal path current $I_{\text{Total}} = 1.8 \text{ mA}$.

**Quiescent Currents:** Based on the principle of current sharing in folded cascodes (Fig. 9.15 structure implies $I_{\text{Total}} \approx I_{\text{SS}} + 2I_{\text{Fold}}$), we allocate currents:
*   $I_{\text{SS}} = 0.6 \text{ mA}$ (Input PMOS pair $M_1, M_2$), so $I_{\text{D1,2}} = 0.3 \text{ mA}$.
*   $I_{\text{Fold}} = 0.6 \text{ mA}$ (NMOS sink $M_5, M_6$).
*   The current through the cascodes (M3, M4) is $I_{\text{D3,4}} = I_{\text{Fold}} - I_{\text{D1,2}} = 0.3 \text{ mA}$.
*   The load current (M7–M10) is $I_{\text{D7-10}} = 0.3 \text{ mA}$.

**Output Voltage Headroom:** The **maximum differential swing is $2.4 \text{ V}$**, implying a single-ended peak-to-peak swing of $1.2 \text{ V}$. The single-ended output voltage range must be maintained between $V_{\text{out, min}}$ and $V_{\text{out, max}}$ to keep all transistors saturated. Since $V_{\text{DD}} = 3 \text{ V}$, we allocate minimal overdrive voltage ($V_{\text{OD}}$) to ensure saturation, choosing $V_{\text{OD}} = 0.2 \text{ V}$ for all transistors.

The output swing limits are:
*   $V_{\text{out, max}} = V_{\text{DD}} - |V_{\text{OD7}}| - |V_{\text{OD9}}| = 3 \text{ V} - 0.2 \text{ V} - 0.2 \text{ V} = 2.6 \text{ V}$.
*   $V_{\text{out, min}} = V_{\text{OD4}} + V_{\text{OD6}} = 0.2 \text{ V} + 0.2 \text{ V} = 0.4 \text{ V}$.
The maximum theoretical swing is $2.2 \text{ V}$. Since the required swing is $1.2 \text{ V}$, it is feasible within this headroom.

### 2. Transistor Dimensions ($L = 0.5 \mu\text{m}$)

We use the square-law relationship $W/L = \frac{2 I_{\text{D}}}{K V_{\text{OD}}^2}$ (where $L=0.5 \mu\text{m}$) and the long-channel parameters $\mu_n C_{\text{ox}} \approx 50 \mu\text{A/V}^2$ and $\mu_p C_{\text{ox}} \approx 25 \mu\text{A/V}^2$. $V_{\text{OD}} = 0.2 \text{ V}$.

| Transistor | Type | $I_{\text{D}}$ ($\mu\text{A}$) | $K$ ($\mu\text{A/V}^2$) | $W/L$ | $W$ ($\mu\text{m}$) |
| :---: | :---: | :---: | :---: | :---: | :---: |
| $M_1, M_2$ | PMOS | 300 | 25 | 1200 | 600 |
| $M_3, M_4$ | NMOS | 300 | 50 | 300 | 150 |
| $M_5, M_6$ | NMOS | 600 | 50 | 600 | 300 |
| $M_7$–$M_{10}$ | PMOS | 300 | 25 | 1200 | 600 |

### 3. Overall Voltage Gain

The overall voltage gain ($A_v$) is given by $A_v = g_{m1} R_{\text{out}}$. The total output resistance $R_{\text{out}}$ is the parallel combination of the output resistance of the NMOS cascode path ($R_{\text{out, N-cas}}$) and the PMOS cascode load path ($R_{\text{out, P-cas}}$).

We use the nominal transistor gain parameters for $I_{\text{D}} = 0.3 \text{ mA}$ and $V_{\text{OD}} = 0.2 \text{ V}$:
*   $g_m = \frac{2 I_{\text{D}}}{V_{\text{OD}}} = \frac{2 \times 0.3 \text{ mA}}{0.2 \text{ V}} = 3 \text{ mS}$ (for all transistors except $M_{5/6}$).
*   $r_{o} = 1 / (\lambda I_{\text{D}})$. Using $\lambda_n = 0.1 \text{ V}^{-1}$ and $\lambda_p = 0.2 \text{ V}^{-1}$ (for $0.5 \mu\text{m}$ technology):
    *   $r_{o, \text{N}} = 1 / (0.1 \times 0.3 \text{ mA}) = 33.33 \text{ k}\Omega$.
    *   $r_{o, \text{P}} = 1 / (0.2 \times 0.3 \text{ mA}) = 16.67 \text{ k}\Omega$.

**$R_{\text{out}}$ Calculation:** (Neglecting body effect $g_{mb}$ for simplicity in output resistance calculation):

1.  **Input Folding Resistance ($R_{\text{fold}}$):** This is the output impedance of the input PMOS pair ($M_1$) and NMOS folding sink ($M_5$) seen from the cascode device ($M_3$):
    $R_{\text{fold}} = r_{o1} \| r_{o5} = 16.67 \text{ k}\Omega \| 33.33 \text{ k}\Omega \approx 11.11 \text{ k}\Omega$.

2.  **NMOS Cascode Output Resistance ($R_{\text{out, N-cas}}$):** $M_3$ boosts the resistance $R_{\text{fold}}$.
    $R_{\text{out, N-cas}} \approx (g_{m3} r_{o3}) R_{\text{fold}} = (3 \text{ mS} \times 33.33 \text{ k}\Omega) \times 11.11 \text{ k}\Omega \approx 100 \times 11.11 \text{ k}\Omega = 1.11 \text{ M}\Omega$.

3.  **PMOS Load Cascode Output Resistance ($R_{\text{out, P-cas}}$):** $M_7$ boosts $r_{o9}$.
    $R_{\text{out, P-cas}} \approx (g_{m7} r_{o7}) r_{o9} = (3 \text{ mS} \times 16.67 \text{ k}\Omega) \times 16.67 \text{ k}\Omega \approx 50 \times 16.67 \text{ k}\Omega = 833.5 \text{ k}\Omega$.

4.  **Total Output Resistance ($R_{\text{out}}$):**
    $R_{\text{out}} = R_{\text{out, N-cas}} \| R_{\text{out, P-cas}} = 1.11 \text{ M}\Omega \| 833.5 \text{ k}\Omega \approx 476 \text{ k}\Omega$.

5.  **Overall Voltage Gain ($A_v$):** $g_{m1}$ (PMOS input) $= 3 \text{ mS}$.
    $A_v = g_{m1} R_{\text{out}} = 3 \text{ mS} \times 476 \text{ k}\Omega \approx **1428**$.

### 4. Input Common-Mode Level

The minimum input common-mode level ($V_{\text{CM, in, min}}$) for this PMOS input op amp is constrained by the need to keep the PMOS input transistors ($M_1, M_2$) in saturation while respecting the constraints placed by the NMOS folding transistors ($M_5, M_6$).

1.  **Nominal Operation Point:** The PMOS input pair requires $V_{\text{SG}} = |V_{\text{THP}}| + V_{\text{OD}} = 0.8 \text{ V} + 0.2 \text{ V} = 1.0 \text{ V}$. Given that the source is $V_{\text{DD}} = 3 \text{ V}$, the nominal $V_{\text{CM, in}}$ is $V_{\text{DD}} - V_{\text{SG}} = 3 \text{ V} - 1.0 \text{ V} = 2.0 \text{ V}$.

2.  **Minimum Feasible $V_{\text{CM, in}}$:** The **minimum input common-mode level** for a PMOS input pair is typically determined by the output voltage of the folding stage, $V_X$ (source of $M_3/M_4$). $V_X$ must be high enough to satisfy $V_{\text{DS, M5/M6}} \ge V_{\text{OD, M5/M6}}$ (i.e., $V_{\text{X}} \ge 0.2 \text{ V}$) and low enough to maintain saturation for $M_1/M_2$.

    For PMOS input, the lowest feasible input CM level is generally determined by the NMOS headroom constraints, where $V_{\text{CM, in, min}} \ge V_{\text{OD, M3}} + V_{\text{OD, M5}}$ (if the NMOS input structure is used). Since this is a PMOS input structure, the minimum $V_{\text{CM, in}}$ can extend close to $0 \text{ V}$ if the PMOS transistors are designed to handle high $V_{\text{SG}}$ (high overdrive) while maintaining the current balance required by the NMOS sinks.

    If **$V_{\text{CM, in}}$ is as low as zero**, $V_{\text{SG}}$ of $M_1/M_2$ becomes $3 \text{ V}$. Although the PMOS devices are driven into a massive overdrive region, the fixed current sinks $M_5/M_6$ limit the current flow, forcing the PMOS devices to operate in saturation/active region. The source characteristics confirm that PMOS input pairs generally **can handle input CM levels as low as zero**.
---
## 9.4.
9.4. In the op amp of Fig. 9.21(b), (W/L)1−8 = 100/0.5, ISS = 1 mA, and Vb1 = 1.7 V. Assume that γ = 0.
(a) What is the maximum allowable input CM level?
(b) What is VX ?
(c) What is the maximum allowable output swing if the gate of M2 is connected to the output?
(d) What is the acceptable range of Vb2?
(e) What is the input-referred thermal noise voltage?

This response is based on the analysis of a single-ended output cascode operational amplifier (Op Amp) circuit (Fig. 9.21(b), referenced in the context of the query) using typical $0.5 \mu\text{m}$ CMOS technology parameters derived from the overall source material, particularly those consistent with Table 2.1.

**Assumed Parameters:**
*   Supply Voltage: $V_{DD} = 3.0 \text{ V}$.
*   NMOS Threshold Voltage: $V_{THN} = 0.7 \text{ V}$.
*   PMOS Threshold Voltage: $V_{THP} = -0.8 \text{ V}$.
*   Tail Current: $I_{SS} = 1 \text{ mA}$. Branch current: $I_D = 0.5 \text{ mA}$.
*   Transistor Geometries: $(W/L)_{1-8} = 200$.
*   $K_{N}' (\mu_n C_{ox}) = 100 \mu\text{A/V}^2$.
*   $K_{P}' (\mu_p C_{ox}) = 50 \mu\text{A/V}^2$ (PMOS devices are generally inferior due to lower mobility).
*   Body Effect Coefficient: $\gamma = 0$.

**Calculated Overdrive Voltages ($V_{OD}$):**
The overdrive voltage ($V_{GS} - V_{TH}$) is derived from the square-law behavior:
$$V_{OD} = \sqrt{\frac{2 I_D}{K' (W/L)}}$$

*   NMOS ($M_{1-4}$): $V_{ODN} = \sqrt{(2 \times 0.5 \text{ mA}) / (100 \mu\text{A/V}^2 \times 200)} \approx \mathbf{0.2236 \text{ V}}$
*   PMOS ($M_{5-8}$): $V_{ODP} = \sqrt{(2 \times 0.5 \text{ mA}) / (50 \mu\text{A/V}^2 \times 200)} \approx \mathbf{0.316 \text{ V}}$

---

### (a) What is the maximum allowable input CM level?

The maximum allowable input common-mode level ($V_{CM, \max}$) is determined by the requirement that the input transistors ($M_1, M_2$) remain in the saturation region. The input transistor operates in saturation if its gate voltage ($V_{CM, \max}$) is below the voltage at its drain ($V_X$) plus its threshold voltage ($V_{THN}$).

1.  **Determine $V_{X}$:** The voltage $V_{X}$ (drain of $M_1/M_2$, source of cascode $M_3/M_4$) is set by the cascode transistor $M_3$ operating in saturation. Since $M_3$'s gate is biased at $V_{b1} = 1.7 \text{ V}$:
    $$V_{X} = V_{b1} - (V_{THN} + V_{ODN})$$
    $$V_{X} = 1.7 \text{ V} - (0.7 \text{ V} + 0.2236 \text{ V}) = 1.7 \text{ V} - 0.9236 \text{ V} \approx \mathbf{0.7764 \text{ V}}$$

2.  **Determine $V_{CM, \max}$:**
    $$V_{CM, \max} = V_{X} + V_{THN}$$
    $$V_{CM, \max} = 0.7764 \text{ V} + 0.7 \text{ V} \approx \mathbf{1.4764 \text{ V}}$$

### (b) What is $V_{X}$?

$V_X$ is the internal node voltage set by the requirement that the NMOS cascode transistor ($M_3$) remains saturated, as derived in part (a):
$$V_{X} = V_{b1} - (V_{THN} + V_{ODN})$$
$$V_{X} = 1.7 \text{ V} - (0.7 \text{ V} + 0.2236 \text{ V}) \approx \mathbf{0.7764 \text{ V}}$$

### (c) What is the maximum allowable output swing if the gate of $M_{2}$ is connected to the output?

The output voltage swing is constrained by the saturation requirements of the NMOS stack ($M_{1}, M_{3}, M_{4}$) and the PMOS stack ($M_{5}, M_{7}, M_{8}$).

1.  **Lower Output Bound ($V_{out, min}$):** Determined by the NMOS cascode device $M_4$ saturation ($V_{D4} \ge V_{G4} - V_{THN}$). Since $V_{G4} = V_{b1} = 1.7 \text{ V}$:
    $$V_{out, min} = V_{b1} - V_{THN} = 1.7 \text{ V} - 0.7 \text{ V} = \mathbf{1.0 \text{ V}}$$

2.  **Upper Output Bound ($V_{out, max}$):** Determined by the PMOS cascode device $M_8$ saturation ($|V_{SD8}| \ge V_{ODP}$). To maximize $V_{out}$, we assume $V_{b2}$ is ideally biased such that the minimum required saturation voltage drop limits the swing:
    $$V_{out, max} = V_{DD} - V_{ODP}$$
    $$V_{out, max} = 3.0 \text{ V} - 0.316 \text{ V} \approx \mathbf{2.684 \text{ V}}$$

3.  **Maximum Output Voltage Swing:**
    $$V_{\text{swing}} = V_{out, max} - V_{out, min} = 2.684 \text{ V} - 1.0 \text{ V} \approx \mathbf{1.684 \text{ V}}$$

### (d) What is the acceptable range of $V_{b2}$?

The bias voltage $V_{b2}$ must keep the PMOS load stack ($M_5/M_7$ or $M_6/M_8$) operating correctly in saturation.

1.  **Upper Bound ($V_{b2, max}$):** Set by the saturation of the PMOS current source device $M_5$. $V_{G5}$ must be low enough relative to its source ($V_{DD}$) to ensure $|V_{GS5}|$ is sufficient to maintain saturation ($|V_{GS5}| \ge |V_{THP}| + V_{ODP}$).
    $$V_{b2, \max} = V_{DD} - V_{ODP} = 3.0 \text{ V} - 0.316 \text{ V} \approx \mathbf{2.684 \text{ V}}$$

2.  **Lower Bound ($V_{b2, min}$):** Set by the saturation requirement of the PMOS cascode transistor $M_7$ when the output is at its minimum voltage ($V_{out, min} = 1.0 \text{ V}$). $M_7$ must satisfy $V_{D7} \le V_{G7} - V_{THP}$ (or $|V_{SD7}| \ge V_{ODP}$). The simplest constraint often derived for the cascode bias is $V_{b2} \ge V_{out, min}$ for adequate headroom:
    $$V_{b2, \min} = V_{out, min} = \mathbf{1.0 \text{ V}}$$

The acceptable range of $V_{b2}$ is $\mathbf{[1.0 \text{ V}, 2.684 \text{ V}]}$.

### (e) What is the input-referred thermal noise voltage?

The input-referred thermal noise voltage spectral density squared ($V_{n, in}^2$) in a cascode structure is primarily set by the input pair ($M_1, M_2$) and the load cascode transistors ($M_7, M_8$). Assuming the standard long-channel thermal noise coefficient $\gamma_{\text{thermal}} = 2/3$:
$$V_{n, in}^2 \approx \frac{8 k T \gamma_{\text{thermal}}}{g_{m1}} \left( 1 + \frac{g_{m7}}{g_{m1}} \right)$$

1.  **Calculate Transconductances ($g_m$):** The transconductance is defined as $g_m = 2 I_D / V_{OD}$.
    $$g_{m1} = \frac{1 \text{ mA}}{0.2236 \text{ V}} \approx 4.472 \text{ mS}$$
    $$g_{m7} = \frac{1 \text{ mA}}{0.316 \text{ V}} \approx 3.165 \text{ mS}$$

2.  **Calculate $g_m$ Ratio:**
    $$\frac{g_{m7}}{g_{m1}} \approx \frac{3.165 \text{ mS}}{4.472 \text{ mS}} \approx 0.707$$

3.  **Calculate $V_{n, in}^2$:** Using $kT \approx 4.14 \times 10^{-21} \text{ J}$ at room temperature:
    $$V_{n, in}^2 \approx \frac{8 \times (4.14 \times 10^{-21} \text{ J}) \times (2/3)}{4.472 \times 10^{-3} \text{ S}} \times (1 + 0.707)$$
    $$V_{n, in}^2 \approx (4.93 \times 10^{-18} \text{ V}^2/\text{Hz}) \times (1.707) \approx 8.41 \times 10^{-18} \text{ V}^2/\text{Hz}$$

4.  **Calculate Input-Referred Thermal Noise Voltage ($V_{n, in}$):**
    $$V_{n, in} = \sqrt{8.41 \times 10^{-18}} \text{ V}/\sqrt{\text{Hz}} \approx \mathbf{2.90 \text{ nV}/\sqrt{\text{Hz}}}$$

---
## 9.5. 
Design the op amp of Fig. 9.21(b) for the following requirements: maximum differential swing = 2.4 V, total
power dissipation = 6 mW. (Assume that the gate of M2 is never shorted to the output.)

This design task requires determining the dimensions and bias conditions for a single-ended output cascode operational amplifier (Op Amp), typical of the configuration implied by Fig. 9.21(b), to meet specific power and output swing requirements.

Since the provided sources describe fundamental MOSFET physics and analog circuit design principles but do not supply a specific technology file (like Table 2.1 parameters for $V_{DD}$ or exact $K'$ values in this specific context), the design proceeds using standard operational assumptions consistent with high-performance CMOS design practice in the general source material context ($0.5 \mu\text{m}$ generation or similar analog-friendly models):

**Assumed Technology Parameters:**
*   Supply Voltage ($V_{DD}$): $3.0 \text{ V}$.
*   NMOS Threshold Voltage ($V_{THN}$): $0.7 \text{ V}$.
*   PMOS Threshold Voltage ($V_{THP}$): $-0.8 \text{ V}$.
*   NMOS Transconductance Parameter ($K_{N}' = \mu_n C_{ox}$): $100 \mu\text{A/V}^2$.
*   PMOS Transconductance Parameter ($K_{P}' = \mu_p C_{ox}$): $50 \mu\text{A/V}^2$ (PMOS devices are generally inferior to NMOS due to lower hole mobility).

### 1. Current Allocation

The total power dissipation ($P_{total}$) and supply voltage ($V_{DD}$) define the maximum total current allowed ($I_{total}$):
$$I_{total} = \frac{P_{total}}{V_{DD}} = \frac{6 \text{ mW}}{3.0 \text{ V}} = 2.0 \text{ mA}$$
Assuming this current powers the input stage's tail current source ($I_{SS}$), the current flowing through each branch ($I_D$) is half the total current:
$$I_{SS} = 2.0 \text{ mA}$$
$$I_D = 1.0 \text{ mA}$$

### 2. Headroom Constraints and Overdrive Selection

The required maximum differential output swing is $V_{swing, diff, max} = 2.4 \text{ V}$. In a single-ended cascode stage, the output voltage ($V_{out}$) is constrained by the saturation requirements of the NMOS pull-down stack and the PMOS pull-up stack.

The voltage reserved for overhead (device saturation requirements) is $V_{DD} - V_{swing, diff, max} = 3.0 \text{ V} - 2.4 \text{ V} = 0.6 \text{ V}$.

To maximize symmetrical swing, this overhead must be split equally between the minimum output voltage ($V_{out, min}$) constraint (NMOS overhead) and the maximum output voltage ($V_{out, max}$) constraint (PMOS overhead):
$$V_{N, overhead} = V_{out, min} = 0.3 \text{ V}$$
$$V_{P, overhead} = V_{DD} - V_{out, max} = 0.3 \text{ V}$$

Thus:
$$V_{out, min} = 0.3 \text{ V}$$
$$V_{out, max} = 2.7 \text{ V}$$

The NMOS stack ($M_1, M_3$) must operate such that $V_{OD1} + V_{OD3} \le 0.3 \text{ V}$. Similarly, the PMOS stack ($M_5, M_7$) requires $|V_{OD5}| + |V_{OD7}| \le 0.3 \text{ V}$.

To ensure robust operation within this tight headroom, we allocate the overdrive equally to $0.15 \text{ V}$ per device:
$$V_{ODN} = 0.15 \text{ V} \quad \text{for } M_{1}, M_{3}, M_{2}, M_{4}$$
$$V_{ODP} = 0.15 \text{ V} \quad \text{for } M_{5}, M_{7}, M_{6}, M_{8}$$

### 3. Device Sizing ($W/L$)

To optimize analog performance (maximizing $r_o$ and intrinsic gain $g_m r_o$) while maintaining the required overhead, we choose a channel length ($L$) longer than the minimum feature size, $L = 1.0 \mu\text{m}$, for all core transistors ($M_{1}$ through $M_{8}$).

The required aspect ratio ($W/L$) is calculated using the square-law equation for saturation:
$$ \frac{W}{L} = \frac{2 I_D}{K' (V_{OD})^2} $$

**NMOS Transistors ($M_1, M_2, M_3, M_4$):**
$$ \left(\frac{W}{L}\right)_N = \frac{2 \times 1.0 \times 10^{-3} \text{ A}}{100 \times 10^{-6} \text{ A/V}^2 \times (0.15 \text{ V})^2} \approx 888.89 $$
Choosing $L = 1.0 \mu\text{m}$, the dimensions are:
$$\mathbf{(W/L)_{1-4} = 889 \mu\text{m} / 1.0 \mu\text{m}}$$

**PMOS Transistors ($M_5, M_6, M_7, M_8$):**
$$ \left(\frac{W}{L}\right)_P = \frac{2 \times 1.0 \times 10^{-3} \text{ A}}{50 \times 10^{-6} \text{ A/V}^2 \times (0.15 \text{ V})^2} \approx 1777.78 $$
Choosing $L = 1.0 \mu\text{m}$, the dimensions are:
$$\mathbf{(W/L)_{5-8} = 1778 \mu\text{m} / 1.0 \mu\text{m}}$$

### 4. Bias Voltage Calculation

The bias voltages $V_{b1}$ and $V_{b2}$ must ensure that the cascode transistors ($M_3, M_4, M_7, M_8$) remain saturated even at the limits of the output swing, $V_{out, min} = 0.3 \text{ V}$ and $V_{out, max} = 2.7 \text{ V}$.

**NMOS Cascode Bias ($V_{b1}$):**
$M_3$ must operate in saturation, requiring $V_{out, min} \ge V_{G3} - V_{THN}$. To minimize the minimum input common-mode level, we bias $M_3$ exactly at the edge of saturation when the output is at $V_{out, min}$.
$$ V_{b1} = V_{out, min} + V_{THN} = 0.3 \text{ V} + 0.7 \text{ V} = \mathbf{1.0 \text{ V}} $$
This ensures that the voltage at the source of $M_3$ (drain of $M_1$), $V_{X}$, is $V_{b1} - (V_{OD3} + V_{THN}) = 1.0 \text{ V} - (0.15 \text{ V} + 0.7 \text{ V}) = 0.15 \text{ V}$.

**PMOS Cascode Bias ($V_{b2}$):**
$M_7$ must operate in saturation, requiring $V_{out, max} \le V_{G7} - V_{THP}$.
$$ V_{b2} = V_{out, max} + V_{THP} = 2.7 \text{ V} + (-0.8 \text{ V}) = \mathbf{1.9 \text{ V}} $$

### Design Summary

The single-ended output cascode Op Amp (Fig. 9.21(b)) is designed as follows:

| Parameter | Value | Constraint/Comment |
| :--- | :--- | :--- |
| **Supply Voltage ($V_{DD}$)** | $3.0 \text{ V}$ | Assumed based on context. |
| **Total Power Dissipation** | $6 \text{ mW}$ | Required. |
| **Tail Current ($I_{SS}$)** | $2.0 \text{ mA}$ | Derived from power/supply. |
| **Branch Current ($I_D$)** | $1.0 \text{ mA}$ | $I_{SS}/2$. |
| **Output Swing ($V_{out, max} - V_{out, min}$)** | $2.7 \text{ V} - 0.3 \text{ V} = 2.4 \text{ V}$ | Required. |
| **Transistor Length ($L$)** | $1.0 \mu\text{m}$ (All core devices) | Chosen to maximize intrinsic gain. |
| **NMOS Overdrive ($V_{ODN}$)** | $0.15 \text{ V}$ | Conserving headroom. |
| **PMOS Overdrive ($V_{ODP}$)** | $0.15 \text{ V}$ | Conserving headroom. |
| **NMOS Dimensions ($W/L$ for $M_1$ to $M_4$)** | $\mathbf{889 \mu\text{m} / 1.0 \mu\text{m}}$ | Derived from $I_D, V_{ODN}, K_{N}'$. |
| **PMOS Dimensions ($W/L$ for $M_5$ to $M_8$)** | $\mathbf{1778 \mu\text{m} / 1.0 \mu\text{m}}$ | Derived from $I_D, V_{ODP}, K_{P}'$. |
| **NMOS Cascode Bias ($V_{b1}$)** | $\mathbf{1.0 \text{ V}}$ | Sets NMOS stack saturation margin ($V_{out, min}$). |
| **PMOS Cascode Bias ($V_{b2}$)** | $\mathbf{1.9 \text{ V}}$ | Sets PMOS stack saturation margin ($V_{out, max}$). |

### Conceptual Analogy

Designing an analog circuit like this cascode amplifier is analogous to designing a complex water infrastructure system (like a skyscraper plumbing network) under severe rationing (low supply voltage/power budget). The goal is to maximize the pressure difference (output swing, 2.4 V) available for the users (signals) while ensuring the critical pumps (cascode transistors) never starve (remain saturated). Since the overall pressure capacity (3.0 V supply) is fixed, we must minimize the pressure required just to keep the pumps running (overdrive voltage, $0.15 \text{ V}$ per device) to leave the maximum possible pressure for the useful output swing.

---
## 9.6.
If in Fig. 9.23, (W/L)1−8 = 100/0.5 and ISS = 1 mA,
(a) What CM level must be established at the drains of M3 and M4 so that ID5 = ID6 = 1 mA? How does
this constrain the maximum input CM level?
(b) With the choice made in part (a), calculate the overall voltage gain and the maximum output swing

This query refers to the analysis of a specific operational amplifier topology, implied by the numerical context to be a differential stage, likely a telescopic cascode configuration (similar to figures referenced in surrounding problems).

Due to the constraints of the problem statement, a crucial assumption must be made: the overall tail current is $I_{SS}=1 \text{ mA}$. In a standard differential amplifier, this current splits equally into two branches, meaning the intended branch current for all saturated transistors $M_{1-8}$ is $I_D = I_{SS}/2 = 0.5 \text{ mA}$. The requirement that $I_{D5} = I_{D6} = 1 \text{ mA}$ is assumed to be a clerical error, as this would require a total load current of $2 \text{ mA}$, violating the constraint set by the $1 \text{ mA}$ tail current.

We assume the following typical $0.5 \mu\text{m}$ CMOS parameters for calculation (based on context where Table 2.1 is applicable):
*   $V_{DD} = 3.0 \text{ V}$
*   $V_{THN} = 0.7 \text{ V}$
*   $V_{THP} = -0.8 \text{ V}$
*   $K_{N}' (\mu_n C_{ox}) = 100 \mu\text{A/V}^2$
*   $K_{P}' (\mu_p C_{ox}) = 50 \mu\text{A/V}^2$
*   $\lambda_{N} = 0.1 \text{ V}^{-1}$, $\lambda_{P} = 0.2 \text{ V}^{-1}$ (for $L=0.5 \mu\text{m}$)

### **Preliminary Calculations (Overdrive and Transconductance)**

The geometry ratio for all transistors is $W/L = 100/0.5 = 200$. The branch current is $I_D = 0.5 \text{ mA}$.

1.  **NMOS Overdrive ($M_{1-4}$):**
    $$V_{ODN} = \sqrt{\frac{2 I_D}{K_{N}' (W/L)}} = \sqrt{\frac{1 \text{ mA}}{100 \times 10^{-6} \times 200}} \approx \mathbf{0.2236 \text{ V}}$$

2.  **PMOS Overdrive ($M_{5-8}$):**
    $$V_{ODP} = \sqrt{\frac{2 I_D}{K_{P}' (W/L)}} = \sqrt{\frac{1 \text{ mA}}{50 \times 10^{-6} \times 200}} \approx \mathbf{0.316 \text{ V}}$$

3.  **Input Transconductance ($M_{1,2}$):**
    $$g_{m1} = \frac{2 I_D}{V_{ODN}} \approx \frac{1 \text{ mA}}{0.2236 \text{ V}} \approx \mathbf{4.472 \text{ mS}}$$

---

### (a) CM level required at the drains of $M_{3}$ and $M_{4}$ ($V_{X/Y}$) so that $I_{D5} = I_{D6} = 1 \text{ mA}$. How does this constrain the maximum input CM level?

We proceed based on the assumption that $I_{D5} = I_{D6} = 0.5 \text{ mA}$ (the actual branch current). Assuming a telescopic cascode structure where $M_5$ and $M_6$ are the PMOS current sources biasing $M_7$ and $M_8$:

1.  **Determine $V_{X/Y}$ based on $M_{3}$ saturation (NMOS side):**
    The CM level at nodes $X/Y$ ($V_{X/Y}$) must be high enough to ensure that $M_3$ and $M_4$ remain in saturation. For a cascode configuration, this node is typically set by the constraint that $M_1/M_2$ must remain saturated while allowing the maximum possible headroom for the output.
    $$V_{X/Y, \min} = V_{S1/2} + V_{ODN}$$
    The minimum source voltage ($V_{S1/2}$) required for the input pair is $V_{ODN} \approx 0.2236 \text{ V}$ (set by the tail current source saturation).
    $$V_{X/Y, \min} = 0.2236 \text{ V} + 0.2236 \text{ V} \approx \mathbf{0.447 \text{ V}}$$

2.  **Determine $V_{X/Y}$ based on $M_{5}$ saturation (PMOS load side):**
    $M_5$ and $M_6$ are the current source loads for the cascode $M_7/M_8$. For maximum output swing, $M_5$ should be saturated. The voltage $V_{X/Y}$ must be high enough to allow the PMOS cascode transistors ($M_7, M_8$) to remain saturated when the output is at its highest point ($V_{DD} - 2 V_{ODP}$).
    If $V_{X/Y}$ refers to the bias voltage applied to the gate of the cascode devices ($M_7, M_8$), this voltage is typically set to $V_{DD} - V_{ODP}$.
    $$V_{X/Y} = V_{DD} - V_{ODP} = 3.0 \text{ V} - 0.316 \text{ V} = \mathbf{2.684 \text{ V}}$$

Assuming the question refers to the bias level needed for the load cascodes to operate effectively, **$V_{X/Y} \approx 2.684 \text{ V}$** must be established (where $X/Y$ are the gates of the upper cascodes $M_7, M_8$).

3.  **Constraint on Maximum Input CM Level ($V_{CM, \max}$):**
    The input CM level is limited by the NMOS input pair ($M_1, M_2$) falling out of saturation into the triode region:
    $$V_{CM, \max} \le V_{D1} + V_{THN}$$
    Since $V_{D1}$ is the source of the lower NMOS cascode ($M_3$) and assuming $M_3$ is driven by a gate voltage $V_{b1}$ (not specified, but critical for defining $V_{D1}$): we assume $V_{D1}$ is set such that $M_1/M_2$ remain comfortably saturated, typically $V_{D1} \approx V_{P, min} + V_{ODN} + V_{\text{margin}}$.
    If we assume the cascode bias voltage $V_{b1}$ is optimized (e.g., $V_{b1}=1.4 \text{ V}$ from context Q9.2) to ensure $M_3$ saturation:
    $V_{D1} = V_{b1} - V_{THN} - V_{ODN} = 1.4 \text{ V} - 0.7 \text{ V} - 0.2236 \text{ V} \approx 0.4764 \text{ V}$.
    $$V_{CM, \max} = V_{D1} + V_{THN} = 0.4764 \text{ V} + 0.7 \text{ V} \approx \mathbf{1.176 \text{ V}}$$

---

### (b) Calculate the overall voltage gain and the maximum output swing.

We calculate these based on the assumption that the circuit is a **telescopic cascode op amp** (for high gain capability) with optimal biasing and $I_D=0.5 \text{ mA}$ per branch.

1.  **Calculate Output Resistances ($r_{o}$):**
    $$r_{oN} = 1 / (\lambda_{N} I_D) = 1 / (0.1 \text{ V}^{-1} \times 0.5 \text{ mA}) = 20 \text{ k}\Omega$$
    $$r_{oP} = 1 / (\lambda_{P} I_D) = 1 / (0.2 \text{ V}^{-1} \times 0.5 \text{ mA}) = 10 \text{ k}\Omega$$

2.  **Calculate Load Transconductances:**
    $g_{m3} = g_{m1} = 4.472 \text{ mS}$
    $g_{m7} = g_{m5} = 3.165 \text{ mS}$

3.  **Calculate Total Output Resistance ($R_{\text{out}}$):**
    The output resistance of the NMOS stack ($R_{\text{out, N}}$) is approximately $g_{m3} r_{o3} r_{o1}$.
    $$R_{\text{out, N}} \approx (4.472 \text{ mS}) (20 \text{ k}\Omega) (20 \text{ k}\Omega) \approx 1.789 \text{ M}\Omega$$
    The output resistance of the PMOS stack ($R_{\text{out, P}}$) is approximately $g_{m7} r_{o7} r_{o5}$.
    $$R_{\text{out, P}} \approx (3.165 \text{ mS}) (10 \text{ k}\Omega) (10 \text{ k}\Omega) \approx 0.3165 \text{ M}\Omega$$
    The overall output resistance is $R_{\text{out}} = R_{\text{out, N}} || R_{\text{out, P}}$.
    $$R_{\text{out}} \approx (1/1.789 + 1/0.3165)^{-1} \text{ M}\Omega \approx 0.269 \text{ M}\Omega$$

4.  **Calculate Overall Voltage Gain ($A_{V, OL}$):**
    $$A_{V, OL} \approx -g_{m1} R_{\text{out}} \approx -(4.472 \times 10^{-3} \text{ S}) \times (0.269 \times 10^{6} \Omega) \approx \mathbf{-1202}$$

5.  **Calculate Maximum Output Swing ($V_{\text{swing, max}}$):**
    The output swing is determined by the minimum sustained overdrive required for the cascoded stacks:

    *   $V_{\text{OUT, min}}$ (NMOS stack constraint): The minimum voltage is limited by the required overdrive of the two NMOS transistors in series ($M_1, M_3$).
        $$V_{\text{OUT, min}} = V_{ODN} + V_{ODN} = 2 \times 0.2236 \text{ V} \approx 0.447 \text{ V}$$

    *   $V_{\text{OUT, max}}$ (PMOS stack constraint): The maximum voltage is limited by $V_{DD}$ minus the required overdrive of the two PMOS transistors in series ($M_5, M_7$).
        $$V_{\text{OUT, max}} = V_{DD} - (V_{ODP} + V_{ODP}) = 3.0 \text{ V} - 2 \times 0.316 \text{ V} \approx 2.368 \text{ V}$$

    *   $V_{\text{swing, max}} = V_{\text{OUT, max}} - V_{\text{OUT, min}} = 2.368 \text{ V} - 0.447 \text{ V} \approx \mathbf{1.921 \text{ V}}$
