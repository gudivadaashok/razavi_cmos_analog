# Chapter 6: Frequency Response of Amplifiers

## 6.1 General Considerations
**What the concept is**
Analyzing how the gain and phase of an amplifier change with frequency due to parasitic capacitances ($C_{gs}, C_{gd}, C_{db}, C_{sb}$) and load capacitance ($C_L$).

**Why it is used**
To determine the **Bandwidth** (speed) of the circuit and ensure **Stability**.

**The Bad / Backpack**
- **Complexity:** Every node in a circuit creates a pole. A simple circuit can have dozens of poles.
- **Approximation:** We must identify the "Dominant Poles" to make analysis manageable.

### 6.1.1 Miller Effect
**What the concept is**
A capacitor $C_F$ connected between the input and output of an inverting amplifier with gain $-A_v$ appears at the input as a much larger capacitor: $C_{in} = C_F (1 + A_v)$.

**Why it is used**
To explain why the Gate-Drain capacitance ($C_{gd}$) drastically limits the bandwidth of Common-Source amplifiers.

**The Bad / Backpack**
- **Input Impedance:** It kills the input impedance at high frequencies.
- **Pole Splitting:** It pushes the input pole down (lower frequency) and the output pole up.

### 6.1.2 Association of Poles with Nodes
**What the concept is**
A simplified way to find poles. For a node $j$, the pole is roughly $\omega_{p,j} = 1 / (R_{eq,j} C_{tot,j})$, where $R_{eq,j}$ is the resistance seen to ground and $C_{tot,j}$ is the total capacitance at that node.

**Why it is used**
**Inspection.** It allows us to estimate bandwidth without writing full transfer functions.

**The Bad / Backpack**
- **Interaction:** It assumes poles are decoupled (isolated). If capacitors connect two high-impedance nodes (like $C_{gd}$), this method fails (use Miller or Zero-Value Time Constant instead).

## 6.2 Common-Source Stage
**What the concept is**
The frequency response is dominated by two poles:
1.  **Input Pole:** $\omega_{in} \approx 1 / (R_S [C_{gs} + C_{gd}(1+A_v)])$. (Miller Multiplied).
2.  **Output Pole:** $\omega_{out} \approx 1 / (R_D C_L)$.

**Why it is used**
The baseline for comparison.

**The Bad / Backpack**
- **Miller Effect:** High gain ($A_v$) implies high input capacitance, killing the bandwidth if the source resistance $R_S$ is high.
- **RHP Zero:** $C_{gd}$ creates a Right-Half-Plane zero at $\omega_z = g_m / C_{gd}$, which degrades phase margin.

**Practical techniques to mitigate**
- **Low Source Impedance:** Drive the CS stage with a low-impedance source to push the input pole out.
- **Cascode:** Use a cascode to eliminate the Miller effect.

## 6.3 Source Followers
**What the concept is**
Ideally a wideband buffer.
- **Input Capacitance:** $C_{in} = C_{gd} + C_{gs}(1 - A_v)$. Since $A_v \approx 1$, the $C_{gs}$ term is "bootstrapped" and disappears. $C_{in}$ is very small.

**Why it is used**
High-speed buffering.

**The Bad / Backpack**
- **Inductive Output:** The output impedance $Z_{out}$ can look inductive at high frequencies. If driving a capacitive load, this forms an LC tank and can cause **ringing** or oscillation.
- **Negative Resistance:** In some conditions, the input impedance can have a negative real part, leading to instability.

**Practical techniques to mitigate**
- **Series Resistor:** Add a small resistor in series with the gate to dampen oscillations.

## 6.4 Common-Gate Stage
**What the concept is**
Non-inverting amplifier with low input impedance.
- **No Miller Effect:** The input and output are separated by the grounded gate. $C_{gd}$ is grounded.
- **Bandwidth:** Usually limited by the output pole $\omega_{out} = 1 / (R_D C_L)$.

**Why it is used**
High-frequency applications (LNA, TIA) where input matching is required and Miller effect must be avoided.

## 6.5 Cascode Stage
**What the concept is**
A CS stage driving a CG stage.
- **Miller Elimination:** The input CS stage sees a low load resistance ($1/g_{m,CG}$). The gain from Gate to Drain of the input device is small ($\approx -1$). Therefore, the Miller multiplication of $C_{gd}$ is negligible.

**Why it is used**
**Bandwidth Extension.** It achieves the high gain of a CS stage with the high bandwidth of a CG stage.

**The Bad / Backpack**
- **Extra Pole:** The internal node (Source of cascode) creates a pole, but it is usually at high frequency ($g_{m,CG} / C_{par}$).

## 6.6 Differential Pair
### 6.6.1 With Passive Loads
**What the concept is**
Similar to the Common-Source stage.
- **Differential Mode:** The tail node is a virtual ground. Half-circuit analysis applies. Bandwidth is determined by $R_D C_L$.
- **Common Mode:** The tail capacitance $C_{SS}$ creates a zero, causing CMRR to degrade at high frequencies.

### 6.6.2 With Active Load (Current Mirror)
**What the concept is**
The "Mirror Pole" issue.
- **Asymmetry:** The signal path through the current mirror has an extra pole at $\omega_p \approx g_m / C_{gs}$ (approx $f_T/2$).
- **Pole-Zero Doublet:** The asymmetry creates a pole-zero pair that affects settling time.

## 6.7 Gain-Bandwidth Trade-Offs
**What the concept is**
For a single-pole amplifier, the Gain-Bandwidth Product (GBW) is constant.
$GBW \approx \frac{g_m}{C_L}$.

**Why it is used**
**Fundamental Limit.** To get more speed ($BW$), you must burn more power (increase $g_m$) or reduce the load ($C_L$). You cannot simply increase gain without losing bandwidth.

### 6.7.1 One-Pole Circuits
Ideally, $A_v(s) = \frac{A_0}{1 + s/\omega_0}$.
$GBW = A_0 \omega_0$.

### 6.7.2 Multi-Pole Circuits
Adding more stages increases gain but adds more poles.
- **Shrinking Bandwidth:** Cascading identical stages reduces the overall bandwidth because the poles stack up. $\omega_{-3dB, tot} \approx \omega_{-3dB, stage} \sqrt{2^{1/n} - 1}$.

## 6.8 Appendix A: Extra Element Theorem (EET)
**What the concept is**
A powerful theorem by Middlebrook. It allows you to calculate the effect of adding a single element (like a capacitor) on the transfer function of a circuit without re-analyzing the whole circuit.

## 6.9 Appendix B: Zero-Value Time Constant (ZVTC)
**What the concept is**
A method to estimate the -3dB bandwidth by summing the time constants of each capacitor.
$\omega_{-3dB} \approx \frac{1}{\sum R_{j0} C_j}$.
- **Procedure:** Turn off independent sources. For each capacitor $C_j$, calculate the resistance $R_{j0}$ seen across its terminals while all other capacitors are open-circuited.

**Why it is used**
**Conservative Estimate.** It provides a quick, pessimistic estimate of the bandwidth.

## 6.10 Appendix C: Dual of Miller’s Theorem
**What the concept is**
Deals with an impedance connected in series with the common leg of a T-network (like source degeneration).
