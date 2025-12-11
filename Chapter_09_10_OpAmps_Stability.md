# Chapter 9: Operational Amplifiers

## Table of Contents
- [9.1 General Considerations](#91-general-considerations)
  - [9.1.1 Performance Parameters](#911-performance-parameters)
- [9.2 One-Stage Op Amps](#92-one-stage-op-amps)
  - [9.2.1 Basic Topologies](#921-basic-topologies)
  - [9.2.2 Design Procedure](#922-design-procedure)
  - [9.2.3 Linear Scaling](#923-linear-scaling)
  - [9.2.4 Folded-Cascode Properties](#924-folded-cascode-properties)
- [9.3 Two-Stage Op Amps](#93-two-stage-op-amps)
- [9.4 Gain Boosting (Regulated Cascode)](#94-gain-boosting-regulated-cascode)
- [9.5 Comparison](#95-comparison)
- [9.6 Output Swing Calculations](#96-output-swing-calculations)
- [9.7 Common-Mode Feedback (CMFB)](#97-common-mode-feedback-cmfb)
  - [9.7.1 Basic Concepts](#971-basic-concepts)
  - [9.7.2 CM Sensing Techniques](#972-cm-sensing-techniques)
  - [9.7.3 CM Feedback Techniques](#973-cm-feedback-techniques)
- [9.8 Input Range Limitations](#98-input-range-limitations)
- [9.9 Slew Rate](#99-slew-rate)
- [9.10 High-Slew-Rate Op Amps](#910-high-slew-rate-op-amps)
- [9.11 Power Supply Rejection (PSRR)](#911-power-supply-rejection-psrr)
- [9.12 Noise in Op Amps](#912-noise-in-op-amps)
- [10.1 General Considerations](#101-general-considerations)
- [10.2 Multipole Systems](#102-multipole-systems)
- [10.3 Phase Margin (PM)](#103-phase-margin-pm)
- [10.4 Basic Compensation](#104-basic-compensation)
- [10.5 Two-Stage Op-Amp Compensation (Miller)](#105-two-stage-op-amp-compensation-miller)
- [10.6 Slewing](#106-slewing)
- [10.7 Other Techniques](#107-other-techniques)
- [10.8 Nyquist Criterion](#108-nyquist-criterion)
  - [10.8.7 Multiple 180° Crossings](#1087-multiple-180-crossings)


## 9.1 General Considerations
**What the concept is**
The OpAmp is the fundamental building block of analog circuits. It is a high-gain, differential-input, single-ended output amplifier.

### 9.1.1 Performance Parameters
**Key Metrics:**
1.  **Gain:** Open-loop gain ($A_{OL}$). Determines the precision of feedback systems. Error $\propto 1/A_{OL}$.
2.  **Small-Signal Bandwidth:** Unity Gain Bandwidth ($f_{u}$). Determines the speed of processing small signals.
3.  **Large-Signal Speed (Slew Rate):** Maximum rate of change of output voltage ($dV/dt$).
4.  **Output Swing:** The range of output voltages for which all transistors remain in saturation.
5.  **Linearity:** Distortion performance.
6.  **Noise & Offset:** The lower limit of detectable signal.
7.  **Power Dissipation:** The cost of performance.

## 9.2 One-Stage Op Amps
**What the concept is**
Operational Transconductance Amplifiers (OTAs) where the output is taken from a high-impedance node directly connected to the input stage (or its cascode).

### 9.2.1 Basic Topologies
1.  **Telescopic Cascode:**
    *   *Structure:* Differential pair with cascode loads stacked directly on top.
    *   *Pros:* Highest speed (only NMOS in signal path), Low Power, Low Noise.
    *   *Cons:* Terrible Output Swing, Difficult to short input to output (buffer).
2.  **Folded Cascode:**
    *   *Structure:* Input pair (e.g., PMOS) folds down to a cascode load of the opposite type (NMOS).
    *   *Pros:* Good Input Common Mode Range, Output swing better than Telescopic, Input/Output shorting is easy.
    *   *Cons:* Lower speed (folding node pole), Higher power (two current legs), Higher noise.

### 9.2.2 Design Procedure
1.  Choose $L$ for required gain/channel length modulation.
2.  Choose $I_{tail}$ based on Slew Rate or Power budget.
3.  Size input devices ($W/L$) for $g_m$ (Bandwidth) or Noise.
4.  Size current source/load devices for Swing and Stability (parasitic poles).

### 9.2.3 Linear Scaling
**Concept:** If you scale all widths ($W$) and currents ($I$) by a factor $\alpha$, the Gain ($g_m r_o$) remains constant, but Bandwidth and Power increase by $\alpha$.

### 9.2.4 Folded-Cascode Properties
**The Bad / Backpack**
- **Folding Pole:** The node where the input folds into the cascode has capacitance. This creates a non-dominant pole that limits phase margin.

## 9.3 Two-Stage Op Amps
**What the concept is**
Separating the functions of Gain and Swing into two stages.
- **Stage 1:** High Gain (e.g., Differential Pair).
- **Stage 2:** High Swing (e.g., Common Source).

**Why it is used**
To achieve Rail-to-Rail output swing (CS stage) while maintaining high gain.

**Challenges and Limitations**
- **Stability:** Two high-impedance nodes = Two dominant poles = Oscillation.
- **Compensation:** Requires Miller Compensation ($C_C$) to split the poles.

## 9.4 Gain Boosting (Regulated Cascode)
**What the concept is**
Using an auxiliary amplifier to actively regulate the source voltage of a cascode transistor.
- **Effect:** Multiplies the output impedance by the gain of the auxiliary amp ($A_{aux}$).
- $R_{out} \approx g_m r_o^2 \cdot A_{aux}$.

**Why it is used**
To achieve "Two-Stage Gain" (e.g., 100dB) with "One-Stage Speed" (single dominant pole at output).

**Challenges and Limitations**
- **Doublets:** The auxiliary amp introduces its own poles, creating pole-zero doublets that can ruin settling time.

## 9.5 Comparison
| Topology | Gain | Swing | Speed | Power | Noise |
|----------|------|-------|-------|-------|-------|
| **Telescopic** | High | Low | Highest | Low | Low |
| **Folded Cascode** | Medium | Medium | High | Medium | Medium |
| **Two-Stage** | Highest | Highest | Low | Medium | Low |
| **Gain Boosted** | Very High | Low | Medium | High | Medium |

## 9.6 Output Swing Calculations
**Concept:**
- **Telescopic:** $V_{swing} = V_{DD} - 5 V_{OV}$. (Very limited).
- **Folded Cascode:** $V_{swing} = V_{DD} - 4 V_{OV}$.
- **Two-Stage:** $V_{swing} = V_{DD} - 2 V_{OV}$ (Rail-to-Rail).

## 9.7 Common-Mode Feedback (CMFB)
**What the concept is**
A negative feedback loop required for fully differential amplifiers to define the output Common-Mode (CM) voltage.

### 9.7.1 Basic Concepts
- Differential feedback defines $V_{out,diff}$.
- CMFB defines $V_{out,CM}$. Without it, $V_{out,CM}$ drifts to the rails.

### 9.7.2 CM Sensing Techniques
- **Resistive Sensing:** Resistors between outputs. *Problem:* Loads the gain.
- **MOSFET Sensing:** Triode devices. *Problem:* Limited range.

### 9.7.3 CM Feedback Techniques
- Control the tail current source or the active load current source to adjust the output CM level.

## 9.8 Input Range Limitations
**Concept:**
The range of input CM voltages for which the input differential pair remains in saturation.
- **NMOS Pair:** $V_{in,min} = V_{GS} + V_{tail}$.
- **PMOS Pair:** $V_{in,max} = V_{DD} - (V_{SG} + V_{tail})$.
- **Rail-to-Rail Input:** Requires parallel NMOS and PMOS pairs.

## 9.9 Slew Rate
**What the concept is**
The maximum rate at which the output can change. $SR = I_{tail} / C_{L}$ (for one-stage) or $I_{tail} / C_C$ (for two-stage).

**Why it is used**
Large-signal speed limit.

**Challenges and Limitations**
- **Trade-off:** To double SR, you must double $I_{tail}$ (Power).

## 9.10 High-Slew-Rate Op Amps
**What the concept is**
Techniques to boost Slew Rate without burning static power.
- **Class AB Input:** Input stage current increases dynamically with input step size.
- **Slew Boosting:** Auxiliary circuit detects slewing and injects extra current.

## 9.11 Power Supply Rejection (PSRR)
**What the concept is**
The ability to reject noise on $V_{DD}$ and Ground.
- $PSRR = A_{diff} / A_{supply}$.

**Challenges and Limitations**
- **High Frequency:** PSRR degrades at high frequency because $C_{gd}$ couples supply noise to the output.

## 9.12 Noise in Op Amps
**What the concept is**
Input-referred noise.
- **Thermal Noise:** $\overline{V_n^2} = 4kT (2/3 g_m)$.
- **Flicker Noise:** $\overline{V_n^2} = K / (C_{ox} W L f)$.

**Practical techniques to mitigate**
- **Input Stage:** Maximize $g_m$ (for thermal) and Area (for flicker) of the input pair. The input stage noise dominates; subsequent stages are divided by $A_1$.

# Chapter 10: Stability and Frequency Compensation

## 10.1 General Considerations
**What the concept is**
Ensuring that a feedback system does not turn into an oscillator.

**Why it is used**
Negative feedback is essential for linearity, but if the phase shift reaches 180° while the loop gain is > 1, the feedback becomes positive, and the circuit oscillates.

**Challenges and Limitations**
- **Paranoia:** Designers must ensure stability not just typically, but across all Process, Voltage, and Temperature (PVT) corners and load conditions.

**Practical techniques to mitigate**
- **Worst-Case Sim:** Always simulate stability with the maximum expected load capacitance and minimum temperature (highest gain).

## 10.2 Multipole Systems
**What the concept is**
Real circuits have capacitance at every node. Each node creates a pole. Each pole contributes -90° of phase shift.

**Why it is used**
To model the frequency response.

**Challenges and Limitations**
- **Phase Accumulation:** 3 poles = -270° phase shift. Oscillation is almost guaranteed if you close the loop.

**Practical techniques to mitigate**
- **Dominant Pole:** You must force one pole to be at a much lower frequency than all others. This ensures the gain drops to < 1 before the phase shift reaches -180°.

## 10.3 Phase Margin (PM)
**What the concept is**
The difference between the phase shift and -180° at the "Unity Gain Frequency" (where Loop Gain = 1).

**Why it is used**
**Settling Metric.**
- PM = 45°: Ringing (oscillatory settling).
- PM = 60°: Fast, clean settling (optimal).
- PM = 90°: Slow, overdamped.

**Challenges and Limitations**
- **Trade-off:** Higher PM usually means lower Bandwidth.

**Practical techniques to mitigate**
- **Target 60°:** Aim for 60-65° in nominal conditions to ensure > 45° in worst-case corners.

## 10.4 Basic Compensation
**What the concept is**
Intentionally adding a large capacitor to one node to lower its pole frequency.

**Why it is used**
To create a Dominant Pole.

**Challenges and Limitations**
- **Bandwidth Killer:** It drastically reduces the bandwidth of the amplifier.

**Practical techniques to mitigate**
- **Output Compensation:** In one-stage OTAs, the load capacitor *is* the compensation. This is efficient because driving a larger load makes the system *more* stable (slower).

## 10.5 Two-Stage Op-Amp Compensation (Miller)
**What the concept is**
Connecting a capacitor $C_C$ between the input and output of the second gain stage.

**Why it is used**
**Pole Splitting.** The Miller effect multiplies $C_C$ by the gain of the second stage, creating a massive capacitance at the first stage output (Dominant Pole). Simultaneously, it pushes the output pole to a *higher* frequency.

**Challenges and Limitations**
- **RHP Zero:** The capacitor $C_C$ provides a feedforward path for current at high frequencies, creating a Right Half Plane Zero. This zero adds phase lag (like a pole) but increases gain, making stability worse.

**Practical techniques to mitigate**
- **Nulling Resistor:** Place a resistor $R_z$ in series with $C_C$. $R_z \approx 1/g_{m2}$. This pushes the RHP zero to infinity or cancels the non-dominant pole.

## 10.6 Slewing
**What the concept is**
The maximum rate at which the output voltage can change ($dV/dt$).

**Why it is used**
Large-signal speed limit.

**Challenges and Limitations**
- **Miller Limit:** In a Miller-compensated OpAmp, the slew rate is limited by the current available to charge the compensation capacitor: $SR = I_{tail} / C_C$.

**Practical techniques to mitigate**
- **Increase Tail Current:** Simple but power-hungry.
- **Slew Rate Enhancement:** Use auxiliary circuits that detect large input steps and temporarily inject extra current to charge $C_C$ faster.

## 10.7 Other Techniques
**What the concept is**
Feedforward compensation, Nested Miller Compensation (NMC).

**Why it is used**
For multi-stage amplifiers (3+ stages) needed in low-voltage designs.

**Challenges and Limitations**
- **Pole-Zero Doublets:** These complex schemes often create closely spaced pole-zero pairs that cause "slow tails" in the step response (settling takes forever).

**Practical techniques to mitigate**
- **Avoid if possible:** Stick to 1 or 2 stages if the supply voltage permits.

## 10.8 Nyquist Criterion
**What the concept is**
A rigorous mathematical test for stability involving plotting the loop gain in the complex plane.

**Why it is used**
**Ambiguity.** Bode plots can be misleading for "conditionally stable" systems or systems with RHP poles. Nyquist is the absolute truth.

**Challenges and Limitations**
- **Unintuitive:** Hard to visualize compared to Bode plots.

**Practical techniques to mitigate**
- **Use for Debug:** If a circuit oscillates in SPICE but the Bode plot looks fine, check the Nyquist plot. You might have a conditional stability issue.

### 10.8.7 Multiple 180° Crossings
**What the concept is**
The phase dips below -180° and comes back up before the gain crossover.

**Why it is used**
**Conditional Stability.** The system is stable *only* if the gain remains high.

**Challenges and Limitations**
- **Startup/Clipping:** If the output clips (gain drops to zero) or during power-up, the gain drops, and the system enters the unstable region and oscillates.

**Practical techniques to mitigate**
- **Ban it:** Never design conditionally stable systems for general-purpose use.
