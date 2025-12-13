# Open Book Exam Tactics: Razavi CMOS Analog Design (Topics 1-5)

This guide provides specific tactics, key equations, and "where to look" strategies for the first 5 major topics of the ECE 587 syllabus, based on Razavi's *Design of Analog CMOS Integrated Circuits*.




Here is a concise, step-by-step guide on how to use your open textbook to solve exam problems efficiently:

1. Pattern Match (Scan & Identify)
Look at the Circuit: Identify the key blocks (e.g., Single Transistor, Differential Pair, Current Mirror, Cascode).
Find the Figure: Flip to the relevant chapter (Ch 2 for Devices, Ch 4 for Diff Amps, Ch 5 for Mirrors) and find the diagram that looks like your problem.
Check Assumptions: Does the problem say "Saturation"? "
λ
=
0
λ=0"? Match these conditions to the text around the figure.
2. Retrieve "Golden Equations"
Don't Derive: Copy the final formulas directly from the book or your summary sheet.
Key Formulas to Grab:
DC: 
I
D
I 
D
​
  equation (Saturation vs. Triode), 
V
G
S
V 
GS
​
  calculation.
AC: Gain (
A
v
A 
v
​
 ), Transconductance (
g
m
g 
m
​
 ), Output Resistance (
R
o
u
t
R 
out
​
 ).
Watch Units: Ensure 
μ
C
o
x
μC 
ox
​
  and 
W
/
L
W/L units match (usually 
μ
A
/
V
2
μA/V 
2
  and 
μ
m
/
μ
m
μm/μm).
3. Execute & Verify
Plug in Numbers: Substitute the given values into the retrieved equations.
Check Regions (The Trap):
Calculate 
V
O
V
=
V
G
S
−
V
T
H
V 
OV
​
 =V 
GS
​
 −V 
TH
​
 .
Verify 
V
D
S
>
V
O
V
V 
DS
​
 >V 
OV
​
  for Saturation.
If 
V
D
S
<
V
O
V
V 
DS
​
 <V 
OV
​
 , stop! You must use the Triode equation.
Body Effect: If the Source is not at Ground (for NMOS), remember 
V
T
H
V 
TH
​
  will be higher than given. Mention this even if you don't calculate it fully.

---

## 1. CMOS Device Modeling (Chapter 2)
**Exam Strategy:** Most problems here ask for $I_D$, $g_m$, $r_o$, or capacitances. Watch out for "Triode vs. Saturation" traps.

### Key Locations
*   **Table 2.1 (or similar summary):** Look for the table summarizing $I_D$, $g_m$, and $r_o$ in different regions.
*   **Section 2.4 (Small Signal Model):** For $C_{gs}$, $C_{gd}$, $C_{db}$ formulas.

### The "Golden Equations"
*   **Saturation ($V_{DS} \ge V_{GS} - V_{TH}$):**
    $$I_D = \frac{1}{2} \mu C_{ox} \frac{W}{L} (V_{GS} - V_{TH})^2 (1 + \lambda V_{DS})$$
*   **Triode ($V_{DS} < V_{GS} - V_{TH}$):**
    $$I_D = \mu C_{ox} \frac{W}{L} \left[ (V_{GS} - V_{TH})V_{DS} - \frac{1}{2}V_{DS}^2 \right]$$
*   **Transconductance ($g_m$):**
    $$g_m = \sqrt{2 \mu C_{ox} \frac{W}{L} I_D} = \frac{2 I_D}{V_{OV}}$$
*   **Output Resistance ($r_o$):**
    $$r_o \approx \frac{1}{\lambda I_D}$$

### Common Exam Traps
*   **Region Check:** Always calculate $V_{OV} = V_{GS} - V_{TH}$ and compare it to $V_{DS}$. If $V_{DS} < V_{OV}$, you MUST use the Triode formula.
*   **Body Effect:** If Source $\neq$ Ground (for NMOS), $V_{TH}$ increases. Look for the equation: $V_{TH} = V_{TH0} + \gamma (\sqrt{2\phi_F + V_{SB}} - \sqrt{2\phi_F})$.

---

## 2. Differential Amplifiers (Chapter 4)
**Exam Strategy:** Identify if it's a "Resistive Load" or "Active Load" (Current Mirror).

### Key Locations
*   **Figure 4.8:** Basic Diff Pair with Resistors.
*   **Figure 4.28:** Diff Pair with Active Current Mirror Load (5-Transistor OTA).

### The "Golden Equations"
*   **Differential Gain (Resistive Load):**
    $$A_v = -g_m R_D$$
*   **Differential Gain (Active Load):**
    $$A_v = g_{m1} (r_{o2} || r_{o4})$$
    *(Note: $r_{o2}$ is input transistor, $r_{o4}$ is load transistor)*
*   **Common Mode Gain ($A_{CM}$):**
    $$A_{CM} \approx \frac{-R_D}{2 R_{SS}}$$
    *(Where $R_{SS}$ is the tail current source resistance)*
*   **CMRR:**
    $$CMRR = \left| \frac{A_{DM}}{A_{CM}} \right| \approx g_m R_{SS}$$

### Common Exam Traps
*   **Half-Circuit:** If the circuit is symmetric, cut it in half! Analyze just one side to find $A_v$.
*   **Input Range:** If asked for ICMR, check when the tail source enters triode (Min Input) or when input transistors enter triode (Max Input).

---

## 3. Passive and Active Current Mirrors (Chapter 5)
**Exam Strategy:** Focus on "Copying Accuracy" and "Output Resistance".

### Key Locations
*   **Figure 5.1/5.4:** Basic Current Mirror.
*   **Figure 5.18:** Cascode Current Mirror.

### The "Golden Equations"
*   **Current Ratio (Basic):**
    $$\frac{I_{out}}{I_{ref}} = \frac{(W/L)_{out}}{(W/L)_{ref}}$$
*   **Output Resistance (Cascode):**
    $$R_{out} \approx g_{m2} r_{o2} r_{o1}$$
    *(The "Shielding" effect multiplies $r_o$ by $g_m r_o$)*
*   **Systematic Error:**
    $$V_{DS,ref} \neq V_{DS,out} \implies \text{Mismatch due to } \lambda$$

### Common Exam Traps
*   **Headroom:** Cascode mirrors need more voltage. $V_{min} = V_{TH} + 2V_{OV}$ (Standard) or $2V_{OV}$ (Low-Voltage Cascode).

---

## 4. Operational Amplifiers (Chapter 9)
**Exam Strategy:** Usually involves "Folded Cascode" or "Telescopic" topologies.

### Key Locations
*   **Figure 9.6:** Telescopic Cascode OpAmp.
*   **Figure 9.14:** Folded Cascode OpAmp.

### The "Golden Equations"
*   **Gain (Telescopic/Folded):**
    $$A_v \approx g_{m,in} \cdot (R_{out,N} || R_{out,P})$$
    $$R_{out} \approx (g_m r_o) \cdot r_o$$
*   **Unity Gain Bandwidth ($GBW$):**
    $$\omega_u = \frac{g_{m,in}}{C_L}$$
*   **Slew Rate:**
    $$SR = \frac{I_{SS}}{C_L}$$

### Common Exam Traps
*   **Swing:** Telescopic has poor output swing. Folded Cascode has better swing but consumes more power.
*   **Input Range:** Folded Cascode allows input to go near (or beyond) one rail.

---

## 5. Noise (Chapter 7)
**Exam Strategy:** Add noise powers ($V_n^2$), not voltages.

### Key Locations
*   **Table 7.1:** Noise of Resistors and MOSFETs.

### The "Golden Equations"
*   **Resistor Noise:**
    $$\overline{V_n^2} = 4 k T R \Delta f$$
*   **MOS Thermal Noise:**
    $$\overline{I_n^2} = 4 k T \left( \frac{2}{3} g_m \right) \Delta f$$
*   **Input Referred Noise (CS Stage):**
    $$\overline{V_{n,in}^2} = \frac{4kT (2/3 g_m)}{g_m^2} = \frac{8kT}{3 g_m}$$
    *(To reduce noise, increase $g_m$!)*
*   **Flicker Noise (1/f):**
    $$\overline{V_n^2} = \frac{K}{C_{ox} WL f}$$
    *(To reduce flicker noise, increase Area $WL$)*

### Common Exam Traps
*   **Bandwidth:** Noise is usually integrated over bandwidth. $\text{Total Noise} = \text{Noise Density} \times \sqrt{BW}$ (if flat).
*   **Dominant Source:** In a diff pair, the input transistors and the current source loads dominate. The tail source noise is common-mode (rejected) if the circuit is balanced.

---
---

# Open Book Exam Strategy Demo
**Problem Used:** Razavi Problem 5.3 (Differential Pair with Current Mirror Biasing)

This section demonstrates how to use the **"Open Book Exam Tactics"** guide above to solve a specific problem efficiently under exam conditions.

## The Problem Statement
**Circuit:** Fig. 5.8 (Differential Pair biased by a Current Mirror).
**Parameters:** $(W/L)_N = (W/L)_P = 20$ (Effective ratio $10/0.5$), $I_{REF} = 100 \mu A$.
**Input:** $V_{in,CM} = 1.3 V$.
**Task (a):** Assuming $\lambda = 0$, calculate $V_P$ (Tail Node Voltage) and the drain voltage of the PMOS loads.

## Step 1: Pattern Match (Scan the Guide)
**Action:** Look at the circuit diagram.
*   **Observation:** It has a differential pair ($M_1, M_2$) and a current mirror ($M_{ref}, M_{tail}$).
*   **Guide Lookup:**
    *   Go to **Topic 2 (Differential Amplifiers)** for the Diff Pair structure.
    *   Go to **Topic 3 (Current Mirrors)** for the Biasing structure.

## Step 2: Retrieve "Golden Equations"
**Action:** Don't derive from scratch. Copy the relevant formulas from the guide/book.

*   **From Topic 3 (Current Mirrors):**
    *   *Concept:* "Current Ratio".
    *   *Equation:* $I_{tail} = I_{REF} \times \frac{(W/L)_{tail}}{(W/L)_{ref}}$.
    *   *Application:* Since sizes are equal, $I_{tail} = 100 \mu A$.

*   **From Topic 2 (Diff Amps):**
    *   *Concept:* "Half-Circuit" / Symmetry.
    *   *Tactic:* "If symmetric, cut it in half."
    *   *Application:* $I_{D1} = I_{D2} = I_{tail} / 2 = 50 \mu A$.

*   **From Topic 1 (Device Modeling):**
    *   *Concept:* "Saturation Current" (to find $V_{GS}$).
    *   *Equation:* $I_D = \frac{1}{2} \mu C_{ox} \frac{W}{L} (V_{GS} - V_{TH})^2$.
    *   *Inverted Form (Useful for Exam):* $V_{GS} = V_{TH} + \sqrt{\frac{2 I_D}{\mu C_{ox} (W/L)}}$.

## Step 3: Execute the Calculation
**Action:** Plug numbers into the Golden Equations.

1.  **Calculate Overdrive ($V_{OV}$) for Input Pair:**
    *   $I_D = 50 \mu A$.
    *   $W/L = 20$.
    *   $\mu_n C_{ox} = 110 \mu A/V^2$ (Standard value).
    *   $V_{OV} = \sqrt{\frac{2 \cdot 50}{110 \cdot 20}} = \sqrt{\frac{100}{2200}} \approx \sqrt{0.045} \approx 0.21 V$.
    *   $V_{GS1} = 0.5 + 0.21 = 0.71 V$.

2.  **Calculate Tail Node Voltage ($V_P$):**
    *   Circuit KVL: $V_{in,CM} - V_{GS1} = V_P$.
    *   $V_P = 1.3 V - 0.71 V = \mathbf{0.59 V}$.

3.  **Calculate PMOS Diode Voltage:**
    *   PMOS is diode-connected (Gate = Drain).
    *   Current = $50 \mu A$.
    *   $V_{SG} = |V_{THP}| + \sqrt{\frac{2 I_D}{\mu_p C_{ox} (W/L)}}$.
    *   $V_{SG} = 0.5 + \sqrt{\frac{2 \cdot 50}{50 \cdot 20}} = 0.5 + \sqrt{0.1} \approx 0.5 + 0.316 = 0.816 V$.
    *   $V_{Drain} = V_{DD} - V_{SG} = 2.5 - 0.816 = \mathbf{1.68 V}$.

## Step 4: Check for "Exam Traps"
**Action:** Consult the "Common Exam Traps" section of the guide.

*   **Trap Check 1 (Region):** "Always calculate $V_{OV}$ and compare to $V_{DS}$."
    *   *Check $M_{tail}$:* $V_{DS} = V_P = 0.59 V$. $V_{OV,tail} \approx 0.2 V$. Since $0.59 > 0.2$, it is in **Saturation**. (Valid).
    *   *Check $M_1$:* $V_{Drain} = 1.68 V$. $V_{Source} = 0.59 V$. $V_{DS} = 1.09 V$. $V_{OV} = 0.21 V$. Saturation? Yes. (Valid).

*   **Trap Check 2 (Body Effect):** "If Source $\neq$ Ground, $V_{TH}$ increases."
    *   *Observation:* $M_1$ source is at $V_P = 0.59 V$.
    *   *Correction:* If the problem required high precision, we would recalculate $V_{TH1}$ using the body effect formula from Topic 1. Since the problem didn't specify $\gamma$, we assume $\gamma=0$ or ignore it for the first pass.

## Summary
By using the guide, we:
1.  Immediately identified the relevant chapters (2 and 3).
2.  Grabbed the exact equations needed ($I_D$ inversion, Current Ratio).
3.  Verified our answer against common traps (Saturation check).

---

# General Open Book Exam Strategy (3-Step Process)

## 1. Pattern Match (Scan & Identify)
*   **Look at the Circuit:** Identify the key blocks (e.g., Single Transistor, Differential Pair, Current Mirror, Cascode).
*   **Find the Figure:** Flip to the relevant chapter (Ch 2 for Devices, Ch 4 for Diff Amps, Ch 5 for Mirrors) and find the diagram that looks like your problem.
*   **Check Assumptions:** Does the problem say "Saturation"? Or "Lambda = 0" ($\lambda = 0$)? Match these conditions to the text around the figure.

## 2. Retrieve "Golden Equations"
*   **Don't Derive:** Copy the final formulas directly from the book or your summary sheet.
*   **Key Formulas to Grab:**
    *   **DC:** Current $I_D$ (Saturation vs. Triode), Gate-Source Voltage $V_{GS}$.
    *   **AC:** Gain $A_v$, Transconductance $g_m$, Output Resistance $R_{out}$.
*   **Watch Units:** Ensure $\mu C_{ox}$ and $W/L$ units match (usually $\mu A/V^2$ and $\mu m / \mu m$).

## 3. Execute & Verify
*   **Plug in Numbers:** Substitute the given values into the retrieved equations.
*   **Check Regions (The Trap):**
    *   Calculate Overdrive: $V_{OV} = V_{GS} - V_{TH}$.
    *   Verify Saturation: $V_{DS} > V_{OV}$.
    *   If $V_{DS} < V_{OV}$, stop! You must use the Triode equation.
*   **Body Effect:** If the Source is not at Ground (for NMOS), remember $V_{TH}$ will be higher than given. Mention this even if you don't calculate it fully.
