# Chapter 13: Switched-Capacitor Circuits

## 13.1 General
**What the concept is**
A technique to process analog signals using discrete-time sampling. It replaces resistors with capacitors and switches. A capacitor $C$ switched at frequency $f_s$ behaves like a resistor $R_{eq} = 1/(f_s C)$.

**Why it is used**
**Accuracy.** In CMOS, absolute resistor values vary by $\pm 20\%$. However, the *ratio* of two capacitors ($C_1/C_2$) can be matched to within 0.1%. This allows for extremely precise filters and gain stages.

**Challenges and Limitations**
- **Aliasing:** Since it is a sampled system, signals above $f_s/2$ fold back into the baseband.
- **Clocking:** Requires complex non-overlapping clock generation.

**Practical techniques to mitigate**
- **Anti-Aliasing Filter:** Always precede an SC circuit with a continuous-time low-pass filter (RC filter).
- **Oversampling:** Run the clock much faster than the signal bandwidth to relax the anti-aliasing requirements.

## 13.2 Sampling Switches

### 13.2.1 MOSFETs as Switches
**What the concept is**
Using a MOS transistor in the deep triode region as a voltage-controlled switch.

**Why it is used**
It is a simple, zero-static-power switch.

**Challenges and Limitations**
- **Signal Dependent $R_{on}$:** The on-resistance depends on $V_{GS} - V_{TH}$. Since the source voltage is the signal ($V_{in}$), $V_{GS}$ varies with the signal. This modulates $R_{on}$, causing harmonic distortion.

**Practical techniques to mitigate**
- **Transmission Gate:** Use an NMOS and PMOS in parallel. As one resistance goes up, the other goes down, keeping the total resistance more constant.
- **Bootstrapping:** Use a circuit to boost the gate voltage such that $V_{GS}$ is constant (e.g., $V_{gate} = V_{in} + V_{DD}$) regardless of the input signal.

### 13.2.2 Speed
**What the concept is**
The RC time constant of the switch and sampling capacitor: $\tau = R_{on} C_H$.

**Why it is used**
**Settling.** The voltage on the capacitor must settle to within 1 LSB of accuracy within half a clock period ($T/2$).

**Challenges and Limitations**
- **Trade-off:** To make it faster (lower $R_{on}$), you need a wider switch. But a wider switch has more parasitic capacitance, which causes charge injection errors.

**Practical techniques to mitigate**
- **Sizing:** Size the switch for $\sim 7-10 \tau$ settling time within the clock phase. Do not make it larger than necessary.

### 13.2.3 Precision
**What the concept is**
The ability to capture the exact voltage.

**Why it is used**
ADCs require high precision.

**Challenges and Limitations**
- **Leakage:** The switch is never perfectly off. Subthreshold leakage drains the charge from the capacitor.

**Practical techniques to mitigate**
- **Speed:** Run the clock fast enough that leakage is negligible during the hold time.

### 13.2.4 Charge Injection Cancellation
**What the concept is**
When the MOS switch turns off, the charge in the channel ($Q_{ch} = W L C_{ox} V_{OV}$) must go somewhere. It dumps onto the sampling capacitor, creating an error voltage $\Delta V = Q_{ch}/C_H$.

**Why it is used**
This is the dominant error source in SC circuits.

**Challenges and Limitations**
- **Non-linear:** The charge depends on $V_{in}$ (via $V_{OV}$), so the error is signal-dependent (distortion).

**Practical techniques to mitigate**
- **Bottom-Plate Sampling:** A specific timing sequence. Turn off the switch connected to ground (bottom plate) *first*. Since the bottom plate is at a fixed potential (ground), the charge injection is constant (offset, not distortion).
- **Differential:** Use fully differential circuits. The injection appears as a common-mode error and is rejected.

## 13.3 SC Amplifiers

### 13.3.1 Unity-Gain Sampler/Buffer
**What the concept is**
A circuit that samples $V_{in}$ on phase 1 and connects the capacitor to the output on phase 2. A common topology is the **"Flip-Around"** sample-and-hold.
1.  **Sampling Phase ($\phi_1$):** The capacitor $C_H$ is connected between $V_{in}$ and Ground (or Virtual Ground). Charge $Q = C_H V_{in}$ is stored.
2.  **Hold Phase ($\phi_2$):** The bottom plate of $C_H$ is disconnected from $V_{in}$ and connected to the output, while the top plate is connected to the OpAmp input (virtual ground). The OpAmp forces the charge to stay on $C_H$, transferring the voltage to the output.

**Why it is used**
Track-and-Hold (T/H) amplifier for ADCs. The flip-around architecture has a feedback factor of $\beta \approx 1$, which is ideal for speed and power efficiency.

**Challenges and Limitations**
- **Offset:** The OpAmp input offset voltage ($V_{os}$) appears directly at the output.
- **Input Common Mode:** The OpAmp input nodes move from a defined common mode to the signal level (in some simple topologies), or the switches must handle rail-to-rail swings.

**Practical techniques to mitigate**
- **Autozeroing / Correlated Double Sampling (CDS):** Store the OpAmp offset on the capacitor during the sampling phase, then subtract it during the amplification phase.
- **Bottom-Plate Sampling:** Essential to avoid signal-dependent charge injection.

### 13.3.2 Non-inverting Amplifier
**What the concept is**
A circuit using two capacitors, $C_S$ (Sampling) and $C_F$ (Feedback).
1.  **Sampling ($\phi_1$):** $C_S$ charges to $V_{in}$. $C_F$ is reset (discharged).
2.  **Amplification ($\phi_2$):** $C_S$ is connected to the virtual ground. Its charge is forced onto $C_F$.
   - Conservation of Charge: $Q_{final} = Q_{initial} \implies C_F V_{out} = C_S V_{in}$.
   - Gain: $V_{out}/V_{in} = C_S / C_F$.

**Why it is used**
Precision amplification. The gain depends only on the ratio of capacitor areas ($C_S/C_F$), which can be matched to within 0.1% in CMOS, far better than resistor ratios.

**Challenges and Limitations**
- **Capacitive Loading:** The OpAmp must drive $C_F$ in series with $C_S$ (effective load $\approx \frac{C_S C_F}{C_S + C_F}$), plus the load capacitance $C_L$.
- **Slewing:** Large voltage steps at the output require the OpAmp to slew, limiting the maximum clock frequency.

**Practical techniques to mitigate**
- **Scaling:** Use the smallest capacitors allowed by the $kT/C$ noise requirement to maximize speed.
- **Slewing Budget:** Allocate ~30% of the half-clock period for slewing and 70% for linear settling.

### 13.3.3 Precision Multiply-by-Two Circuit
**What the concept is**
A specialized amplifier with Gain = 2. It often uses a single capacitor $C_S$ sampled during $\phi_1$, and then during $\phi_2$, the input connects to the output *plus* the capacitor, effectively doubling the voltage? No, typically it uses two equal capacitors $C_1 = C_2 = C$.
1.  **Sampling:** Both $C_1$ and $C_2$ sample $V_{in}$. Total Charge $Q = 2 C V_{in}$.
2.  **Amplification:** $C_1$ flips around to the feedback path, while $C_2$ connects to the output? Or $C_1$ connects to output?
   - Standard approach: $C_1$ wraps around the OpAmp (feedback), $C_2$ dumps its charge into $C_1$.
   - Result: $V_{out} = 2 V_{in}$.

**Why it is used**
The core building block of **Pipelined ADCs** (1.5-bit per stage architecture). It generates the "residue" for the next stage.

**Challenges and Limitations**
- **Capacitor Mismatch:** If $C_1 \neq C_2$, the gain is not exactly 2. This causes Gain Error, leading to DNL/INL in the ADC.
- **Finite Gain Error:** Finite OpAmp gain $A$ results in a gain of $\approx 2 (1 - 1/A\beta)$.

**Practical techniques to mitigate**
- **Capacitor Shuffling / Dynamic Element Matching (DEM):** Randomly swap the roles of $C_1$ and $C_2$ every clock cycle. This turns the gain error into random noise, which can be filtered out.

## 13.4 SC Integrator
**What the concept is**
A circuit that accumulates charge on a feedback capacitor $C_F$ without resetting it every cycle.
- **Equation:** $V_{out}(z) = \frac{C_S}{C_F} \frac{z^{-1}}{1 - z^{-1}} V_{in}(z)$.
- In time domain: $V_{out}[n] = V_{out}[n-1] + \frac{C_S}{C_F} V_{in}[n-1]$.

**Why it is used**
The heart of **Sigma-Delta ($\Sigma\Delta$) Modulators** and SC Filters. It performs the integration function required for noise shaping or filtering.

**Challenges and Limitations**
- **Integrator Leakage:** If the OpAmp has finite DC gain $A$, the pole is not exactly at DC ($z=1$). It moves inside the unit circle ($z = 1 - \epsilon$). The integrator "leaks" charge over time, limiting the precision of low-frequency signal processing.
- **Parasitic Capacitance:** Parasitics at the input and output nodes can cause gain errors if not handled by a "Parasitic-Insensitive" topology.

**Practical techniques to mitigate**
- **Parasitic-Insensitive Topology:** Use two switches at the input of $C_S$ and two at the virtual ground side, ensuring that parasitic capacitors are either always grounded or driven by a voltage source, never transferring charge to $C_F$.
- **High-Gain OpAmps:** Use Gain Boosting (Regulated Cascodes) to achieve DC gains > 80-100dB to minimize leakage.

## 13.5 SC Common-Mode Feedback (CMFB)
**What the concept is**
A switched-capacitor network used to define the output Common-Mode (CM) voltage of a fully differential amplifier.
It typically consists of:
1.  **Sensing Capacitors:** Connected between the differential outputs and the bias control node.
2.  **Refresh Switches:** Periodically recharge the sensing capacitors to a known reference voltage ($V_{CM,ref} - V_{bias,nom}$).

**Why it is used**
**Linearity & Headroom.** Continuous-time CMFB (using resistors) loads the output (reducing gain) and limits swing. SC-CMFB uses capacitors which act as open circuits at DC, preserving the high DC gain of the OpAmp.

**Challenges and Limitations**
- **Clock Feedthrough:** The switching action injects charge glitches ("spurs") into the bias line at the clock frequency $f_s$. These spurs can mix with the signal.
- **Sampled Nature:** It is a discrete-time loop. It can have stability issues if the CMFB loop bandwidth is too close to the switching frequency.

**Practical techniques to mitigate**
- **Small Caps:** Use the smallest possible capacitors for the CMFB network (just large enough to overcome leakage currents) to minimize loading and injection.
- **Filtering:** Add a small continuous-time capacitor to ground on the bias control node to smooth out the switching glitches.
