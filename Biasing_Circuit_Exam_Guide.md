# Exam Guide: MOS Biasing with Resistive Divider
**Reference:** Razavi, *Design of Analog CMOS Integrated Circuits*, Chapter 5 (Figure 5.2).

This guide provides a structured approach to solving biasing problems, specifically for the resistive divider topology, suitable for an open-book exam setting.

---

## 1. Circuit Identification
*   **Visual Cue:** A single NMOS transistor ($M_1$) with its gate connected to a node between two resistors ($R_1, R_2$) stacked between $V_{DD}$ and Ground.
*   **Function:** The resistors form a voltage divider to set a fixed Gate-Source Voltage ($V_{GS}$).
*   **Key Assumption:** The gate current is zero ($I_G = 0$), so the divider is unloaded.

---

## 2. The "Golden Equations"
*Locate these in Chapter 2 (Basic Physics) or Chapter 5 (Biasing).*

### A. DC Operating Point (Saturation)
$$I_D = \frac{1}{2} \mu_n C_{ox} \frac{W}{L} (V_{GS} - V_{TH})^2 (1 + \lambda V_{DS})$$
*If $\lambda = 0$, omit the last term.*

### B. Biasing Equation (Voltage Divider)
$$V_{GS} = V_{DD} \cdot \frac{R_2}{R_1 + R_2}$$

### C. Transconductance ($g_m$)
Used for sensitivity calculations.
$$g_m = \mu_n C_{ox} \frac{W}{L} (V_{GS} - V_{TH}) = \frac{2 I_D}{V_{GS} - V_{TH}}$$

---

## 3. Step-by-Step Solution Strategy

### Part (a): Determine Resistor Ratio ($R_2/R_1$)
**Goal:** Find the $R_2/R_1$ required to support a specific $I_{out}$.
1.  **Invert the $I_D$ Equation:** Solve for the required Overdrive Voltage ($V_{OV} = V_{GS} - V_{TH}$).
    $$V_{GS} - V_{TH} = \sqrt{\frac{2 I_D}{\mu_n C_{ox} (W/L)}}$$
2.  **Find $V_{GS}$:** Add $V_{TH}$ to the result.
    $$V_{GS} = V_{OV} + V_{TH}$$
3.  **Solve Divider Ratio:** Use the voltage divider equation.
    $$\frac{V_{GS}}{V_{DD}} = \frac{R_2}{R_1 + R_2}$$
    *Algebraic Tip:* If $\frac{R_2}{R_1+R_2} = k$, then $\frac{R_2}{R_1} = \frac{k}{1-k}$.

### Part (b): Sensitivity to Supply Voltage ($S_{V_{DD}}^{I_{out}}$)
**Goal:** Quantify how much $I_{out}$ changes when $V_{DD}$ changes.
1.  **Definition:** $S = \frac{V_{DD}}{I_{out}} \frac{\partial I_{out}}{\partial V_{DD}}$.
2.  **Chain Rule:**
    $$\frac{\partial I_{out}}{\partial V_{DD}} = \frac{\partial I_{out}}{\partial V_{GS}} \cdot \frac{\partial V_{GS}}{\partial V_{DD}}$$
    *   $\frac{\partial I_{out}}{\partial V_{GS}} = g_m$
    *   $\frac{\partial V_{GS}}{\partial V_{DD}} = \frac{R_2}{R_1+R_2} = \frac{V_{GS}}{V_{DD}}$
3.  **Result:**
    $$S = \frac{V_{DD}}{I_{out}} \left( g_m \frac{V_{GS}}{V_{DD}} \right) = \frac{g_m V_{GS}}{I_{out}} = \frac{2 V_{GS}}{V_{GS} - V_{TH}}$$

### Part (c): Sensitivity to Threshold Voltage ($V_{TH}$)
**Goal:** Calculate $\Delta I_{out}$ for a given $\Delta V_{TH}$.
1.  **Concept:** $V_{TH}$ appears inside the square term. Increasing $V_{TH}$ decreases the overdrive, reducing current.
2.  **Formula:**
    $$\frac{\partial I_{out}}{\partial V_{TH}} = -g_m$$
3.  **Calculation:**
    $$\Delta I_{out} \approx -g_m \cdot \Delta V_{TH}$$

### Part (d): Temperature Dependence
**Goal:** Calculate $\Delta I_{out}$ when $T$ changes.
1.  **Identify Variable:** Usually mobility $\mu_n$ is the dominant temperature-dependent factor.
2.  **Relation:** $\mu_n(T) \propto T^{-1.5}$.
3.  **Ratio Method:**
    $$\frac{I_{new}}{I_{old}} = \left( \frac{T_{new}}{T_{old}} \right)^{-1.5}$$
4.  **Calculation:**
    $$I_{new} = I_{old} \cdot \left( \frac{T_{new}}{T_{old}} \right)^{-1.5}$$

### Part (e): Worst-Case Analysis
**Goal:** Find the maximum possible deviation.
1.  **List all changes:** Calculate $\Delta I$ for each parameter ($V_{DD}, V_{TH}, T$) individually.
2.  **Check Signs:**
    *   $\Delta V_{DD} > 0 \rightarrow \Delta I > 0$
    *   $\Delta V_{TH} < 0 \rightarrow \Delta I > 0$ (Lower threshold = More current)
    *   $\Delta T < 0 \rightarrow \Delta I > 0$ (Lower temp = Higher mobility = More current)
3.  **Sum Absolute Worst Case:** Add the magnitudes of the changes in the direction that maximizes the error.

---

## 4. Open Book Exam Tactics

1.  **Pattern Match:** Don't read the whole chapter. Look at the exam circuit, then flip through Chapter 4 (Diff Amps) or Chapter 5 (Biasing) until you see a matching schematic.
2.  **Verify Assumptions:** Check the problem statement for "Saturation", "Triode", or "Subthreshold". Ensure the formula you pick from the book matches this region.
3.  **Unit Check:**
    *   $I_D$: usually $\mu A$ or $mA$.
    *   $k_n = \mu C_{ox}$: usually $\mu A/V^2$.
    *   $g_m$: usually $mS$ (mA/V) or $\mu S$.
    *   Ensure you don't mix $mA$ and $\mu A$ in the same equation.
