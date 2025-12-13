# Open Book Exam Tactics: Razavi CMOS Analog Design (Topics 1-5)

This guide provides specific tactics, key equations, and "where to look" strategies for the first 5 major topics of the ECE 587 syllabus, based on Razavi's *Design of Analog CMOS Integrated Circuits*.

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
