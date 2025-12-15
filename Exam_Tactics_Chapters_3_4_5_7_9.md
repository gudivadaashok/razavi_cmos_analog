# Open Book Exam Tactics: Chapters 3, 4, 5, 7, 9

This guide provides specific tactics, key equations, and "where to look" strategies for the requested chapters from Razavi's *Design of Analog CMOS Integrated Circuits*.

---

## 1. Single-Stage Amplifiers (Chapter 3)
**Exam Strategy:** First, identify the topology (Common Source, Common Gate, or Source Follower). Second, identify the load (Resistor, Diode-connected, or Current Source).

### Key Locations
*   **Table 3.1 (or Chapter Summary):** Look for the table comparing $R_{in}$, $R_{out}$, and $A_v$ for CS, CG, and CD stages.
*   **Section 3.3.6:** Equations for Common Source with Source Degeneration.

### The "Golden Equations"
*   **Common Source (Current Source Load):**
    $$A_v = -g_m (r_{o,n} || r_{o,p})$$
*   **Common Source (Diode-Connected Load):**
    $$A_v \approx -\sqrt{\frac{(W/L)_{in}}{(W/L)_{load}}}$$
*   **Source Follower (Common Drain):**
    $$A_v = \frac{g_m R_L}{1 + (g_m + g_{mb}) R_L} \approx \frac{g_m}{g_m + g_{mb}}$$
    $$R_{out} \approx \frac{1}{g_m + g_{mb}}$$
*   **Common Gate:**
    $$A_v = (g_m + g_{mb}) R_D$$
    $$R_{in} \approx \frac{1}{g_m + g_{mb}}$$
*   **Source Degeneration:**
    $$G_m = \frac{g_m}{1 + g_m R_S}$$
    $$A_v = - \frac{g_m R_D}{1 + g_m R_S} \approx -\frac{R_D}{R_S}$$

### Common Exam Traps
*   **Body Effect:** In Source Followers and Common Gate stages, the Source is NOT at ground. You must include $g_{mb}$ (body transconductance) in the gain and impedance formulas.
*   **Impedance Looking Up vs. Down:** Looking into the Drain gives $r_o$ (or boosted $r_o$). Looking into the Source gives $1/g_m$.
*   **Loading:** If a stage drives a low impedance load (like another Common Gate stage), the gain drops significantly. Always check the effective load resistance.

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
*   **Input Range:** If asked for ICMR (Input Common Mode Range), check when the tail source enters triode (Min Input) or when input transistors enter triode (Max Input).

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

## 4. Noise (Chapter 7)
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

## 5. Operational Amplifiers (Chapter 9)
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
