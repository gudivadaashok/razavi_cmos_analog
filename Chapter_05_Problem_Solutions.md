# Chapter 5: Current Mirrors and Biasing - Problem Solutions

This document provides step-by-step solutions and guidance for the selected problems from Chapter 5 of Razavi's *Design of Analog CMOS Integrated Circuits*.

---

## Problem 5.1 (Biasing with Resistive Divider)
**Given:** Fig. 5.2, $(W/L)_1 = 50/0.5 = 100$, $\lambda = 0$, $I_{out} = 0.5 mA$, $M_1$ saturated.
**Assumptions:** $V_{DD} = 2.5V$, $\mu_n C_{ox} = 110 \mu A/V^2$, $V_{TH} = 0.5V$.

### (a) Determine $R_2/R_1$
1.  **Find $V_{GS}$:**
    $$I_D = \frac{1}{2} \mu_n C_{ox} (W/L) (V_{GS} - V_{TH})^2$$
    $$500\mu A = \frac{1}{2} (110) (100) (V_{GS} - 0.5)^2$$
    $$(V_{GS} - 0.5)^2 = \frac{500}{5500} \approx 0.0909 \implies V_{GS} \approx 0.8V$$
2.  **Find Ratio:**
    $$V_{GS} = V_{DD} \frac{R_2}{R_1+R_2} \implies 0.8 = 2.5 \frac{R_2}{R_1+R_2}$$
    $$\frac{R_2}{R_1+R_2} = 0.32 \implies R_2 = 0.32 R_1 + 0.32 R_2 \implies 0.68 R_2 = 0.32 R_1$$
    $$\frac{R_2}{R_1} \approx 0.47$$

### (b) Sensitivity to $V_{DD}$
$$S = \frac{V_{DD}}{I_{out}} \frac{\partial I_{out}}{\partial V_{DD}} = \frac{2 V_{GS}}{V_{GS} - V_{TH}} = \frac{2(0.8)}{0.3} \approx 5.33$$

### (c) Change if $\Delta V_{TH} = 50 mV$
$$\Delta I_{out} \approx -g_m \Delta V_{TH}$$
$$g_m = \frac{2 I_D}{V_{OV}} = \frac{1000 \mu A}{0.3 V} \approx 3.33 mA/V$$
$$\Delta I_{out} = -(3.33) (0.05) \approx -0.167 mA$$

### (d) Temperature Change (300K to 370K)
$$\mu \propto T^{-1.5} \implies I \propto T^{-1.5}$$
$$I_{new} = 0.5 \left(\frac{370}{300}\right)^{-1.5} \approx 0.365 mA$$
$$\Delta I = 0.365 - 0.5 = -0.135 mA$$

### (e) Worst Case Change
*   $\Delta V_{DD} (+10\%)$: $+0.266 mA$
*   $\Delta V_{TH} (-50mV)$: $+0.167 mA$
*   $\Delta T$ (Opposing): $-0.135 mA$
*   **Max Increase:** $+0.298 mA$ (if T change is forced).
*   **Max Decrease:** $-0.568 mA$ (Current drops to 0).

---

## Problem 5.2 (Fig 5.7 - Basic Current Mirror)
**Task:** Sketch $I_{out}$ vs $V_{DD}$ (0 to 3V).
*   **Region 1 ($V_{DD} < V_{TH} + V_{OV}$):** $M_{ref}$ is off or in subthreshold. $I_{out} \approx 0$.
*   **Region 2 ($V_{DD} > V_{TH} + V_{OV}$):** $M_{ref}$ turns on. $I_{ref}$ flows. $V_{GS}$ is established.
*   **Region 3 ($V_{DS,out} < V_{OV}$):** If $V_{DD}$ is the supply for the output branch too, $M_{out}$ starts in triode and moves to saturation.
*   **Result:** $I_{out}$ rises from 0 and settles at $I_{ref}$ once $V_{DD}$ is high enough to keep $M_{out}$ in saturation.

---

## Problem 5.3 (Fig 5.8 - Diff Pair Biasing)
**Given:** $(W/L)_{N,P} = 10/0.5$, $I_{REF} = 100 \mu A$, $V_{in,CM} = 1.3V$.

### (a) $\lambda = 0$
*   **Current:** $I_{REF}$ is mirrored to the tail. $I_{tail} = I_{REF} = 100 \mu A$.
*   **$V_P$ (Tail Node):** $V_{in,CM} - V_{GS1} = V_P$.
    $$50 \mu A = \frac{1}{2} \mu_n C_{ox} (20) (V_{GS} - V_{TH})^2 \implies V_{GS} \approx V_{TH} + 0.2V$$
    $$V_P = 1.3 - (0.5 + 0.2) = 0.6V$$

### (b) With $\lambda$
*   $I_{tail}$ will differ from $I_{REF}$ because $V_{DS,ref} \neq V_{DS,tail}$.
*   $V_{DS,ref} = V_{GS,ref}$. $V_{DS,tail} = V_P$.
*   Use $I_{tail} = I_{REF} \frac{1 + \lambda V_P}{1 + \lambda V_{GS,ref}}$.

---

## Problem 5.5 (Fig 5.12a - Cascode Mirror)
**Given:** $(W/L) = 80$, $I_{REF} = 0.3 mA$.

### (a) Determine $V_b$ for $V_X = V_Y$
*   For $V_X = V_Y$, the $V_{DS}$ of the bottom transistors must be equal.
*   $V_Y = V_{GS1}$.
*   $V_X$ is determined by $V_b - V_{GS3}$.
*   Set $V_b = V_{GS3} + V_{GS1} = 2 V_{GS}$.

### (b) Mismatch if $V_b$ deviates
*   If $V_b$ changes, $V_X$ changes.
*   $\Delta I / I \approx g_m r_o \Delta V_{DS}$ is incorrect for mirrors.
*   Use $\frac{\Delta I}{I} = \frac{\Delta V_{DS}}{r_o I} = \lambda \Delta V_{DS}$.
*   $\Delta V_{DS} = \Delta V_b$.
*   Mismatch $\approx \lambda \Delta V_b$.

---

## Problem 5.6 (Fig 5.18b - High Swing Cascode)
**Given:** Ratio 1:3 for top/bottom.

### (a) Determine $V_X$ and Range of $V_b$
*   $V_X$ is the drain of the bottom device.
*   For high swing, we want $V_X$ as low as possible ($V_{OV}$).
*   $V_b$ sets $V_X$. $V_X = V_b - V_{GS,cascode}$.
*   Range: $V_{GS} + V_{OV} \ge V_b \ge V_{TH} + 2V_{OV}$.

---

## Problem 5.7 (Fig 5.23a - Active Load Diff Pair)
**Given:** $I_{SS} = 0.5 mA$.

### (a) Small Signal Gain
*   $A_v = g_{m1} (r_{o2} || r_{o4})$.
*   Calculate $g_{m1}$ and $r_o$ using $I_D = 0.25 mA$.

### (b) Max Output Swing
*   **Max:** $V_{DD} - |V_{OD,4}|$.
*   **Min:** $V_{in,CM} - V_{TH1} + V_{OD1}$ (Input enters triode) OR $V_{min}$ determined by M2 entering triode ($V_{out} > V_{in,CM} - V_{TH}$).

---

## Problem 5.20 (Defect Resistance)
**Scenario:** Resistor $R_1$ appears.
*   If $R_1$ is in the tail: It degenerates the tail source, increasing CMRR but reducing headroom.
*   If $R_1$ is in the load: It reduces the output resistance of the mirror, killing the gain.
*   **Gain Calculation:** Replace $r_{o,load}$ with $r_{o,load} || R_1$.
    $$A_v = g_m (r_{o,driver} || r_{o,load} || R_1)$$

---

## Problem 5.21 (Digital Diff Pair)
**Question:** Why does $V_{min}$ depend on $V_{in,CM}$?
*   **Answer:** The output node can swing down until the input transistor enters the triode region.
*   Condition for Saturation: $V_{DS} \ge V_{GS} - V_{TH}$.
*   $V_D - V_S \ge V_G - V_S - V_{TH} \implies V_D \ge V_G - V_{TH}$.
*   Here, $V_D = V_{out}$ and $V_G = V_{in,CM}$.
*   Therefore, $V_{out,min} = V_{in,CM} - V_{TH}$.
*   As $V_{in,CM}$ goes up, the minimum allowable output voltage also goes up, reducing the swing.

---

## Problem 5.24 (Subthreshold)
**Given:** Eq 5.64 (Exponential current).

### (a) Deep Triode ($V_{DS} \ll V_T$)
*   $1 - \exp(-V_{DS}/V_T) \approx 1 - (1 - V_{DS}/V_T) = V_{DS}/V_T$.
*   $I_D \approx I_0 \exp(...) \cdot (V_{DS}/V_T)$.
*   $R_{on} = V_{DS}/I_D = \frac{V_T}{I_0 \exp(...)}$. (Linear Resistor).

### (b) Saturation ($V_{DS} \gg V_T$)
*   $\exp(-V_{DS}/V_T) \approx 0$.
*   $I_D = I_0 \exp(V_{GS}/ \eta V_T)$.
*   $g_m = \frac{\partial I_D}{\partial V_{GS}} = \frac{I_D}{\eta V_T}$. (Max possible $g_m/I_D$ ratio).

---

## Problem 5.26 (Supply Rejection)
**Circuit:** Fig 5.67 (Likely a self-biased reference or simple mirror).
*   **Concept:** How much does $I_{out}$ change if $V_{DD}$ changes?
*   **Analysis:** Draw the small signal model.
*   If it's a simple resistor-MOS mirror: $PSRR \approx \frac{1}{g_m R}$.
*   If it's a cascode: PSRR improves by factor of $g_m r_o$.
*   **Calculation:** Find $v_{out} / v_{dd}$ using KCL at the output node.
