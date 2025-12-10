# Chapter 5: Current Mirrors and Biasing Techniques

## 5.1 Basic Current Mirrors
**What the concept is**
A circuit that copies a reference current ($I_{REF}$) to an output branch ($I_{OUT}$). It consists of a diode-connected "master" device and a "slave" device sharing the same $V_{GS}$.

**Why it is used**
**Biasing.** Instead of distributing bias *voltages* (which is dangerous because $V_{TH}$ varies), we distribute *currents*. A central "Golden Current" is generated and mirrored to every block on the chip.

**The Bad / Backpack**
- **Channel Length Modulation:** If $V_{DS}$ of the slave differs from the master, the currents won't match due to the finite output resistance ($r_o$). $I_{out} \neq I_{ref}$.
- **Systematic Mismatch:** If the layout environments differ (e.g., one has a neighbor, one doesn't), stress effects cause mismatch.

**Practical techniques to mitigate**
- **Length Scaling:** Use non-minimum channel lengths ($L > 2-3 \times L_{min}$) for mirrors. This increases $r_o$ (reducing $V_{DS}$ sensitivity) and improves matching (Area $\propto WL$).
- **Dummy Devices:** Place dummy transistors on the edges of the mirror array to ensure uniform etching and stress.

## 5.2 Cascode Current Mirrors
**What the concept is**
Stacking a second transistor (cascode) on top of the mirror device.

**Why it is used**
**Precision.** It boosts the output impedance to $g_m r_o^2$. This shields the bottom transistor from $V_{out}$ variations, ensuring $I_{out}$ is incredibly constant (acts like an ideal current source).

**The Bad / Backpack**
- **Headroom Penalty:** Standard cascode requires $V_{TH} + 2V_{OV}$ headroom. This is painful in low-voltage designs.

**Practical techniques to mitigate**
- **Low-Voltage Cascode:** Use a specific bias voltage for the cascode gate that allows the bottom device to operate at the edge of saturation ($V_{DS} \approx V_{OV}$). This reduces headroom requirement to $2V_{OV}$.

## 5.3 Active Current Mirrors
**What the concept is**
A differential pair with a current mirror acting as the load.

**Why it is used**
**Diff-to-Single Conversion.** It takes a differential input current and combines it into a single-ended output voltage. It preserves the gain of both sides (unlike discarding one side).

**The Bad / Backpack**
- **Mirror Pole:** The internal node of the mirror creates a pole at $f \approx g_m / C_{gs}$. This is often close to the signal bandwidth, degrading phase margin.
- **Asymmetry:** The signal path for the positive and negative inputs is different (one goes through the mirror, one is direct). This causes poor high-frequency CMRR.

**Practical techniques to mitigate**
- **Pole Splitting:** Ensure the load capacitance $C_L$ is large enough that the output pole is dominant, separating it from the mirror pole.

### 5.3.1 Large-Signal
**What the concept is**
Slew Rate behavior. When a large step is applied, one side turns off, and the entire tail current $I_{SS}$ flows into (or out of) the load capacitor.

**Why it is used**
To determine the maximum speed of the amplifier for large signals.

**The Bad / Backpack**
- **Current Limit:** Slew rate is strictly limited by $I_{SS}$. $SR = I_{SS} / C_L$.

**Practical techniques to mitigate**
- **Power vs. Speed:** If you need faster slewing, you must burn more static power ($I_{SS}$), or use Class-AB input stages (which are complex).

### 5.3.2 Small-Signal
**What the concept is**
Gain analysis. $G_m \approx g_{m,in}$. $R_{out} \approx r_{o,n} || r_{o,p}$. Gain $A_v = G_m R_{out}$.

**Why it is used**
To design for DC gain and Bandwidth.

**The Bad / Backpack**
- **Doublet:** Due to the asymmetry, a pole-zero doublet exists which can affect settling time in precision applications.

**Practical techniques to mitigate**
- **Avoid in High-Speed:** For very high-speed paths, fully differential topologies are preferred over active mirrors to avoid the mirror pole and doublet issues.

### 5.3.3 Common-Mode Properties
**What the concept is**
Input Common Mode Range (ICMR). The range of input voltages where all transistors remain in saturation.

**Why it is used**
To ensure the amplifier works near the rails.

**The Bad / Backpack**
- **Dead Zones:** A standard NMOS pair cannot work near ground. A PMOS pair cannot work near VDD.

**Practical techniques to mitigate**
- **Rail-to-Rail Input:** Use parallel NMOS and PMOS input pairs to cover the full range. (Warning: $g_m$ varies as you switch between pairs, causing non-linearity).

### 5.3.4 Five-Transistor OTA Properties
**What the concept is**
The simplest operational transconductance amplifier. Input pair + Current Mirror Load + Tail Source.

**Why it is used**
**Efficiency.** It is the most power-efficient way to get gain. Used extensively as an internal buffer or in non-critical loops.

**The Bad / Backpack**
- **Noise:** The noise of the current mirror load contributes directly to the input.
- **Gain:** Limited to $\sim 40dB$ in modern processes.

**Practical techniques to mitigate**
- **Noise Optimization:** Make the $g_m$ of the load devices small (long L) and the $g_m$ of the input devices large (short L, wide W). Noise $\propto g_{m,load}/g_{m,in}$.

## 5.4 Biasing Techniques
**What the concept is**
Circuits that establish the stable DC operating points ($I_D$, $V_{GS}$) for the main amplifiers.

**Why it is used**
To make circuit performance independent of Supply Voltage ($V_{DD}$) and Temperature ($T$).

**The Bad / Backpack**
- **Positive Feedback:** High-performance bias loops (like Beta-multiplier) use positive feedback and can have a "zero-current" stable state (circuit doesn't start).

**Practical techniques to mitigate**
- **Start-up Circuits:** Always include a "kicker" circuit that detects if the bias is zero and injects current to wake it up, then turns off.

### 5.4.1 CS Biasing
**What the concept is**
Setting the gate voltage of a CS stage to define its current.

**Why it is used**
To define $g_m$ and swing.

**The Bad / Backpack**
- **Resistive Dividers:** Never bias a gate using a resistive divider from VDD. $V_{GS}$ will vary with VDD, and $I_D$ will vary wildly.

**Practical techniques to mitigate**
- **Current Mirroring:** Always generate a current using a robust reference (Bandgap) and mirror it to the CS stage.

### 5.4.2 CG Biasing
**What the concept is**
Setting the gate voltage of the Common Gate device.

**Why it is used**
To maximize headroom for the device below it.

**The Bad / Backpack**
- **Body Effect:** The source of the CG device moves, modulating $V_{TH}$.

**Practical techniques to mitigate**
- **Local Bias Generation:** Generate the bias voltage locally using a "diode-connected" stack that mimics the main branch structure.

### 5.4.3 Source Follower Biasing
**What the concept is**
Defining the current source load for the follower.

**Why it is used**
To ensure the follower can drive the load (sinking current).

**The Bad / Backpack**
- **Asymmetric Slewing:** A source follower can source lots of current (via the transistor) but can only sink fixed current (via the bias source).

**Practical techniques to mitigate**
- **Push-Pull:** Use a "Super Source Follower" or Class-AB follower if symmetric drive is needed.

### 5.4.4 Differential Pair Biasing
**What the concept is**
Setting the tail current $I_{SS}$.

**Why it is used**
It defines the Power, Slew Rate, and $G_m$ of the diff pair.

**The Bad / Backpack**
- **Noise Injection:** Noise from the tail current source appears as Common Mode noise. If CMRR is poor, it becomes differential noise.

**Practical techniques to mitigate**
- **Filtering:** Place a large bypass capacitor on the gate of the tail current source to shunt noise to ground.
