# Chapter 3: Single-Stage Amplifiers - Questions and Answers

## 3.1 Applications & 3.2 General Considerations

**Q1: What is the "Analog Octagon" and why is it important?**
**A1:** The Analog Octagon represents the multi-dimensional trade-offs in analog design involving Gain, Bandwidth, Swing, Power, Noise, Linearity, Supply Voltage, and Area. It highlights that improving one metric (e.g., Gain) usually degrades another (e.g., Bandwidth or Power), requiring designers to prioritize specs based on the application.

**Q2: Why are single-stage amplifiers considered the "atomic units" of analog design?**
**A2:** They are the simplest circuits capable of providing voltage gain, current buffering, or impedance transformation. Complex systems like OpAmps are essentially cascades of these single-stage building blocks (e.g., a differential pair followed by a common-source stage).

## 3.3 Common-Source (CS) Stage

**Q3: What is the primary function of a Common-Source stage?**
**A3:** It acts as a transconductance amplifier, converting an input voltage at the Gate into an output current at the Drain, which then flows through a load to produce voltage gain. It provides high input impedance and significant voltage gain ($A_v = -g_m R_{out}$).

**Q4: Explain the "Miller Effect" in a CS stage.**
**A4:** The Gate-Drain capacitance ($C_{gd}$) connects the input and output. Since the output is an amplified and inverted version of the input, the effective capacitance seen at the input is multiplied by the gain: $C_{in} \approx (1 + |A_v|) C_{gd}$. This significantly reduces the input bandwidth.

**Q5: Why is a Current-Source Load preferred over a Resistive Load for high gain?**
**A5:** To achieve high gain with a resistive load ($R_D$), $R_D$ must be very large, which causes a massive DC voltage drop ($I_D R_D$) that crushes the output swing. A current-source load (active load) provides a very high incremental resistance ($r_o$) for AC signals while maintaining a small, fixed DC voltage drop ($V_{DS,sat}$), allowing high gain without sacrificing headroom.

**Q6: What is the benefit of using a Diode-Connected Load?**
**A6:** A diode-connected load provides a gain determined by the ratio of device dimensions ($A_v \approx -\sqrt{(W/L)_{in} / (W/L)_{load}}$). This makes the gain relatively independent of process, voltage, and temperature (PVT) variations compared to other loads.

**Q7: How does Source Degeneration affect a CS stage?**
**A7:** Adding a resistor ($R_S$) in series with the source linearizes the amplifier by applying local negative feedback. It stabilizes the transconductance to $G_m \approx 1/R_S$ (independent of transistor parameters) and increases the linear input range, but at the cost of reduced gain and increased noise.

## 3.4 Source Follower (Common Drain)

**Q8: What is the main application of a Source Follower?**
**A8:** It is used as a voltage buffer. It has high input impedance and low output impedance ($1/g_m$), making it ideal for driving low-impedance loads (like resistors or large capacitors) without loading the preceding stage.

**Q9: Why is the gain of a Source Follower always less than 1?**
**A9:** The gain is given by $A_v = \frac{g_m R_L}{1 + (g_m + g_{mb}) R_L}$. Even with an infinite load, the body effect ($g_{mb}$) acts as a "back-gate" that fights the input, limiting the maximum gain to $\approx \frac{g_m}{g_m + g_{mb}}$ (typically 0.8 - 0.9).

## 3.5 Common-Gate Stage

**Q10: What is the key characteristic of a Common-Gate stage?**
**A10:** It has a very low input impedance ($\approx 1/g_m$). This makes it excellent for matching to low-impedance sources (like $50\Omega$ transmission lines in RF) or acting as a current buffer (converting input current to output voltage).

**Q11: Why is the Common-Gate stage often used in high-frequency circuits?**
**A11:** Because the Gate is grounded, there is no direct capacitive coupling between the input (Source) and output (Drain). This eliminates the Miller effect, allowing the stage to operate at much higher bandwidths than a Common-Source stage.

## 3.6 Cascode Stage

**Q12: How does a Cascode stage boost gain?**
**A12:** A Cascode stage stacks a Common-Gate device on top of a Common-Source device. The CG device shields the CS device from output voltage variations, effectively multiplying the output resistance by the intrinsic gain of the cascode device ($R_{out} \approx g_m r_o^2$). This allows for extremely high voltage gain in a single stage.

**Q13: What is the main disadvantage of a Telescopic Cascode?**
**A13:** Voltage headroom. Stacking two transistors requires a minimum output voltage of $V_{OV1} + V_{OV2} + V_{TH}$ (or similar depending on biasing) to keep both in saturation. This limits the available output swing, which is critical in low-voltage processes.

**Q14: What is a Folded Cascode and why is it used?**
**A14:** A Folded Cascode uses an input device and a cascode device of opposite types (e.g., PMOS input, NMOS cascode). The signal current is "folded" to the output branch. This topology allows the input and output voltage ranges to overlap and generally provides better input common-mode range and headroom performance than a telescopic cascode, making it suitable for low-voltage OpAmps.

## 3.7 Choice of Device Models

**Q15: Why is the "Square Law" model often insufficient for modern design?**
**A15:** Modern short-channel devices suffer from velocity saturation, making the current linearly proportional to overdrive ($I_D \propto V_{GS}-V_{TH}$) rather than quadratic. Additionally, output impedance is dominated by DIBL and SCBE rather than just channel length modulation.

**Q16: What is the $g_m/I_D$ design methodology?**
**A16:** It is a design approach that moves away from inaccurate analytic equations. Instead, designers use lookup tables generated from SPICE simulations to choose device sizes based on efficiency ($g_m/I_D$) and speed ($f_T$) targets, ensuring much higher accuracy in the initial design phase.
