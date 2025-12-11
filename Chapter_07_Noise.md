# Chapter 7: Noise

## 7.1 Statistical Characteristics of Noise
**What the concept is**
Noise is a random process. We cannot predict the voltage at a specific time, but we can predict the *power* (variance) and the *frequency distribution* (spectrum).

**Why it is used**
**Sensitivity Limit.** Noise determines the smallest signal a circuit can process. In an ADC, it determines the maximum resolution (ENOB). In an RF receiver, it determines the sensitivity.

**Challenges and Limitations**
- **Physics:** You cannot eliminate noise; you can only trade it for power.
- **Math:** It requires statistical thinking (RMS, PSD) rather than deterministic algebra.

**Practical techniques to mitigate**
- **Averaging:** Noise has zero mean. Averaging (filtering) reduces the noise variance.
- **Power Burn:** To reduce thermal noise voltage by 2x, you must increase capacitor size by 4x and power by 4x.

### 7.1.1 Noise Spectrum
**What the concept is**
Power Spectral Density (PSD), $S_v(f)$, measured in $V^2/Hz$. It shows how noise power is distributed across frequencies.
- **White Noise:** $S_v(f)$ is constant (e.g., Thermal Noise).
- **Colored Noise:** $S_v(f)$ varies with frequency (e.g., Flicker Noise $\propto 1/f$).

**Why it is used**
To identify which noise sources dominate in the signal bandwidth.

**Challenges and Limitations**
- **Two-Sided vs. One-Sided:** Mathematicians use $-\infty$ to $+\infty$ (Two-Sided). Engineers use $0$ to $+\infty$ (One-Sided). Be careful with factors of 2.

**Practical techniques to mitigate**
- **Integration:** To get the total RMS noise voltage, you must integrate the PSD over your system's bandwidth: $V_{rms} = \sqrt{\int_{0}^{\infty} S_v(f) |H(f)|^2 df}$.

### 7.1.2 Amplitude Distribution
**What the concept is**
The Probability Density Function (PDF). For most electronic noise (thermal, shot), it is Gaussian (Normal Distribution).

**Why it is used**
To predict the probability of rare events (e.g., a large noise spike causing a bit error).

**Challenges and Limitations**
- **Crest Factor:** Noise peaks can be much larger than the RMS value. A $3\sigma$ peak happens 0.3% of the time.

**Practical techniques to mitigate**
- **Design Margin:** Design for $6\sigma$ (peak-to-peak noise $\approx 6 \times V_{rms}$) to ensure errors are negligible (BER $< 10^{-9}$).

### 7.1.3 Correlated and Uncorrelated Sources
**What the concept is**
- **Uncorrelated:** Sources arise from different physical devices (e.g., two different resistors). They add in **Power** (Mean Square): $\overline{V_{tot}^2} = \overline{V_1^2} + \overline{V_2^2}$.
- **Correlated:** Sources arise from the same physical mechanism (e.g., gate and drain noise of the same transistor). They add in **Voltage**: $\overline{V_{tot}^2} = \overline{(V_1 + V_2)^2}$.

**Why it is used**
To simplify calculation. We usually assume sources are uncorrelated unless stated otherwise.

**Challenges and Limitations**
- **Differential Circuits:** Common-mode noise is highly correlated. If you treat it as uncorrelated, you will underestimate the error.

**Practical techniques to mitigate**
- **Superposition:** Turn on one noise source at a time, calculate output, square it. Then sum all squares.

### 7.1.4 Signal-to-Noise Ratio (SNR)
**What the concept is**
The ratio of Signal Power to Noise Power.
$SNR = 10 \log_{10} \left( \frac{P_{signal}}{P_{noise}} \right) = 20 \log_{10} \left( \frac{V_{signal,rms}}{V_{noise,rms}} \right)$.

**Why it is used**
The ultimate figure of merit for analog signal quality.

**Challenges and Limitations**
- **The Ceiling:** Once noise hits the thermal floor ($kT/C$), the only way to improve SNR is to increase signal swing.

**Practical techniques to mitigate**
- **Maximize Swing:** It is "cheaper" to double the signal amplitude (6dB SNR gain) than to quarter the noise power (4x area/power penalty).

### 7.1.5 Noise Analysis Procedure
**What the concept is**
A systematic method to calculate the total input-referred noise:
1.  **Identify Sources:** Add a noise voltage/current source for every resistor and transistor.
2.  **Output Noise:** Calculate the contribution of each source to the output ($\overline{V_{out,i}^2}$).
3.  **Summation:** Sum all output noise powers: $\overline{V_{out,tot}^2} = \sum \overline{V_{out,i}^2}$.
4.  **Input Referral:** Divide the total output noise by the squared gain of the circuit ($A_v^2$) to find the Input Referred Noise: $\overline{V_{in,tot}^2} = \overline{V_{out,tot}^2} / A_v^2$.

**Why it is used**
**Input Referred Noise** allows fair comparison between different topologies regardless of their gain.

**Challenges and Limitations**
- **Tedious:** Hand analysis is prone to algebra errors.

**Practical techniques to mitigate**
- **Simulation:** Use `.NOISE` analysis in SPICE. It automatically lists the top contributors. Use hand analysis only to understand *how* to fix the top contributors.

## 7.2 Types of Noise

### 7.2.1 Thermal Noise
**What the concept is**
Caused by random thermal motion of electrons in conductors.
- **Resistor:** $\overline{V_n^2} = 4kTR \Delta f$.
- **MOSFET:** $\overline{I_n^2} = 4kT \gamma g_m \Delta f$ (Channel noise), where $\gamma \approx 2/3$ for long channels.

**Why it is used**
It is the fundamental noise floor at room temperature.

**Challenges and Limitations**
- **Temperature:** It gets worse as the chip gets hot.
- **Trade-off:** Lowering resistance ($R$) lowers voltage noise but burns more power.

**Practical techniques to mitigate**
- **$g_m$ Maximization:** In amplifiers, input referred noise is $\propto 1/g_m$. Burn current to increase $g_m$.
- **Capacitor Sizing:** In sampling circuits, noise is $kT/C$. Use large capacitors.

### 7.2.2 Flicker Noise (1/f)
**What the concept is**
Caused by "traps" at the silicon-oxide interface capturing and releasing carriers. The PSD increases at low frequencies.

**Why it is used**
It dominates in audio, biological signals, and sensor interfaces.

**Challenges and Limitations**
- **CMOS is Dirty:** MOS devices have much worse 1/f noise than BJTs because the current flows along the surface interface.
- **NMOS vs PMOS:** PMOS devices often have lower 1/f noise (buried channel effect in some processes).

**Practical techniques to mitigate**
- **Area:** Flicker noise is inversely proportional to gate area ($W \times L$). Use large devices for the input pair.
- **Chopping:** Modulate the signal to a high frequency (where 1/f is low), amplify it, and demodulate. This removes 1/f noise completely.

### 7.2.3 Shot Noise
**What the concept is**
Caused by the discrete nature of charge carriers crossing a potential barrier (like a pn-junction). $\overline{I_n^2} = 2qI_{DC} \Delta f$.

**Why it is used**
It is the dominant noise source in Bipolar Transistors (base and collector current) and Photodiodes. In MOSFETs, it appears as gate leakage noise.

**Challenges and Limitations**
- **White Noise:** Like thermal noise, it is spectrally flat (white).
- **Current Dependent:** It scales linearly with DC current (unlike thermal noise which scales with conductance).

**Practical techniques to mitigate**
- **Keep Currents Low:** In leakage-sensitive circuits, minimize the DC current crossing junctions.
- **MOSFETs:** In standard MOSFET operation (channel conduction), shot noise is usually negligible compared to thermal noise, unless gate leakage is high (ultra-thin oxides).

## 7.3 Representing Noise in Circuits
**What the concept is**
Modeling a noisy circuit as a noiseless circuit with a voltage source and a current source at the input.

**Why it is used**
To abstract the noise performance into two parameters ($V_n, I_n$).

**Challenges and Limitations**
- **Source Impedance:** If the source impedance is high, the current noise ($I_n R_S$) dominates. If low, voltage noise dominates.

**Practical techniques to mitigate**
- **Matching:** For LNAs, there is an optimal source impedance ($Z_{opt}$) that balances the contribution of $V_n$ and $I_n$ to achieve Minimum Noise Figure ($NF_{min}$).

## 7.4 Noise in Single-Stage Amplifiers

### 7.4.1 CS
**What the concept is**
Input referred noise is dominated by the input transistor and the load.
$\overline{V_{n,in}^2} \approx \frac{4kT\gamma}{g_m} (1 + \frac{g_{m,load}}{g_{m,in}})$.

**Why it is used**
To guide sizing.

**Challenges and Limitations**
- **Active Loads:** Active loads (current sources) add noise.

**Practical techniques to mitigate**
- **Rule of Thumb:** Make $g_{m,in}$ large (short L, high current) and $g_{m,load}$ small (long L, low Vov). This minimizes the load's contribution.

### 7.4.2 CG
**What the concept is**
Noise performance is generally worse than CS because the current gain is unity, but it has low input impedance.

**Why it is used**
Impedance matching.

**Challenges and Limitations**
- **No Gain:** It doesn't amplify the signal power relative to the noise as well as a CS stage.

**Practical techniques to mitigate**
- **Feedback:** Use feedback (shunt-shunt) LNA topologies instead of pure CG if noise is critical.

### 7.4.3 Source Followers
**What the concept is**
A noise generator with gain < 1.

**Why it is used**
Buffering.

**Challenges and Limitations**
- **Direct Addition:** The gate noise of the follower appears directly at the output. Since the signal is not amplified, the SNR degrades immediately.

**Practical techniques to mitigate**
- **Placement:** Never place a source follower at the *input* of a signal chain. Only use it after the signal has been amplified significantly.

### 7.4.4 Cascode
**What the concept is**
The noise of the cascode device (the top transistor) is negligible at low frequencies.

**Why it is used**
It allows us to ignore the cascode device in noise calculations.

**Challenges and Limitations**
- **High Frequency:** At high frequencies (near the pole at the source of the cascode), its noise contribution rises.

**Practical techniques to mitigate**
- **Ignore it:** For most baseband/audio designs, ignore cascode noise. Focus entirely on the input device and the load.

## 7.5 Noise in Current Mirrors
**What the concept is**
Current mirrors copy noise just like they copy signal.

**Why it is used**
Biasing.

**Challenges and Limitations**
- **Amplification:** If you mirror a current with a gain of $M$ (multiply), you multiply the noise power by $M$ as well.

**Practical techniques to mitigate**
- **Filtering:** Heavily filter the bias voltage (gate of the mirror) with a large capacitor to ground. This kills the noise from the master diode.

## 7.6 Noise in Differential Pairs
**What the concept is**
$\overline{V_{n,in}^2} = 2 \times \overline{V_{n,device}^2}$.

**Why it is used**
Differential operation.

**Challenges and Limitations**
- **3dB Penalty:** You have two input devices, so you have twice the noise power (3dB increase) compared to a single-ended stage.

**Practical techniques to mitigate**
- **Worth it:** The rejection of supply noise (which is usually much larger than device noise) makes the 3dB penalty acceptable.

## 7.7 Noise/Power Trade-Off
**What the concept is**
Noise $\propto 1/\sqrt{Power}$.

**Why it is used**
To budget power.

**Challenges and Limitations**
- **Diminishing Returns:** To reduce noise by 10x (20dB), you need 100x the power.

**Practical techniques to mitigate**
- **Efficient Architectures:** Use inverter-based (Class AB) amplifiers which have twice the $g_m$ for the same current, giving better noise/power efficiency.

## 7.8 Noise Bandwidth
**What the concept is**
The "Brick Wall" equivalent bandwidth. For a 1-pole system, $NBW = \frac{\pi}{2} f_{3dB}$.

**Why it is used**
To calculate total integrated noise easily: $V_{rms} = \sqrt{PSD \times NBW}$.

**Challenges and Limitations**
- **Aliasing:** In sampled systems, wideband noise aliases back into the baseband, increasing the effective noise bandwidth.

**Practical techniques to mitigate**
- **Anti-Aliasing Filter:** Always limit the bandwidth before sampling.

## 7.9 Input Noise Integration
**What the concept is**
Calculating the total area under the noise curve.

**Why it is used**
To get the final number (microvolts RMS) that goes into the datasheet.

**Challenges and Limitations**
- **1/f Divergence:** The integral of $1/f$ from 0 to $\infty$ is infinite.

**Practical techniques to mitigate**
- **Observation Time:** Set the lower limit of integration to $1/T_{obs}$ (e.g., 0.1Hz). Frequencies lower than that look like DC drift, not noise.

## 7.10 Appendix: Noise Correlation
**What the concept is**
Advanced modeling where gate induced noise is correlated with drain noise.

**Why it is used**
Critical for RF CMOS design (Noise Figure optimization).

**Challenges and Limitations**
- **Complexity:** Requires complex complex-number algebra.

**Practical techniques to mitigate**
- **Ignore for Analog:** For frequencies $< f_T/10$, correlation is negligible.
