# Chapter 4: Differential Amplifiers

## Table of Contents
- [4.1 Single-Ended vs Differential](#41-single-ended-vs-differential)
- [4.2 Basic Differential Pair](#42-basic-differential-pair)
  - [4.2.1 Qualitative Analysis](#421-qualitative-analysis)
  - [4.2.2 Quantitative Analysis](#422-quantitative-analysis)
  - [4.2.3 Degenerated Differential Pair](#423-degenerated-differential-pair)
- [4.3 Common-Mode Response](#43-common-mode-response)
- [4.4 Differential Pair with MOS Loads](#44-differential-pair-with-mos-loads)
- [4.5 Gilbert Cell](#45-gilbert-cell)
- [4.6 Common-Mode Feedback (CMFB)](#46-common-mode-feedback-cmfb)


## 4.1 Single-Ended vs Differential
**What the concept is**
Single-ended signals are referenced to a fixed potential (usually ground). Differential signals are carried on two wires with opposite phases ($V^+ = V_{CM} + \Delta V/2$, $V^- = V_{CM} - \Delta V/2$).

**Why it is used**
**Noise Immunity.** The "magic" of differential circuits is that they reject "Common Mode" noise. If a noise spike (from supply or substrate) hits both wires equally, the *difference* remains unchanged.
**Swing:** It doubles the available signal swing ($V_{DD} \to 2V_{DD}$).

**Challenges and Limitations**
- **Area & Power:** Requires 2x the components and 2x the power (roughly) compared to a single-ended equivalent.
- **Complexity:** Requires a tail current source and Common-Mode Feedback (CMFB) if the output is fully differential.

**Practical techniques to mitigate**
- **Always Differential:** In modern mixed-signal SoCs, the digital noise is so high that single-ended analog is almost extinct. The area penalty is the price of admission.
- **Layout:** Route differential traces parallel and close together so they pick up noise identically (which is then rejected).

## 4.2 Basic Differential Pair
**What the concept is**
Two source-coupled transistors biased by a constant tail current source ($I_{SS}$). The input is the voltage difference between the gates.

**Why it is used**
It is the fundamental input stage for almost all OpAmps and comparators. It steers current between the two branches based on the input difference.

**Challenges and Limitations**
- **Limited Linear Range:** The circuit is only linear for a small input range ($\Delta V_{in} \approx \sqrt{2} V_{OV}$). Beyond that, it saturates (one side takes all the current).
- **Offset:** Mismatch between the two transistors (threshold voltage $V_{TH}$ or size $W/L$) creates an input-referred offset voltage.

**Practical techniques to mitigate**
- **Overdrive Voltage:** To increase the linear input range, increase the overdrive voltage ($V_{OV} = V_{GS} - V_{TH}$) of the input pair (at the cost of transconductance).
- **Layout:** Use "Common Centroid" layout for the input pair to cancel process gradients and reduce offset.

### 4.2.1 Qualitative Analysis
**What the concept is**
Understanding the circuit by inspection. If $V_{in1}$ rises, $M_1$ turns on harder. Since total current $I_{SS}$ is fixed, $M_2$ must turn off. The common source node follows the higher input.

**Why it is used**
To quickly verify polarity and feedback loop directions without doing math.

**Challenges and Limitations**
- **Intuition Traps:** It's easy to forget about the tail current source's compliance voltage. If the inputs go too low, the tail source enters triode, and the differential action dies.

**Practical techniques to mitigate**
- **Check the Tail:** Always verify that the tail current source has enough $V_{DS}$ to stay in saturation across the entire Input Common Mode Range (ICMR).

### 4.2.2 Quantitative Analysis
**What the concept is**
Large signal behavior: $I_{D1}-I_{D2} = \frac{1}{2} \mu C_{ox} \frac{W}{L} \Delta V_{in} \sqrt{\frac{4I_{SS}}{\mu C_{ox} W/L} - \Delta V_{in}^2}$.
Small signal transconductance: $G_m = g_m$ (at equilibrium).

**Why it is used**
To size the transistors for a specific gain and swing.

**Challenges and Limitations**
- **Square Law Limitations:** The equation assumes long-channel behavior. In short-channel devices, the velocity saturation makes the $I-V$ curve more linear but the transconductance lower.

**Practical techniques to mitigate**
- **$g_m$ Efficiency:** Design for a specific $g_m$ to meet bandwidth requirements ($BW = g_m / C_L$).
- **Tail Current:** Set $I_{SS}$ based on the required Slew Rate ($SR = I_{SS} / C_L$).

### 4.2.3 Degenerated Differential Pair
**What the concept is**
Adding resistors ($R_S$) in series with the sources of the differential pair.

**Why it is used**
**Linearization.** It extends the linear input range by dropping the signal across the linear resistors instead of the non-linear gate-source capacitance. $G_m \approx 1/R_S$.

**Challenges and Limitations**
- **Noise:** The resistors add thermal noise directly at the input.
- **Gain Reduction:** Transconductance drops significantly.
- **Headroom:** The voltage drop $I_{SS} R_S/2$ eats into the headroom.

**Practical techniques to mitigate**
- **Use Case:** Essential for input buffers or highly linear variable gain amplifiers (VGAs). Avoid in the first stage of a low-noise amplifier.

## 4.3 Common-Mode Response
**What the concept is**
How the circuit behaves when both inputs change by the same amount. Ideally, the output differential voltage should not change.

**Why it is used**
To quantify **CMRR (Common Mode Rejection Ratio)**. High CMRR is crucial to reject supply noise and ground bounce.

**Challenges and Limitations**
- **Finite Tail Impedance:** If the tail current source is not perfect (finite $R_{SS}$), common-mode variations change the bias current, which (due to mismatch) creates a differential error.
- **CM-to-DM Conversion:** Asymmetries in the load or input pair convert CM noise into differential signal.

**Practical techniques to mitigate**
- **Cascode the Tail:** Use a cascode current source for the tail to maximize its output impedance ($R_{SS}$). This is the single most effective way to boost CMRR.
- **Symmetry:** Rigorous symmetric layout is non-negotiable.

## 4.4 Differential Pair with MOS Loads
**What the concept is**
Replacing passive resistor loads with MOS devices (Diode-connected or Current Source).

**Why it is used**
**Integration:** Resistors are large and inaccurate in CMOS. MOS loads are compact.
**High Gain:** Current source loads (Active Loads) provide high output impedance and thus high gain.

**Challenges and Limitations**
- **Diode Load:** Limits output swing (output cannot go higher than $V_{DD} - V_{TH}$). Low gain.
- **Current Source Load:** Requires a CMFB circuit to define the output common-mode voltage.

**Practical techniques to mitigate**
- **Active Load (Current Mirror Load):** Converts the differential current to a single-ended output voltage, saving the need for a second stage to do the conversion (e.g., in a 5-transistor OTA).

## 4.5 Gilbert Cell
**What the concept is**
A "stack" of three differential pairs. Two top pairs are cross-coupled and driven by one input, while their tail currents are steered by a bottom differential pair driven by a second input.

**Why it is used**
**Analog Multiplier.** It performs the operation $V_{out} = K \times V_{in1} \times V_{in2}$.
**Mixer:** Used in RF to mix a signal with a Local Oscillator (LO) for frequency translation.
**VGA:** Used as a Variable Gain Amplifier by controlling one input as a DC gain control knob.

**Challenges and Limitations**
- **Headroom:** Stacking 3 transistors + load is very difficult in low-voltage processes (e.g., 1.2V or 1.0V).
- **Noise:** The noise of the switching quad (top pairs) contributes significantly to the output.

**Practical techniques to mitigate**
- **Current Commutation:** In mixers, drive the top pair with a large square wave (LO) to switch them abruptly. This reduces the time they spend in the "noisy" active region.
- **Folded Topology:** If headroom is tight, use a folded structure to separate the RF and LO stages.

## 4.6 Common-Mode Feedback (CMFB)
**What the concept is**
A negative feedback loop that senses the Common-Mode level of the differential output ($V_{out,CM} = (V_{out+} + V_{out-})/2$) and adjusts the bias current to force it to a desired reference voltage ($V_{REF}$).

**Why it is used**
**Defining the Operating Point.** In fully differential amplifiers with high-impedance current source loads (like active mirrors or folded cascodes), the output node is "floating". Small mismatches between the PMOS source current and NMOS sink current will drive the output to the supply rails. CMFB is mandatory to hold the output at mid-rail.

**Challenges and Limitations**
- **Stability:** The CMFB loop is a feedback loop itself and can oscillate. It needs its own compensation.
- **Loading:** The circuit used to sense the output CM (e.g., resistors) can load the differential signal, reducing gain.

**Practical techniques to mitigate**
- **Switched-Capacitor CMFB:** Use capacitors and switches to sense the CM level. Capacitors don't load the DC gain of the amplifier.
- **Resistive Averaging:** Use large resistors in parallel with small capacitors to sense the average voltage without killing the AC gain too much.
