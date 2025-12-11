# Chapter 3: Single-Stage Amplifiers

## 3.1 Applications
**What the concept is**
Single-stage amplifiers are the atomic units of analog design. They are the simplest circuits capable of providing voltage gain, current buffering, or impedance transformation. They consist of a single transistor (or a composite pair like a cascode) and a load.

**Why it is used**
They form the internal nodes of complex systems. For example, an OpAmp is typically a differential pair (single stage) followed by a Common Source stage (single stage). They are used to amplify weak sensor signals, drive capacitive loads, or isolate sensitive nodes.

**Challenges and Limitations**
- **Trade-offs:** You rarely get high gain, high speed, and large swing in one simple stage.
- **Loading:** A single stage is often sensitive to what is connected to its output (loading effect).
- **PVT Sensitivity:** Simple stages without feedback have gains that vary wildly with Process, Voltage, and Temperature.

**Practical techniques to mitigate**
- **Cascading:** Chain multiple stages. Use a high-gain stage (CS) followed by a buffer (Source Follower).
- **Feedback:** Never rely on the open-loop gain of a single stage for precise amplification. Use them inside a feedback loop.

## 3.2 General Considerations
**What the concept is**
The "Analog Octagon" of trade-offs: Gain, Bandwidth, Swing, Power, Noise, Linearity, Supply Voltage, and Area.

**Why it is used**
To set realistic expectations. You cannot optimize all parameters simultaneously.

**Challenges and Limitations**
- **The "Squeeze":** Improving one metric usually degrades another. E.g., increasing Gain ($g_m R_{out}$) by increasing $L$ lowers Speed (higher capacitance). Increasing $g_m$ by increasing current burns Power.

**Practical techniques to mitigate**
- **Figure of Merit (FOM):** Define a metric like $(Gain \times BW) / Power$ to compare topologies objectively.
- **Prioritization:** Identify the 2-3 "do or die" specs for your block and sacrifice the others.

## 3.3 Common-Source (CS) Stage
**What the concept is**
The input signal is applied to the Gate, the output is taken from the Drain, and the Source is grounded (AC ground). It is a transconductor ($V \to I$) driving a load impedance.

**Why it is used**
It is the primary gain stage in CMOS. It provides high input impedance (Gate) and significant voltage gain ($A_v = -g_m R_{out}$).

**Challenges and Limitations**
- **Miller Effect:** The Gate-Drain capacitance ($C_{gd}$) is amplified by the gain, appearing as a massive capacitor at the input ($C_{in} \approx A_v C_{gd}$). This kills input bandwidth.
- **High Output Impedance:** It is a poor voltage driver. Driving a low-resistance load kills the gain.
- **Right Half Plane (RHP) Zero:** In high-frequency models, $C_{gd}$ creates a feedforward path that adds a zero in the RHP, complicating stability in feedback loops.

**Practical techniques to mitigate**
- **Cascode it:** Add a Common-Gate device on top to shield the input from the output, eliminating the Miller effect.
- **Buffer it:** Follow the CS stage with a Source Follower if you need to drive a resistive load.
- **Sizing:** For high speed, use minimum channel length ($L$) to maximize $f_T$. For high gain, use non-minimum $L$ (2x-3x min) to increase $r_o$.

### 3.3.1 CS with Resistive Load
**What the concept is**
A CS stage where the load is a simple passive resistor $R_D$.

**Why it is used**
It is linear and simple. Resistors don't add flicker noise (unlike active loads). Used in high-speed RF amplifiers where inductive peaking or low-Q loads are common.

**Challenges and Limitations**
- **Headroom vs. Gain:** To get high gain ($g_m R_D$), you need a large $R_D$. But a large $R_D$ drops a lot of DC voltage ($I_D R_D$), crushing the output swing range.
- **Area:** Large resistors take up huge silicon area in CMOS.

**Practical techniques to mitigate**
- **Use for Low Gain:** Only use this topology when you need low gain (e.g., 2-5x) or very high speed.
- **Poly Resistors:** Use unsalicided poly resistors for better linearity and lower parasitic capacitance compared to diffusion resistors.

### 3.3.2 CS with Diode-Connected Load
**What the concept is**
The load is a MOSFET with its Gate tied to its Drain. It acts like a resistor of value $1/g_{m,load}$.

**Why it is used**
The gain becomes a ratio of dimensions: $A_v \approx -\sqrt{(W/L)_{in} / (W/L)_{load}}$. This is relatively independent of PVT variations. It is self-biased.

**Challenges and Limitations**
- **Low Gain:** Since $g_m$ is roughly proportional to $\sqrt{W/L}$, achieving a gain of 10 requires a size ratio of 100:1, which is impractical.
- **Headroom:** The diode load consumes at least $V_{TH} + V_{OV}$ of headroom.

**Practical techniques to mitigate**
- **Use in Differential Pairs:** This is the standard load for the first stage of a comparator or a low-gain preamp where linearity and speed matter more than raw gain.

### 3.3.3 CS with Current-Source Load
**What the concept is**
The load is a PMOS transistor biased in saturation (constant current source). The load impedance is $r_{o,p}$.

**Why it is used**
To achieve high gain. The incremental resistance $r_o$ is much larger than $1/g_m$, allowing gains of 20-100 in a single stage without a massive DC voltage drop.

**Challenges and Limitations**
- **Floating Output:** The output DC node is high impedance and ill-defined. A tiny mismatch in currents between the NMOS and PMOS sends the output railing to VDD or GND.
- **Process Sensitivity:** The gain $-g_m (r_{o,n} || r_{o,p})$ varies significantly with process corners.

**Practical techniques to mitigate**
- **Feedback Biasing:** You *must* use a global feedback loop (like in an OpAmp configuration) or Common-Mode Feedback (CMFB) to stabilize the output DC operating point.
- **Channel Length:** Use $L > L_{min}$ (e.g., 0.5um in a 0.18um process) for both devices to boost $r_o$ and gain.

### 3.3.4 CS Stage with Active Load
**What the concept is**
Often refers to the CMOS inverter amplifier (Push-Pull) where both NMOS and PMOS are driven by the input.

**Why it is used**
"Current Reuse." Both devices contribute to $g_m$, doubling the efficiency for the same current. $A_v = -(g_{m,n} + g_{m,p})(r_{o,n} || r_{o,p})$.

### 3.3.5 CS Stage with Triode Load
**What the concept is**
The load is a MOS device operating in the deep triode region, behaving like a voltage-controlled resistor.

**Why it is used**
**Tunability.** The resistance $R_{on} \approx \frac{1}{\mu C_{ox} (W/L) (V_{GS} - V_{TH})}$ can be adjusted by changing the gate voltage of the load. This allows for variable gain amplifiers or tuning of filter time constants.

**Challenges and Limitations**
- **Linearity:** The resistance is non-linear with respect to $V_{DS}$ (the output signal). This causes distortion for large swings.
- **Sensitivity:** The resistance is very sensitive to $V_{GS}$ and $V_{TH}$ variations.

**Practical techniques to mitigate**
- **Small Swings:** Use only for small signal swings where the non-linearity is negligible.

### 3.3.6 CS Stage with Source Degeneration
**What the concept is**
Adding a resistor $R_S$ in series with the source of the input transistor.

**Why it is used**
**Linearization.** It makes the effective transconductance $G_m \approx \frac{g_m}{1 + g_m R_S} \approx \frac{1}{R_S}$ (for large $g_m R_S$). The gain becomes $-R_D / R_S$, which is determined by a ratio of passive components, making it highly linear and independent of transistor parameters.

**Challenges and Limitations**
- **Gain Reduction:** You sacrifice gain for linearity.
- **Noise:** The resistor adds thermal noise.

**Practical techniques to mitigate**
- **Variable Gain:** By varying $R_S$ (e.g., using a triode MOS), you can build a Variable Gain Amplifier (VGA).

## 3.4 Source Follower (Common Drain)
**What the concept is**
Input at Gate, Output at Source. The Drain is at AC ground.

**Why it is used**
**Voltage Buffer.** It has high input impedance and low output impedance ($1/g_m$). It is used to drive low-impedance loads (like resistors or large capacitors) without loading the previous stage.
**Level Shifter:** It shifts the DC level down by $V_{GS}$.

**Challenges and Limitations**
- **Gain < 1:** The gain is $\frac{g_m R_L}{1 + g_m R_L} < 1$.
- **Body Effect:** Since the source is not at ground, the body effect ($g_{mb}$) reduces the gain further to $\approx \frac{g_m}{g_m + g_{mb}} \approx 0.8$.
- **Non-Linearity:** $V_{GS}$ depends on $V_{out}$ (via $I_D$ variations if resistive load) and $V_{TH}$ varies with $V_{out}$ (body effect), causing distortion.

**Practical techniques to mitigate**
- **PMOS Follower:** Use a PMOS follower in an N-well process where you can tie the Bulk to the Source to eliminate the body effect.

## 3.5 Common-Gate Stage
**What the concept is**
Input at Source, Output at Drain. Gate is at AC ground.

**Why it is used**
**Current Buffer.** It has low input impedance ($1/g_m$) and high output impedance. Ideally suited for amplifying current signals (e.g., from a photodiode) or as an impedance matching stage in RF (matching $1/g_m$ to $50\Omega$).

**Challenges and Limitations**
- **Low Input Impedance:** It loads the source heavily.
- **No Current Gain:** $A_I = 1$.

**Practical techniques to mitigate**
- **Cascode:** The CG stage is the upper half of a Cascode amplifier.

## 3.6 Cascode Stage
**What the concept is**
A Common-Source stage followed by a Common-Gate stage.

**Why it is used**
**Output Impedance Boost.** The CG stage shields the CS stage, boosting the output resistance to $g_m r_o^2$. This enables very high gain.
**Bandwidth Extension:** It eliminates the Miller effect on the input CS stage, improving high-frequency response.

**Challenges and Limitations**
- **Headroom:** Requires stacking two transistors, consuming $V_{TH} + 2V_{OV}$ of headroom.

### 3.6.1 Folded Cascode
**What the concept is**
The CS input device (e.g., NMOS) and the CG cascode device (e.g., PMOS) are of opposite types. The current "folds" from the CS stage up to the CG stage.

**Why it is used**
**Input Range.** It allows the input and output common-mode levels to be compatible (e.g., both can be near mid-rail).
**Headroom:** Slightly better headroom than telescopic cascode for the output swing.

**Challenges and Limitations**
- **Power:** Requires two bias current legs (one for CS, one for CG), consuming more power than telescopic.
- **Noise:** The current sources in the folding leg add noise.

## 3.7 Choice of Device Models
**What the concept is**
Choosing the right level of abstraction (Level 1 Square Law vs. BSIM) for hand analysis vs. simulation.

**Why it is used**
To balance accuracy and insight.

**Challenges and Limitations**
- **Hand Calc Errors:** Short-channel effects (velocity saturation, DIBL) make simple square-law hand calculations inaccurate (often 50% off).

**Practical techniques to mitigate**
- **$g_m/I_D$ Methodology:** Use lookup tables generated from SPICE for design, rather than analytic equations. Design based on efficiency ($g_m/I_D$) and transit frequency ($f_T$).

**Challenges and Limitations**
- **Bias Sensitivity:** Extremely sensitive to PVT. The region where both devices are in saturation is very narrow.
- **Supply Noise:** Poor PSRR because the supply is directly connected to the source of the PMOS input device.

**Practical techniques to mitigate**
- **Auto-zeroing:** Use in switched-capacitor circuits where the bias point is periodically reset by shorting input to output.
- **Replica Biasing:** Use a replica inverter to generate the bias voltage that sets the current.

### 3.3.5 CS with Triode Load
**What the concept is**
The load is a MOS device biased in the deep triode region ($V_{DS} \ll V_{OV}$), acting as a voltage-controlled resistor.

**Why it is used**
Tunability. You can adjust the gain by changing the gate voltage of the load device.

**Challenges and Limitations**
- **Non-linearity:** The resistance of a triode device is non-linear with signal swing ($R_{on}$ depends on $V_{DS}$).
- **Poor Accuracy:** $R_{on}$ is very sensitive to $V_{GS}$ and $V_{TH}$ variations.

**Practical techniques to mitigate**
- **Differential Cancellation:** Use a fully differential structure. The odd-order non-linearities cancel out, improving linearity.
- **Deep Triode:** Keep the voltage drop across the load small (< 100mV) to maintain linearity.

### 3.3.6 CS with Source Degeneration
**What the concept is**
Placing a resistor $R_S$ in series with the Source.

**Why it is used**
**Linearization.** It applies local negative feedback. The effective transconductance becomes $G_m \approx 1/R_S$ (for large $g_m R_S$). The gain is determined by the ratio $R_D/R_S$, which is very linear.

**Challenges and Limitations**
- **Gain Penalty:** You sacrifice gain for linearity.
- **Noise:** The resistor $R_S$ adds thermal noise.
- **Headroom:** $I_D R_S$ consumes voltage headroom.

**Practical techniques to mitigate**
- **Inductive Degeneration:** In LNA design, use an inductor $L_S$ instead of a resistor. It provides degeneration at high frequencies (improving linearity and input matching) without adding thermal noise or DC voltage drop.

## 3.4 Source Follower (Common Drain)
**What the concept is**
Input at Gate, Output at Source. The Source "follows" the Gate voltage.

**Why it is used**
**Voltage Buffer.** It has high input impedance and low output impedance ($1/g_m$). It is used to drive low-impedance loads or to shift DC levels (level shifter).

**Challenges and Limitations**
- **Gain < 1:** Typically 0.8 - 0.9 due to the body effect ($g_{mb}$) and finite load resistance.
- **Non-linearity:** The body effect makes $V_{TH}$ vary with the output voltage, causing distortion.
- **"Ringy":** The output impedance can look inductive at high frequencies. If driving a capacitive load, it can form an LC tank and ring or oscillate.

**Practical techniques to mitigate**
- **Deep N-Well:** If available, put the NMOS in a deep N-well and tie the Bulk to the Source. This eliminates the body effect and improves gain and linearity.
- **Compensation:** Add a small series resistor at the Gate to dampen the high-frequency ringing.

## 3.5 Common-Gate (CG) Stage
**What the concept is**
Input at Source, Output at Drain, Gate is AC grounded.

**Why it is used**
**Impedance Matching.** The input impedance is $\approx 1/g_m$. By biasing correctly, you can match it to $50\Omega$ for RF inputs.
**Speed.** It has no Miller effect because the Gate is grounded.

**Challenges and Limitations**
- **Low Input Impedance:** It draws current from the signal source. It is not a good voltage sensor.
- **Noise:** The noise of the CG device is directly referred to the input.

**Practical techniques to mitigate**
- **Cascode:** The most common use of CG is as the "top" device in a cascode pair, shielding the "bottom" CS device.
- **$g_m$-boosting:** Use an auxiliary amplifier to boost the effective $g_m$, lowering the input impedance further if needed.

## 3.6 Cascode Stage
**What the concept is**
Stacking a CG transistor on top of a CS transistor.

**Why it is used**
**Output Impedance Boost.** It multiplies the output resistance by the intrinsic gain of the cascode device ($R_{out} \approx g_m r_o^2$). This enables very high gain in a single stage.
**Bandwidth Extension.** It eliminates the Miller effect on the input device.

**Challenges and Limitations**
- **Headroom Starvation:** It requires enough voltage for two transistors to stay in saturation ($V_{out,min} = V_{OV1} + V_{OV2}$).
- **Noise:** The cascode device adds some noise at high frequencies (though negligible at low frequencies).

**Practical techniques to mitigate**
- **Low-Voltage Biasing:** Do not bias the cascode gate at $V_{DD}$. Bias it such that the bottom transistor is *just* at the edge of saturation ($V_{DS} \approx V_{OV}$). This maximizes swing.
- **Wide-Swing Cascode Current Mirrors:** Use specialized biasing circuits to ensure the cascode stack works with minimum headroom.

### 3.6.1 Folded Cascode
**What the concept is**
Instead of stacking a same-type transistor on top, use an opposite-type transistor (e.g., PMOS input, NMOS cascode) and "fold" the current path.

**Why it is used**
**Input Common Mode Range (ICMR).** It allows the input and output voltage ranges to overlap. The input can go close to the negative supply (for PMOS input).
**Low Voltage:** It requires fewer stacked transistors in the output leg compared to a telescopic cascode.

**Challenges and Limitations**
- **Power:** It requires two bias currents (input branch and folded branch), consuming more power.
- **Noise:** The current source devices in the folded branch add significant noise.
- **Speed:** The "folding" node has parasitic capacitance that creates a non-dominant pole, slightly limiting bandwidth compared to telescopic.

**Practical techniques to mitigate**
- **Sizing:** Make the current in the output branch slightly higher than the input branch to ensure the cascode device never turns off during slewing.
- **Choice:** Use Folded Cascode for general-purpose OpAmps where wide input swing is needed. Use Telescopic for low-power, high-gain applications where swing is less critical.

## 3.7 Choice of Device Models
**What the concept is**
Selecting the appropriate level of complexity for analysis: Level 1 (Square Law) for hand calcs vs. BSIM/EKV for simulation.

**Why it is used**
To balance insight with accuracy.

**Challenges and Limitations**
- **The "Square Law" Lie:** Modern short-channel devices do not follow $I_D = \frac{1}{2} \mu C_{ox} \frac{W}{L} (V_{GS}-V_{TH})^2$. Velocity saturation makes it linear ($I_D \propto V_{GS}$). Output impedance is dominated by DIBL and SCBE, not just $\lambda$.

**Practical techniques to mitigate**
- **$g_m/I_D$ Methodology:** Stop designing with voltage equations. Design with efficiency ($g_m/I_D$) and transit frequency ($f_T$).
- **Look-up Tables:** Generate plots of $g_m/I_D$ vs. $I_D/W$ from SPICE and use these for hand sizing.
- **Simulation:** Use hand analysis only for topology selection and rough sizing. Rely on the simulator for the final 20% of accuracy.
