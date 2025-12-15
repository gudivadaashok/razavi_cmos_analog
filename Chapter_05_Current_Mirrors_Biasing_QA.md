# Chapter 5: Current Mirrors and Biasing Techniques - Questions and Answers

## 5.1 Basic Current Mirrors

**Q1: What is the fundamental purpose of a current mirror?**
**A1:** A current mirror is designed to copy a reference current ($I_{REF}$) to one or more output branches ($I_{OUT}$). This allows a single precise "golden current" to be distributed to various blocks on a chip, ensuring consistent biasing without the risks associated with distributing voltage biases (which are sensitive to $V_{TH}$ variations).

**Q2: How does a basic current mirror work physically?**
**A2:** It relies on the principle that if two identical MOS transistors have the same Gate-Source voltage ($V_{GS}$), they will conduct the same Drain current ($I_D$), assuming they are both in saturation. A "master" transistor is diode-connected (Drain shorted to Gate) to generate the specific $V_{GS}$ required to support $I_{REF}$. This $V_{GS}$ is then applied to the Gate of the "slave" transistor, forcing it to conduct a copy of the current.

**Q3: What is the primary source of error in a basic current mirror?**
**A3:** Channel Length Modulation is the primary source of systematic error. If the Drain-Source voltage ($V_{DS}$) of the output transistor differs from that of the reference transistor, the currents will not match due to the finite output resistance ($r_o$). The error is proportional to $\lambda (V_{DS,out} - V_{DS,ref})$.

**Q4: Why are non-minimum channel lengths ($L > L_{min}$) preferred for current mirrors?**
**A4:** Using larger channel lengths increases the output resistance ($r_o \propto L$), which reduces the impact of channel length modulation and improves current matching. It also increases the gate area ($WL$), which reduces random mismatch between devices.

## 5.2 Cascode Current Mirrors

**Q5: What is the main advantage of a Cascode Current Mirror over a basic mirror?**
**A5:** A Cascode Current Mirror significantly increases the output impedance ($R_{out} \approx g_m r_o^2$) compared to a basic mirror ($R_{out} \approx r_o$). This high impedance shields the current source transistor from variations in the output voltage, making the output current much more constant and precise (closer to an ideal current source).

**Q6: What is the "Headroom Penalty" associated with standard cascode mirrors?**
**A6:** A standard cascode structure requires a minimum voltage drop of $V_{TH} + 2V_{OV}$ to keep both transistors in saturation. This consumes a significant portion of the available voltage headroom, which is problematic in modern low-voltage processes.

**Q7: How does a "Low-Voltage Cascode" mitigate the headroom issue?**
**A7:** A Low-Voltage Cascode biases the gate of the cascode device such that the bottom transistor operates at the very edge of saturation ($V_{DS} \approx V_{OV}$). This reduces the minimum required voltage drop across the mirror to approximately $2V_{OV}$, saving one threshold voltage ($V_{TH}$) of headroom.

## 5.3 Active Current Mirrors

**Q8: What is the function of an Active Current Mirror in a differential amplifier?**
**A8:** An Active Current Mirror serves as the load for a differential pair. It performs a differential-to-single-ended conversion, combining the signal currents from both sides of the differential pair into a single output node. This preserves the gain of the differential stage without discarding half the signal.

**Q9: What is the "Mirror Pole" and why is it a problem?**
**A9:** The internal node of the active current mirror (the gate connection of the mirror devices) has capacitance ($C_{gs}$) and resistance ($1/g_m$), creating a pole at $f \approx g_m / C_{gs}$. This pole is often close to the signal bandwidth and introduces phase shift, which can degrade the phase margin and stability of the amplifier.

**Q10: Describe the "Slew Rate" limitation in an OTA with an active current mirror.**
**A10:** The slew rate is the maximum rate at which the output voltage can change. It is limited by the total tail current ($I_{SS}$) available to charge or discharge the load capacitance ($C_L$). $SR = I_{SS} / C_L$. To increase slew rate, one must increase the static power consumption ($I_{SS}$).

**Q11: Why does an active current mirror have poor Common-Mode Rejection Ratio (CMRR) at high frequencies?**
**A11:** The signal path for the two inputs is asymmetrical. One input goes directly to the output, while the other goes through the current mirror (which adds a pole). This asymmetry causes the common-mode gain to increase at high frequencies, degrading the CMRR.

## 5.4 Biasing Techniques

**Q12: Why is "Resistive Divider Biasing" generally avoided in integrated circuits?**
**A12:** Biasing a MOSFET gate with a resistive divider from the supply voltage ($V_{DD}$) makes the gate voltage dependent on $V_{DD}$. Since the drain current is a strong function of $V_{GS}$ (square law), any noise or variation in the supply voltage causes large variations in the bias current. It also relies on the absolute value of $V_{TH}$, which varies significantly with process and temperature.

**Q13: What is the purpose of a "Start-up Circuit" in bias generators?**
**A13:** High-performance bias circuits (like Beta-multiplier references) often use positive feedback loops that have two stable states: the desired operating point and a "zero-current" state. A start-up circuit detects if the circuit is in the zero-current state and injects a small current to "kick" the loop into the desired operating point, after which it turns itself off.

**Q14: How does "Common-Gate Biasing" differ from Common-Source biasing?**
**A14:** In Common-Gate biasing, the input signal is applied to the source, so the source cannot be grounded. The bias current is typically established by a current source connected to the source terminal. This current source must be high-impedance to avoid attenuating the input signal.

**Q15: What is the main challenge in biasing a "Source Follower" (Common Drain) stage?**
**A15:** A source follower is typically biased with a current source load. The main challenge is "Asymmetric Slewing." The transistor can actively source a large amount of current to pull the output up, but the pull-down is limited to the fixed bias current of the load. This leads to different slew rates for rising and falling edges.

**Q16: What is the "Input Common Mode Range" (ICMR) of a differential pair?**
**A16:** The ICMR is the range of input common-mode voltages for which all transistors in the differential pair and the tail current source remain in saturation.
*   **Lower Limit (NMOS):** $V_{in,min} = V_{GS} + V_{DS,sat(tail)}$.
*   **Upper Limit (NMOS):** $V_{in,max} = V_{DD} - V_{DS,sat(load)} + V_{TH}$ (approx, depends on load type).
If the input voltage goes outside this range, the amplifier gain drops significantly or the circuit stops working.
