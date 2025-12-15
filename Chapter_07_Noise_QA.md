# Chapter 7: Noise - Questions and Answers

## 7.1 Statistical Characteristics of Noise

**Q1: Why is noise analyzed using statistical measures like Power Spectral Density (PSD) instead of time-domain waveforms?**
**A1:** Noise is a random process, meaning its instantaneous value cannot be predicted. However, its statistical properties, such as average power and frequency distribution, are deterministic and can be analyzed. PSD describes how noise power is distributed across frequencies, allowing designers to calculate the total noise power within a specific bandwidth.

**Q2: What is the difference between correlated and uncorrelated noise sources?**
**A2:** Uncorrelated noise sources arise from independent physical devices (e.g., two different resistors) and add in **power** (mean square values: $V_{tot}^2 = V_{n1}^2 + V_{n2}^2$). Correlated noise sources arise from the same physical mechanism (e.g., gate and drain noise of the same transistor at high frequencies) and add in **voltage** ($V_{tot}^2 = (V_{n1} + V_{n2})^2$).

**Q3: Define Signal-to-Noise Ratio (SNR) and its significance.**
**A3:** SNR is the ratio of signal power to noise power ($SNR = P_{signal} / P_{noise}$). It is the ultimate figure of merit for analog signal quality, determining the minimum detectable signal and the dynamic range of the system.

## 7.2 Types of Noise

**Q4: What is Thermal Noise and how is it modeled in resistors and MOSFETs?**
**A4:** Thermal noise is caused by the random thermal motion of charge carriers in a conductor.
*   **Resistor:** Modeled as a voltage source $V_n^2 = 4kTR \Delta f$.
*   **MOSFET:** Modeled as a drain current source $I_n^2 = 4kT \gamma g_m \Delta f$, where $\gamma \approx 2/3$ for long-channel devices.

**Q5: What is Flicker Noise (1/f Noise) and how can it be minimized?**
**A5:** Flicker noise is caused by charge trapping/detrapping at the silicon-oxide interface. Its PSD is inversely proportional to frequency ($S_v(f) \propto 1/f$). It can be minimized by increasing the gate area ($W \times L$) of the device or by using circuit techniques like chopping. PMOS devices often exhibit lower 1/f noise than NMOS devices.

**Q6: What is Shot Noise?**
**A6:** Shot noise arises from the discrete nature of charge carriers crossing a potential barrier (like a pn-junction). It is proportional to the DC current ($I_n^2 = 2qI_{DC} \Delta f$). It is dominant in BJTs and photodiodes but usually negligible in MOSFETs (except for gate leakage).

## 7.3 Representing Noise in Circuits

**Q7: Why do we use "Input-Referred Noise"?**
**A7:** Input-referred noise allows for a fair comparison of noise performance between different circuits regardless of their gain. It represents the equivalent noise voltage at the input that would produce the observed output noise if the circuit were noiseless. It is calculated as $V_{n,in}^2 = V_{n,out}^2 / A_v^2$.

## 7.4 Noise in Single-Stage Amplifiers

**Q8: How does the transconductance ($g_m$) of the input transistor affect the input-referred noise of a Common-Source stage?**
**A8:** The input-referred thermal noise voltage is inversely proportional to $g_m$ ($V_{n,in}^2 \approx 4kT\gamma / g_m$). Therefore, increasing $g_m$ (by increasing bias current or width) reduces the input-referred noise.

**Q9: What is the design rule for minimizing the noise contribution of an active load (current source)?**
**A9:** To minimize the noise contribution of an active load, its transconductance ($g_{m,load}$) should be made much smaller than the transconductance of the input device ($g_{m,in}$). This is typically achieved by using long channel lengths and low overdrive voltages for the load devices.

**Q10: Why is the Cascode device usually ignored in low-frequency noise analysis?**
**A10:** At low frequencies, the noise current of the cascode device circulates through the high impedance of the input device's drain and the cascode's source. This creates a source degeneration effect that heavily suppresses the cascode device's noise contribution to the output.

## 7.5 Noise in Current Mirrors

**Q11: How does a current mirror affect noise propagation?**
**A11:** A current mirror copies noise from the reference branch to the output branch, multiplying the noise power by the square of the mirroring ratio ($M^2$). Flicker noise from the diode-connected reference transistor is particularly critical and can be minimized by using large devices or filtering the bias voltage.

## 7.6 Noise in Differential Pairs

**Q12: How does the noise of a differential pair compare to a single-ended Common-Source stage?**
**A12:** A differential pair has two input transistors, both contributing uncorrelated noise. Therefore, the total input-referred noise power is twice that of a single-ended stage ($V_{n,in}^2 = 2 \times V_{n,device}^2$), resulting in a 3 dB noise penalty. However, this is often accepted for the benefit of differential operation (supply rejection).

## 7.7 Noise/Power Trade-Off

**Q13: What is the fundamental trade-off between noise and power consumption?**
**A13:** Thermal noise voltage is inversely proportional to the square root of the bias current ($V_n \propto 1/\sqrt{I_D}$). To reduce the noise voltage by a factor of 2 (6 dB), the power consumption must typically be increased by a factor of 4. This makes low-noise design power-hungry.

## 7.8 Noise Bandwidth

**Q14: What is "Noise Bandwidth"?**
**A14:** Noise bandwidth is the bandwidth of an ideal "brick-wall" filter that would pass the same total noise power as the actual circuit. For a single-pole system with 3-dB bandwidth $f_{3dB}$, the noise bandwidth is $(\pi/2) f_{3dB}$.

## 7.9 Input Noise Integration

**Q15: Why must we be careful when integrating 1/f noise?**
**A15:** The integral of $1/f$ from 0 to $\infty$ diverges. In practice, the lower limit of integration is set by the observation time ($1/T_{obs}$), as frequencies lower than this are indistinguishable from DC drift.
