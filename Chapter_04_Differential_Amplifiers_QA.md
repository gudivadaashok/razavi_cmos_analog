# Chapter 4: Differential Amplifiers - Questions and Answers

## 4.1 Single-Ended vs Differential

**Q1: What is the fundamental difference between single-ended and differential signals?**
**A1:** A single-ended signal is measured with respect to a fixed potential (usually ground). A differential signal is measured as the difference between two nodes that carry equal and opposite signal excursions around a fixed common-mode level.

**Q2: Why are differential circuits preferred in modern IC design despite the area penalty?**
**A2:** Differential circuits offer superior noise immunity. They reject "common-mode" noise (like supply noise or substrate coupling) that affects both signal lines equally. They also provide double the signal swing compared to single-ended circuits for the same supply voltage.

**Q3: How does differential signaling improve dynamic range?**
**A3:** By doubling the maximum signal swing (from $V_{DD}$ to $2V_{DD}$ differential peak-to-peak), differential signaling increases the maximum signal power by a factor of 4 (6 dB). Since the noise power typically increases by only a factor of 2 (3 dB) due to two noise sources, the overall Signal-to-Noise Ratio (SNR) and dynamic range improve by 3 dB.

## 4.2 Basic Differential Pair

**Q4: Explain the operation of a basic differential pair with a tail current source.**
**A4:** A basic differential pair consists of two matched transistors sharing a common source node biased by a constant current source ($I_{SS}$). The input differential voltage steers this fixed tail current between the two branches. If $V_{in1} > V_{in2}$, more current flows through $M_1$ and less through $M_2$, creating a differential output voltage across the load devices.

**Q5: What is the "Common-Mode Input Range" (ICMR) and what limits it?**
**A5:** The ICMR is the range of input common-mode voltages for which all transistors in the differential pair and the tail current source remain in saturation.
*   **Lower Limit:** Determined by the need to keep the tail current source in saturation ($V_{in,CM} \ge V_{GS1} + V_{DS,sat(tail)}$).
*   **Upper Limit:** Determined by the need to keep the input transistors in saturation ($V_{in,CM} \le V_{DD} - R_D I_{SS}/2 + V_{TH}$).

**Q6: How does "Source Degeneration" affect the differential pair?**
**A6:** Adding resistors ($R_S$) in series with the sources of the input transistors linearizes the amplifier. It extends the linear input range by approximately $I_{SS}R_S$ but reduces the effective transconductance to $G_m \approx 1/(R_S + 1/g_m)$. This trades gain for linearity.

**Q7: What is the small-signal differential gain of a basic differential pair with resistive loads?**
**A7:** The differential voltage gain is $|A_{DM}| = g_m R_D$, where $g_m$ is the transconductance of the input transistor and $R_D$ is the load resistance.

## 4.3 Common-Mode Response

**Q8: What is Common-Mode Rejection Ratio (CMRR)?**
**A8:** CMRR is the ratio of the differential gain to the common-mode gain ($CMRR = |A_{DM} / A_{CM}|$). It quantifies the amplifier's ability to reject common-mode signals (noise) and amplify only the differential signal.

**Q9: What is the primary cause of finite CMRR in a differential pair?**
**A9:** Finite CMRR is primarily caused by the finite output impedance of the tail current source ($R_{SS}$) combined with mismatches in the input transistors or load resistors. If $R_{SS}$ is not infinite, a common-mode input change causes a change in the tail current, which (due to asymmetry) results in a differential output error.

**Q10: How can CMRR be improved?**
**A10:** The most effective way to improve CMRR is to increase the output impedance of the tail current source ($R_{SS}$), typically by using a cascode current source. Improving the matching of the input pair and loads through careful layout (e.g., common centroid) also helps.

## 4.4 Differential Pair with MOS Loads

**Q11: Why are MOS loads used instead of resistive loads in integrated circuits?**
**A11:** High-value resistors occupy a very large area in CMOS technology and have poor tolerance. MOS loads (either diode-connected or current sources) are much more compact and can provide higher incremental resistance (active loads), leading to higher gain.

**Q12: Compare Diode-Connected Loads vs. Current-Source Loads.**
**A12:**
*   **Diode-Connected Loads:** Provide low gain ($A_v \approx \sqrt{(W/L)_{in}/(W/L)_{load}}$) but are relatively linear and self-biasing (no CMFB needed). They consume voltage headroom.
*   **Current-Source Loads:** Provide very high gain ($A_v \approx g_m (r_{oN} || r_{oP})$) but require a Common-Mode Feedback (CMFB) circuit to define the output DC level.

**Q13: What is the main limitation of using a simple current-source load?**
**A13:** The output node is high-impedance and "floating." Without a CMFB circuit, small mismatches between the PMOS and NMOS currents will drive the output voltage to either $V_{DD}$ or Ground, saturating the amplifier.

## 4.5 Gilbert Cell

**Q14: What is a Gilbert Cell and what is its primary application?**
**A14:** A Gilbert Cell is a circuit composed of two cross-coupled differential pairs driven by a third differential pair (or current source). It acts as an analog multiplier, producing an output proportional to the product of two input signals. It is widely used as a mixer in RF circuits and as a Variable Gain Amplifier (VGA).

**Q15: How does the Gilbert Cell perform multiplication?**
**A15:** The lower differential pair converts one input voltage into a differential current. This current acts as the "tail current" for the upper cross-coupled pairs. Since the gain of a differential pair is proportional to its tail current ($g_m \propto \sqrt{I_{SS}}$ or $I_{SS}$), the upper stage's gain is modulated by the lower stage's signal, effectively multiplying the two inputs.

**Q16: What is the major design challenge for Gilbert Cells in modern processes?**
**A16:** Voltage headroom. The Gilbert Cell stacks three levels of transistors (current source, lower pair, upper pair) plus the load. In low-voltage processes (e.g., 1V), keeping all these devices in saturation is extremely difficult.

## 4.6 Common-Mode Feedback (CMFB)

**Q17: Why is Common-Mode Feedback (CMFB) necessary in fully differential amplifiers?**
**A17:** In high-gain fully differential amplifiers, the output common-mode level is not well-defined by the input. Small current mismatches can cause the output CM level to drift to the supply rails. CMFB senses the output CM level and adjusts the bias current to force it to a specific reference voltage ($V_{REF}$).

**Q18: Describe a simple method for sensing the output common-mode voltage.**
**A18:** A resistive divider connected between the two differential outputs can sense the average voltage ($V_{out,CM} = (V_{out+} + V_{out-})/2$). However, this loads the amplifier and reduces gain. Switched-capacitor sensing is often preferred in discrete-time circuits as it does not load the DC gain.
