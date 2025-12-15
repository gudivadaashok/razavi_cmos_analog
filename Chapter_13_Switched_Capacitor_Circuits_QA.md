# Chapter 13: Switched-Capacitor Circuits - Q&A

## 1. General Concepts

**Q: What is the fundamental principle of Switched-Capacitor (SC) circuits?**
**A:** SC circuits process analog signals using discrete-time sampling. A capacitor $C$ switched at a frequency $f_s$ behaves like an equivalent resistor $R_{eq} = \frac{1}{f_s C}$.

**Q: Why are SC circuits preferred over continuous-time resistor-based circuits in CMOS?**
**A:** **Accuracy.** While absolute resistor values in CMOS vary significantly ($\pm 20\%$), the *ratio* of capacitor areas ($C_1/C_2$) can be matched to within 0.1%. This allows for the design of extremely precise filters and gain stages.

**Q: What is the primary limitation of SC circuits regarding frequency?**
**A:** **Aliasing.** Since it is a sampled system, signals with frequencies above the Nyquist rate ($f_s/2$) fold back into the baseband.

**Q: How is aliasing mitigated in SC systems?**
**A:**
1.  **Anti-Aliasing Filter:** A continuous-time low-pass filter (usually RC) precedes the SC circuit.
2.  **Oversampling:** Using a clock frequency much higher than the signal bandwidth to relax the requirements on the anti-aliasing filter.

---

## 2. Sampling Switches

**Q: What device is typically used as a switch in SC circuits?**
**A:** A MOS transistor operating in the deep triode region.

**Q: What is the issue with using a simple MOSFET switch?**
**A:** **Signal-Dependent $R_{on}$.** The on-resistance depends on the overdrive voltage ($V_{GS} - V_{TH}$). Since the source is connected to the input signal ($V_{in}$), $V_{GS}$ varies with the signal, modulating $R_{on}$ and causing harmonic distortion.

**Q: How can the signal-dependent resistance be mitigated?**
**A:**
1.  **Transmission Gate:** Using parallel NMOS and PMOS transistors.
2.  **Bootstrapping:** Boosting the gate voltage to keep $V_{GS}$ constant regardless of the input signal level.

**Q: What determines the speed of the sampling process?**
**A:** The RC time constant formed by the switch resistance and the sampling capacitor: $\tau = R_{on} C_H$. The voltage must settle to the required accuracy (e.g., within 1 LSB) within half a clock period.

**Q: What is Charge Injection?**
**A:** When a MOS switch turns off, the charge stored in its channel ($Q_{ch}$) is released onto the sampling capacitor, causing an error voltage $\Delta V = Q_{ch}/C_H$.

**Q: How is Charge Injection Cancellation achieved?**
**A:**
1.  **Bottom-Plate Sampling:** Turning off the switch connected to ground (bottom plate) *first*. Since this node is at a fixed potential, the injected charge is constant (offset) rather than signal-dependent (distortion).
2.  **Differential Operation:** Using fully differential circuits so that charge injection appears as a common-mode error and is rejected.

---

## 3. SC Amplifiers

**Q: How does a Unity-Gain Sampler (Flip-Around) work?**
**A:**
1.  **Sampling ($\phi_1$):** The capacitor $C_H$ samples the input voltage ($V_{in}$).
2.  **Hold ($\phi_2$):** The bottom plate of $C_H$ connects to the output, and the top plate connects to the OpAmp input (virtual ground). The OpAmp forces the charge to stay on $C_H$, transferring $V_{in}$ to the output.

**Q: What is the advantage of the "Flip-Around" architecture?**
**A:** It has a feedback factor of $\beta \approx 1$, which maximizes speed and power efficiency.

**Q: How is a Non-inverting Amplifier implemented in SC circuits?**
**A:** Using a sampling capacitor $C_S$ and a feedback capacitor $C_F$.
1.  **Sampling:** $C_S$ charges to $V_{in}$.
2.  **Amplification:** Charge from $C_S$ is transferred to $C_F$.
   - Gain: $V_{out}/V_{in} = C_S / C_F$.

**Q: What is the main advantage of SC amplifiers over resistive amplifiers?**
**A:** The gain depends on the ratio of capacitor areas ($C_S/C_F$), which is highly accurate (0.1% matching) compared to resistor ratios.

**Q: What is a Precision Multiply-by-Two circuit used for?**
**A:** It is the core building block of **Pipelined ADCs**. It generates the residue for the next stage by amplifying the input by exactly 2.

---

## 4. SC Integrator

**Q: What is the function of an SC Integrator?**
**A:** It accumulates charge on a feedback capacitor $C_F$ without resetting it every cycle.
Time domain equation: $V_{out}[n] = V_{out}[n-1] + \frac{C_S}{C_F} V_{in}[n-1]$.

**Q: Where are SC Integrators commonly used?**
**A:** They are the fundamental building blocks of **Sigma-Delta ($\Sigma\Delta$) Modulators** and SC Filters.

**Q: What is "Integrator Leakage"?**
**A:** Ideally, an integrator has infinite DC gain (pole at $z=1$). Finite OpAmp gain moves the pole slightly inside the unit circle, causing the integrator to "leak" charge over time, which limits precision.

**Q: What is a "Parasitic-Insensitive" topology?**
**A:** A circuit configuration that ensures parasitic capacitances at the input and output nodes do not affect the charge transfer accuracy. This is achieved by switching the parasitic capacitors between ground and virtual ground (or voltage sources), so they never transfer net charge to the feedback capacitor.

---

## 5. SC Common-Mode Feedback (CMFB)

**Q: Why is SC CMFB preferred over continuous-time resistive CMFB?**
**A:** **Linearity and Headroom.** Resistive CMFB loads the amplifier output (reducing gain) and limits voltage swing. SC CMFB uses capacitors, which act as open circuits at DC, preserving the OpAmp's high DC gain and allowing larger swings.

**Q: How does SC CMFB work?**
**A:** It uses sensing capacitors to measure the output common-mode level and refresh switches to periodically recharge these capacitors to a desired reference voltage.

**Q: What is a potential issue with SC CMFB?**
**A:** **Clock Feedthrough.** The switching action injects charge glitches ("spurs") into the bias line at the clock frequency, which can mix with the signal.
