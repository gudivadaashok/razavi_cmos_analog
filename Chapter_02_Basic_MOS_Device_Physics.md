# Chapter 2: Basic MOS Device Physics

## Table of Contents
- [2.1 MOSFET as a Switch](#21-mosfet-as-a-switch)
- [2.2 MOS I/V Characteristics (Saturation vs Triode)](#22-mos-iv-characteristics-saturation-vs-triode)
- [2.3 Second-Order Effects](#23-second-order-effects)
  - [2.3.1 Body Effect](#231-body-effect)
  - [2.3.2 Channel Length Modulation (CLM)](#232-channel-length-modulation-clm)
  - [2.3.3 Subthreshold Conduction](#233-subthreshold-conduction)
- [2.4 Small Signal Model](#24-small-signal-model)
  - [2.1.3 MOS Symbols](#213-mos-symbols)
- [2.2 MOS I/V Characteristics](#22-mos-iv-characteristics)
  - [2.2.1 Threshold Voltage](#221-threshold-voltage)
  - [2.2.2 Derivation of I/V Characteristics](#222-derivation-of-iv-characteristics)
  - [2.2.3 MOS Transconductance](#223-mos-transconductance)
- [2.3 Second-Order Effects](#23-second-order-effects)
  - [1. Channel-Length Modulation](#1-channel-length-modulation)
  - [2. Body Effect](#2-body-effect)
  - [3. Subthreshold Conduction](#3-subthreshold-conduction)
  - [4. Mobility Degradation](#4-mobility-degradation)
  - [5. Velocity Saturation](#5-velocity-saturation)
  - [6. Short-Channel Effects](#6-short-channel-effects)
  - [7. Temperature Effects](#7-temperature-effects)
- [2.4 MOS Device Models](#24-mos-device-models)
  - [2.4.1 MOS Device Layout](#241-mos-device-layout)
  - [2.4.2 MOS Device Capacitances](#242-mos-device-capacitances)
  - [2.4.3 MOS Small-Signal Model](#243-mos-small-signal-model)
  - [2.4.4 MOS SPICE Models](#244-mos-spice-models)
  - [2.4.5 NMOS Versus PMOS Devices](#245-nmos-versus-pmos-devices)
  - [2.4.6 Long-Channel Versus Short-Channel Devices](#246-long-channel-versus-short-channel-devices)
- [2.5 Appendix A: FinFETs](#25-appendix-a-finfets)
  - [Structure](#structure)
  - [Key Features](#key-features)
- [2.6 Appendix B: Behavior of a MOS Device as a Capacitor](#26-appendix-b-behavior-of-a-mos-device-as-a-capacitor)
  - [Gate-Channel Capacitor](#gate-channel-capacitor)
  - [Advantages](#advantages)
  - [Disadvantages](#disadvantages)
  - [Design Techniques](#design-techniques)
- [Summary](#summary)
  - [Key Concepts](#key-concepts)
  - [Design Parameters](#design-parameters)
- [2.6 Common Questions: Device Geometry](#26-common-questions-device-geometry)


## 2.1 MOSFET as a Switch
**What the concept is**
Using the MOSFET as a voltage-controlled resistor. In the "Deep Triode" region ($V_{DS} \ll 2(V_{GS}-V_{TH})$), the channel acts like a linear resistor controlled by the Gate voltage.

**Why it is used**
**Sampling & Logic.** It is the basis of all digital logic (CMOS) and analog sampling circuits (Sample-and-Hold). It allows us to pass or block signals with zero static power.

**Challenges and Limitations**
- **On-Resistance Variation:** $R_{on} \approx \frac{1}{\mu C_{ox} \frac{W}{L} (V_{GS} - V_{TH})}$. Since $V_{GS} = V_{Gate} - V_{in}$, the resistance changes as the signal $V_{in}$ changes. This causes harmonic distortion.
- **Charge Injection:** When the switch turns off, the charge stored in the channel ($Q = W L C_{ox} V_{OV}$) must go somewhere. It dumps onto the source and drain, causing a voltage error.

**Practical techniques to mitigate**
- **Transmission Gate:** Use parallel NMOS and PMOS. As $V_{in}$ rises, NMOS $R_{on}$ increases but PMOS $R_{on}$ decreases, keeping the total resistance roughly constant.
- **Dummy Switch:** Add a half-sized dummy transistor shorted to the signal path to absorb the injected charge.

## 2.2 MOS I/V Characteristics (Saturation vs Triode)
**What the concept is**
- **Triode:** The channel is continuous from Source to Drain. Acts like a resistor.
- **Saturation:** The channel is "pinched off" at the drain end. Current is (mostly) independent of $V_{DS}$. Acts like a Current Source.

**Why it is used**
**Amplification.** We almost always operate transistors in **Saturation** for analog amplifiers because we want high output impedance (current source behavior) to achieve high voltage gain ($A_v = -g_m R_{out}$).

**Challenges and Limitations**
- **Headroom:** To stay in saturation, we need $V_{DS} \ge V_{GS} - V_{TH}$ (Overdrive Voltage, $V_{OV}$). This "eats" voltage headroom, limiting signal swing.
- **Velocity Saturation:** In modern short-channel devices, the current doesn't follow the square law ($I \propto V_{OV}^2$) but becomes linear ($I \propto V_{OV}$), reducing the available transconductance efficiency ($g_m/I_D$).

**Practical techniques to mitigate**
- **Low $V_{OV}$:** Design with low overdrive voltages (e.g., 150mV-200mV) to maximize swing, but ensure it's not so low that you enter weak inversion (unless intended).

## 2.3 Second-Order Effects

### 2.3.1 Body Effect
**What the concept is**
The threshold voltage $V_{TH}$ changes when the Source is not at the same potential as the Bulk (Body). $V_{TH}$ increases as $V_{SB}$ increases.

**Why it is used**
It's usually *not* used intentionally; it's a parasitic effect. However, it can be used for "Body Biasing" to dynamically adjust leakage vs. speed in digital circuits.

**Challenges and Limitations**
- **Gain Reduction:** In a Source Follower, the body effect acts like a "back gate" that fights the main gate, reducing the gain from $\approx 1$ to $\approx \frac{g_m}{g_m + g_{mb}}$ (approx 0.8).
- **Headroom Loss:** In stacked circuits (cascodes), the top devices have high $V_{SB}$, increasing their $V_{TH}$ and eating more headroom.

**Practical techniques to mitigate**
- **Deep N-Well:** Use a triple-well process to isolate the NMOS bulk and tie it to the source ($V_{SB}=0$), eliminating the body effect. (Standard for PMOS, as N-wells are usually separate).

### 2.3.2 Channel Length Modulation (CLM)
**What the concept is**
As $V_{DS}$ increases, the pinch-off point moves towards the source, effectively shortening the channel length $L$. This causes current to increase slightly with $V_{DS}$.

**Why it is used**
It models the finite output impedance of the transistor ($r_o$).

**Challenges and Limitations**
- **Finite Gain:** An ideal current source has infinite impedance. CLM limits the impedance to $r_o \propto L/I_D$. This limits the maximum intrinsic gain of a single transistor to $g_m r_o$.

**Practical techniques to mitigate**
- **Long Channels:** Use $L > L_{min}$ (e.g., 2x to 5x min length) for current mirrors and gain stages to increase $r_o$ and gain.
- **Cascode:** Stack devices to boost the output impedance.

### 2.3.3 Subthreshold Conduction
**What the concept is**
Current doesn't drop to zero immediately below $V_{TH}$. It decays exponentially (like a BJT).

**Why it is used**
**Ultra-Low Power.** In "Subthreshold" or "Weak Inversion" design, we operate here intentionally to get maximum gain efficiency ($g_m/I_D$) at very low currents (nA range).

**Challenges and Limitations**
- **Speed:** It is very slow. $f_T$ is tiny.
- **Leakage:** In digital, this is "leakage" that drains the battery when the chip is sleeping.

**Practical techniques to mitigate**
- **High $V_{TH}$ Devices:** Use special high-threshold transistors for non-critical paths to reduce leakage.

## 2.4 Small Signal Model
**What the concept is**
Linearizing the non-linear MOSFET around a DC operating point (Bias point) to analyze AC signals. Key parameters: $g_m$ (transconductance), $r_o$ (output resistance), $C_{gs}, C_{gd}$ (capacitances).

**Why it is used**
To calculate Gain, Bandwidth, and Impedance using linear circuit theory (KCL/KVL).

**Challenges and Limitations**
- **Approximation:** It is only valid for *small* signals. If the signal swing is large, the parameters ($g_m$) change, causing distortion.

**Practical techniques to mitigate**
- **Large Signal Verification:** Always verify small-signal hand calculations with transient simulations (SPICE) to check for slew rate limits and distortion.

Understanding the physical structure is crucial for understanding device behavior.

#### Cross-Sectional View (NMOS)

```
        Gate (G)
           |
      [Metal/Poly]
           |
    [SiO₂ Oxide]
    ________________
   |                |
   |   p-substrate  |
   |                |
 n+|                |n+
   |                |
Drain(D)      Source(S)
```

**Key Components:**

1. **Substrate (Body/Bulk)**: 
   - p-type silicon for NMOS
   - n-type silicon for PMOS
   - Provides mechanical support
   - Usually connected to ground (NMOS) or V_DD (PMOS)

2. **Source and Drain**:
   - Heavily doped regions (n+ for NMOS, p+ for PMOS)
   - Symmetric in structure (interchangeable in many cases)
   - Source: Terminal from which carriers originate
   - Drain: Terminal where carriers are collected

3. **Gate Oxide**:
   - Thin insulating layer (SiO₂)
   - Thickness: 1-10nm in modern processes
   - Provides electrical isolation between gate and channel
   - Critical for device operation

4. **Gate Terminal**:
   - Historically metal, now typically polysilicon
   - Controls channel formation
   - Separated from substrate by oxide (insulated)

#### Channel Formation

**Without Gate Voltage (V_GS = 0):**
- No conducting path between drain and source
- Only reverse-biased p-n junctions
- Negligible current flow

**With Gate Voltage (V_GS > V_TH):**
- Electric field attracts minority carriers to surface
- **Inversion layer** forms (n-type channel in p-substrate)
- Conducting path established between drain and source
- Current can flow

#### Physical Dimensions

**Key Parameters:**

1. **Length (L)**:
   - Distance between source and drain
   - Channel length
   - Critical dimension (e.g., 28nm, 14nm, 7nm technology)

2. **Width (W)**:
   - Perpendicular to length
   - Determines current capacity
   - Designer-controllable parameter

3. **Oxide Thickness (t_ox)**:
   - Gate oxide thickness
   - Determines oxide capacitance: $C_{ox} = \frac{\epsilon_{ox}}{t_{ox}}$
   - Thinner oxide → stronger gate control

**Aspect Ratio:**

$$\frac{W}{L}$$

This ratio is fundamental to device behavior and circuit design.

---

### 2.1.3 MOS Symbols

Circuit symbols represent MOSFETs in schematics.

#### NMOS Symbols

**Standard Symbol:**
```
    D (Drain)
    |
    |___
    |   |
G --|   |
    |___|
    |
    S (Source)
```

**With Body Terminal:**
```
    D
    |
    |___
    |   |
G --|   |
    |___|
    |
    S
    |
    B (Body/Bulk)
```

**Arrow Convention:**
- Arrow on source terminal
- Points INTO device for NMOS (p to n)
- Indicates p-type substrate

#### PMOS Symbols

**Standard Symbol:**
```
    S (Source)
    |
    |___
    |   |
G --o   |  (bubble indicates PMOS)
    |___|
    |
    D (Drain)
```

**Arrow Convention:**
- Arrow points OUT of device for PMOS (n to p)
- Indicates n-type substrate
- Bubble on gate indicates complementary device

#### Four-Terminal Device

MOSFETs are actually **four-terminal devices**:
1. Gate (G)
2. Drain (D)
3. Source (S)
4. Bulk/Body (B)

**Body Effect:**

When bulk voltage differs from source voltage:
- Threshold voltage changes
- Body acts as a "second gate"
- Important in circuit analysis

**Common Connections:**
- NMOS: Bulk connected to ground (lowest potential)
- PMOS: Bulk connected to V_DD (highest potential)
- Sometimes explicitly shown, often implicit

#### Symbol Variations

**Enhancement Mode** (most common):
- Normally OFF (no channel without V_GS)
- Requires V_GS > V_TH to conduct
- Standard in digital and analog circuits

**Depletion Mode** (less common):
- Normally ON (channel exists at V_GS = 0)
- Requires negative V_GS to turn off
- Special fabrication process
- Used in specialized circuits

---

## 2.2 MOS I/V Characteristics

The relationship between terminal voltages and currents defines device behavior.

### 2.2.1 Threshold Voltage

The **threshold voltage (V_TH)** is the minimum gate-source voltage required to create a conducting channel.

#### Physical Origin

**Formation of Inversion Layer:**

As V_GS increases:
1. **V_GS < 0**: Accumulation (majority carriers)
2. **0 < V_GS < V_TH**: Depletion (few carriers)
3. **V_GS = V_TH**: **Strong inversion** (conducting channel)
4. **V_GS > V_TH**: Channel conducts current

#### Threshold Voltage Expression

$$V_{TH} = V_{TH0} + \gamma(\sqrt{|2\phi_F + V_{SB}|} - \sqrt{|2\phi_F|})$$

Where:
- $V_{TH0}$ = threshold voltage with V_SB = 0
- γ = body effect coefficient (typically 0.3-0.5 V^(1/2))
- φ_F = Fermi potential (≈ 0.3-0.4V)
- V_SB = source-to-bulk voltage

#### Body Effect

**Definition:** Change in threshold voltage due to source-bulk voltage

**Physical explanation:**
- Non-zero V_SB widens depletion region
- Requires more gate voltage to invert surface
- Threshold voltage increases

**Mathematical form:**

$$\Delta V_{TH} = \gamma(\sqrt{|2\phi_F + V_{SB}|} - \sqrt{|2\phi_F|})$$

**Example:**

For γ = 0.4 V^(1/2), φ_F = 0.35V, V_SB = 2V:

$$\Delta V_{TH} = 0.4(\sqrt{2.7} - \sqrt{0.7}) \approx 0.4(1.64 - 0.84) = 0.32V$$

Significant increase in threshold!

#### Factors Affecting V_TH

**1. Oxide Thickness:**
- Thinner oxide → Lower V_TH
- $V_{TH} \propto t_{ox}$

**2. Doping Concentration:**
- Higher substrate doping → Higher V_TH
- Controls depletion charge

**3. Gate Material:**
- Work function difference
- Affects flat-band voltage

**4. Temperature:**
- V_TH decreases with temperature
- $\frac{dV_{TH}}{dT} \approx -0.5$ to $-2$ mV/°C

**5. Channel Length:**
- Short-channel effects reduce V_TH
- Important in modern processes

#### Threshold Voltage Adjustment

**Ion Implantation:**
- Adjust surface doping
- Fine-tune V_TH
- Multiple V_TH options in advanced processes
- Low-V_TH, regular-V_TH, high-V_TH devices

**Applications:**
- Low-V_TH: High-speed circuits
- High-V_TH: Low-leakage circuits
- Multiple V_TH: Optimize speed/power trade-off

---

### 2.2.2 Derivation of I/V Characteristics

The drain current depends on terminal voltages and device geometry.

#### Assumptions for Basic Derivation

1. Gradual channel approximation (E-field mainly vertical)
2. Carrier mobility constant
3. Long-channel device (no short-channel effects)
4. No velocity saturation
5. Uniform doping

#### Channel Charge Density

At distance y from source, gate-to-channel voltage:

$$V_{GC}(y) = V_{GS} - V(y)$$

Where V(y) is channel potential at position y.

**Charge per unit area:**

$$Q_n(y) = -C_{ox}[V_{GS} - V(y) - V_{TH}]$$

Where:
- $C_{ox} = \frac{\epsilon_{ox}}{t_{ox}}$ (oxide capacitance per unit area)
- Negative sign: electrons are negative charges

#### Current Flow

**Drift current:**

$$I_D = -Q_n(y) \cdot W \cdot v_{drift}$$

Where v_drift is drift velocity:

$$v_{drift} = -\mu_n E_y = -\mu_n \frac{dV}{dy}$$

**Combining:**

$$I_D = \mu_n C_{ox} W [V_{GS} - V(y) - V_{TH}] \frac{dV}{dy}$$

#### Integration Along Channel

Rearranging and integrating from source (y=0, V=0) to drain (y=L, V=V_DS):

$$I_D \int_0^L dy = \mu_n C_{ox} W \int_0^{V_{DS}} [V_{GS} - V - V_{TH}] dV$$

$$I_D \cdot L = \mu_n C_{ox} W \left[(V_{GS} - V_{TH})V_{DS} - \frac{V_{DS}^2}{2}\right]$$

**Drain Current (General):**

$$I_D = \mu_n C_{ox} \frac{W}{L} \left[(V_{GS} - V_{TH})V_{DS} - \frac{V_{DS}^2}{2}\right]$$

#### Operating Regions

**1. Cutoff Region (V_GS < V_TH):**

No channel exists:

$$I_D \approx 0$$

(Actually small subthreshold current)

**2. Triode (Linear) Region (V_GS > V_TH and V_DS < V_GS - V_TH):**

Channel exists from source to drain:

$$I_D = \mu_n C_{ox} \frac{W}{L} \left[(V_{GS} - V_{TH})V_{DS} - \frac{V_{DS}^2}{2}\right]$$

**For small V_DS:**

$$I_D \approx \mu_n C_{ox} \frac{W}{L} (V_{GS} - V_{TH})V_{DS}$$

Device acts like a **voltage-controlled resistor**:

$$R_{DS} = \frac{V_{DS}}{I_D} = \frac{1}{\mu_n C_{ox} \frac{W}{L} (V_{GS} - V_{TH})}$$

**3. Saturation Region (V_GS > V_TH and V_DS ≥ V_GS - V_TH):**

Channel **pinches off** at drain end when:

$$V_{DS} = V_{GS} - V_{TH} \equiv V_{DS,sat}$$

At pinch-off, V(L) = V_GS - V_TH

**Saturation current:**

Substitute V_DS = V_GS - V_TH into triode equation:

$$I_D = \mu_n C_{ox} \frac{W}{L} \left[(V_{GS} - V_{TH})^2 - \frac{(V_{GS} - V_{TH})^2}{2}\right]$$

$$I_D = \frac{1}{2} \mu_n C_{ox} \frac{W}{L} (V_{GS} - V_{TH})^2$$

**Simplified notation:**

Define process transconductance parameter:

$$k_n' = \mu_n C_{ox}$$

And device transconductance parameter:

$$k_n = k_n' \frac{W}{L} = \mu_n C_{ox} \frac{W}{L}$$

**Saturation current becomes:**

$$I_D = \frac{1}{2} k_n (V_{GS} - V_{TH})^2$$

Or introducing overdrive voltage $V_{OV} = V_{GS} - V_{TH}$:

$$I_D = \frac{1}{2} k_n V_{OV}^2 = \frac{1}{2} \mu_n C_{ox} \frac{W}{L} V_{OV}^2$$

#### Physical Interpretation of Saturation

**Why does current saturate?**

1. At V_DS = V_DS,sat, channel depth → 0 at drain
2. **Pinch-off point** forms
3. Further increase in V_DS:
   - Moves pinch-off point slightly toward source
   - But voltage across channel remains ≈ V_DS,sat
   - Current stays approximately constant

**In reality:**
- Current increases slightly with V_DS (channel-length modulation)
- Described by parameter λ (lambda)

#### Complete I-V Equations

**Cutoff (V_GS < V_TH):**

$$I_D = 0$$

**Triode (V_GS > V_TH, V_DS < V_{GS} - V_{TH}):**

$$I_D = \mu_n C_{ox} \frac{W}{L} \left[(V_{GS} - V_{TH})V_{DS} - \frac{V_{DS}^2}{2}\right]$$

**Saturation (V_GS > V_TH, V_DS \geq V_{GS} - V_{TH}):**

$$I_D = \frac{1}{2} \mu_n C_{ox} \frac{W}{L} (V_{GS} - V_{TH})^2$$

#### I-V Characteristic Curves

**Output Characteristics (I_D vs V_DS for various V_GS):**

```
I_D
|     V_GS = V_TH + 4V
|    /‾‾‾‾‾‾‾‾‾‾‾
|   /  V_GS = V_TH + 3V
|  /  /‾‾‾‾‾‾‾‾
| /  /  V_GS = V_TH + 2V
|/  /  /‾‾‾‾‾‾
|  /  /  V_GS = V_TH + 1V
| /  /  /‾‾‾
|/  /  /
|__/__/_____________ V_DS
Triode | Saturation
```

**Transfer Characteristics (I_D vs V_GS for fixed V_DS in saturation):**

```
I_D
|        /
|       /
|      /  Square-law
|     /   relationship
|    /
|   /
|  /
| /
|/__________ V_GS
  V_TH
```

Parabolic (square-law) relationship in saturation.

---

### 2.2.3 MOS Transconductance

**Transconductance (g_m)** measures how effectively gate voltage controls drain current.

#### Definition

$$g_m = \frac{\partial I_D}{\partial V_{GS}} \Big|_{V_{DS}=const}$$

#### Transconductance in Saturation

Starting from:

$$I_D = \frac{1}{2} \mu_n C_{ox} \frac{W}{L} (V_{GS} - V_{TH})^2$$

Taking derivative:

$$g_m = \frac{\partial I_D}{\partial V_{GS}} = \mu_n C_{ox} \frac{W}{L} (V_{GS} - V_{TH})$$

**Alternative forms:**

Using $V_{OV} = V_{GS} - V_{TH}$:

$$g_m = \mu_n C_{ox} \frac{W}{L} V_{OV}$$

Using $I_D = \frac{1}{2} \mu_n C_{ox} \frac{W}{L} V_{OV}^2$:

$$g_m = \sqrt{2 \mu_n C_{ox} \frac{W}{L} I_D}$$

Or:

$$g_m = \frac{2I_D}{V_{OV}}$$

#### Useful Relationships

**From the above forms:**

$$g_m = \sqrt{2 \mu_n C_{ox} \frac{W}{L} I_D} = \frac{2I_D}{V_{GS} - V_{TH}}$$

**Normalized transconductance:**

$$\frac{g_m}{I_D} = \frac{2}{V_{OV}}$$

This shows:
- Higher g_m/I_D for lower overdrive voltage
- Trade-off: Lower V_OV → Higher g_m/I_D but larger device

#### Transconductance in Triode Region

In triode:

$$I_D = \mu_n C_{ox} \frac{W}{L} \left[(V_{GS} - V_{TH})V_{DS} - \frac{V_{DS}^2}{2}\right]$$

$$g_m = \mu_n C_{ox} \frac{W}{L} V_{DS}$$

Depends on V_DS, typically smaller than in saturation.

#### Design Implications

**For high gain:**
- Need high g_m
- Options:
  1. Increase W/L (larger area)
  2. Increase I_D (more power)
  3. Improve mobility (process-dependent)
  4. Reduce V_OV (less headroom)

**Trade-offs:**
- g_m vs. power: $g_m \propto \sqrt{I_D}$
- g_m vs. area: $g_m \propto \sqrt{W/L}$
- g_m vs. voltage headroom: $g_m \propto 1/V_{OV}$

---

## 2.3 Second-Order Effects

Real MOSFETs deviate from the ideal square-law model due to several physical effects.

### 1. Channel-Length Modulation

**Physical Origin:**

In saturation, pinch-off point moves toward source as V_DS increases:
- Effective channel length decreases: $L_{eff} = L - \Delta L$
- Current increases

**Modified Saturation Current:**

$$I_D = \frac{1}{2} \mu_n C_{ox} \frac{W}{L} (V_{GS} - V_{TH})^2 (1 + \lambda V_{DS})$$

Where λ is the **channel-length modulation parameter**:

$$\lambda \approx \frac{1}{V_A L}$$

V_A is the Early voltage (analogous to BJTs).

**Output Resistance:**

$$r_o = \frac{\partial V_{DS}}{\partial I_D} = \frac{1}{\lambda I_D} \approx \frac{V_A}{I_D}$$

**Typical values:**
- λ: 0.01 to 0.1 V^(-1) (shorter channels have larger λ)
- r_o: 10kΩ to 1MΩ depending on I_D and L

**Design implications:**
- Longer channel → Higher r_o → Higher gain
- Finite r_o limits intrinsic gain: $A_0 = g_m r_o$

---

### 2. Body Effect

**Already covered in threshold voltage section.**

**Small-signal model:**

Body-to-source voltage affects drain current through V_TH.

**Body transconductance:**

$$g_{mb} = \frac{\partial I_D}{\partial V_{BS}} = \frac{\partial I_D}{\partial V_{GS}} \cdot \frac{\partial V_{GS}}{\partial V_{TH}} \cdot \frac{\partial V_{TH}}{\partial V_{BS}}$$

Since $\frac{\partial I_D}{\partial V_{GS}} = g_m$ and $\frac{\partial V_{GS}}{\partial V_{TH}} = -1$:

$$g_{mb} = -g_m \frac{\partial V_{TH}}{\partial V_{BS}}$$

From body effect:

$$\frac{\partial V_{TH}}{\partial V_{BS}} = \frac{\gamma}{2\sqrt{2\phi_F + V_{SB}}} \equiv \eta$$

**Therefore:**

$$g_{mb} = \eta g_m$$

Where η (eta) typically ranges from 0.1 to 0.3.

**Common notation:**

$$\chi = \frac{g_{mb}}{g_m} = \eta$$

**Impact:**
- Source followers have gain reduced by body effect
- Common-gate input impedance affected
- Important when bulk not connected to source

---

### 3. Subthreshold Conduction

**Region:** V_GS < V_TH

**Ideal model:** I_D = 0

**Reality:** Exponential current flow (like BJT)

$$I_D = I_0 e^{\frac{V_{GS} - V_{TH}}{n V_T}} (1 - e^{-V_{DS}/V_T})$$

Where:
- V_T = kT/q ≈ 26mV at room temperature
- n = subthreshold slope factor (1.3 to 1.7)
- I_0 = process-dependent constant

**For V_DS > 3V_T:**

$$I_D \approx I_0 e^{\frac{V_{GS} - V_{TH}}{n V_T}}$$

**Subthreshold slope:**

$$S = n V_T \ln(10) \approx 60n \text{ mV/decade}$$

Typically 80-100 mV/decade.

**Applications:**
- Ultra-low power circuits
- Weak inversion operation
- Exponential current sources

**Design considerations:**
- Leakage current in digital circuits (off-state)
- Usable region for analog (higher g_m/I_D)
- Trade-off: Lower power but slower

---

### 4. Mobility Degradation

**Vertical Electric Field Effect:**

High gate voltage → Strong vertical E-field → Carriers scatter at Si-SiO₂ interface

**Effective mobility:**

$$\mu_{eff} = \frac{\mu_0}{1 + \theta(V_{GS} - V_{TH})}$$

Where θ is mobility degradation coefficient.

**Modified current:**

$$I_D = \frac{1}{2} \frac{\mu_n C_{ox}}{1 + \theta(V_{GS} - V_{TH})} \frac{W}{L} (V_{GS} - V_{TH})^2$$

**Effect:**
- Current saturates less than square-law predicts
- Transconductance peaks then decreases
- More pronounced in modern (thin-oxide) devices

**Transconductance with mobility degradation:**

$$g_m = \frac{\mu_n C_{ox} \frac{W}{L} (V_{GS} - V_{TH})}{[1 + \theta(V_{GS} - V_{TH})]^2}$$

Peaks at $V_{GS} - V_{TH} = 1/\theta$, then decreases.

---

### 5. Velocity Saturation

**Physical Limit:**

At high electric fields, carrier velocity saturates at v_sat ≈ 10^7 cm/s for silicon.

**Critical Field:**

$$E_{crit} = \frac{v_{sat}}{\mu_n}$$

For short channels, E_field can exceed E_crit.

**Modified Saturation Current:**

When velocity saturation dominates:

$$I_D = W C_{ox} v_{sat} (V_{GS} - V_{TH})$$

**Linear** dependence on (V_GS - V_TH) instead of square-law!

**Transconductance:**

$$g_m = W C_{ox} v_{sat}$$

Independent of bias! Depends only on W.

**Implications:**
- Short-channel devices: more linear I-V
- Reduced g_m compared to long-channel prediction
- Benefits: More constant g_m, lower distortion

---

### 6. Short-Channel Effects

**Definition:** Effects that become significant when L approaches depletion region width.

**Types:**

**a) V_TH Roll-Off:**
- Threshold voltage decreases with shorter L
- Charge sharing between source/drain and gate
- DIBL (Drain-Induced Barrier Lowering)

**b) Punch-Through:**
- Drain depletion region reaches source
- Loss of gate control
- Minimum L for given V_DS

**c) Hot-Carrier Effects:**
- High-energy carriers damage oxide
- Reliability concern
- Degrades device over time

**Design considerations:**
- Use minimum L+1 or L+2 for analog (not absolute minimum)
- Cascode to reduce V_DS
- Avoid high V_DS on minimum-length devices

---

### 7. Temperature Effects

**Mobility:**

$$\mu(T) \propto T^{-1.5}$$ to $$T^{-2}$$

Decreases with temperature.

**Threshold Voltage:**

$$\frac{dV_{TH}}{dT} \approx -0.5 \text{ to } -2 \text{ mV/°C}$$

Decreases with temperature.

**Net Effect on I_D:**

Two competing effects:
1. Lower μ → Lower I_D
2. Lower V_TH → Higher I_D (for given V_GS)

**Result depends on bias:**
- **Low V_GS - V_TH**: V_TH effect dominates → Positive TC
- **High V_GS - V_TH**: μ effect dominates → Negative TC
- **Zero-TC bias point** exists: $V_{GS} - V_{TH} \approx 0.5$V to $0.7$V

**Design implications:**
- Temperature compensation circuits
- Bias at zero-TC point for stability
- PTAT and CTAT current sources

---

## 2.4 MOS Device Models

### 2.4.1 MOS Device Layout

Physical layout determines parasitic capacitances and resistances.

#### Basic Layout Structure

**Top View:**

```
     W (Width)
   <--------->
   ___________  ___
  |  Source   |  ^
  |___________|  |
  |           |  |
  | Channel   |  | L (Length)
  |           |  |
  |___________|  |
  |   Drain   |  v
  |___________|  ‾‾‾
      Gate
   (Poly over)
```

**Layers:**
1. **Active area** (diffusion): Source, drain, channel
2. **Polysilicon**: Gate electrode
3. **Metal layers**: Contacts and routing
4. **Contacts/vias**: Connect layers

#### Layout Parasitics

**Source/Drain Capacitances:**
- **Junction capacitance**: C_j (bottom plate)
- **Sidewall capacitance**: C_jsw (perimeter)
- Voltage-dependent (non-linear)

**Gate Capacitances:**
- **Gate-source overlap**: C_GSO
- **Gate-drain overlap**: C_GDO
- **Gate-channel**: Operating-point dependent

**Resistances:**
- **Source/drain resistance**: R_S, R_D
- **Gate resistance**: R_G (for wide devices)
- **Substrate resistance**: R_sub

#### Multi-Finger Layout

For large W devices, use multiple parallel fingers:

```
S - G - D - G - S - G - D
|   |   |   |   |   |   |
  M fingers
```

**Advantages:**
- Reduced source/drain resistance
- Better matching
- Reduced gate resistance
- More compact layout

**Effective dimensions:**
- Total W = M × W_finger
- Same L for all fingers

---

### 2.4.2 MOS Device Capacitances

Capacitances determine AC behavior and frequency response.

#### Gate Capacitances (Operating Point Dependent)

**1. Cutoff Region (V_GS < V_TH):**

No channel:
- C_GS ≈ C_GSO (overlap only)
- C_GD ≈ C_GDO (overlap only)
- C_GB ≈ W·L·C_ox (oxide capacitance to bulk)

**2. Triode Region:**

Channel exists from source to drain:
- C_GS ≈ (1/2)W·L·C_ox + C_GSO
- C_GD ≈ (1/2)W·L·C_ox + C_GDO
- C_GB ≈ 0 (channel shields bulk)

**3. Saturation Region:**

Channel pinched off at drain:
- C_GS ≈ (2/3)W·L·C_ox + C_GSO
- C_GD ≈ C_GDO (only overlap)
- C_GB ≈ 0

**Overlap Capacitances (always present):**

$$C_{GSO} = C_{GDO} = W \cdot L_{ov} \cdot C_{ox}$$

Where L_ov is the overlap length (typically 0.1-0.3 × L).

#### Junction Capacitances

**Source and Drain to Bulk:**

$$C_{SB} = C_{j,bottom} + C_{j,sidewall}$$

**Bottom-plate (area) capacitance:**

$$C_{j,bottom} = C_{j0} \frac{A_S}{\left(1 + \frac{V_{SB}}{\phi_B}\right)^{m_j}}$$

Where:
- C_j0 = zero-bias capacitance per unit area
- A_S = source area
- φ_B = built-in potential (≈0.6-0.9V)
- m_j = grading coefficient (0.3-0.5)

**Sidewall (perimeter) capacitance:**

$$C_{j,sidewall} = C_{jsw0} \frac{P_S}{\left(1 + \frac{V_{SB}}{\phi_B}\right)^{m_{jsw}}}$$

Where:
- C_jsw0 = zero-bias sidewall capacitance per unit length
- P_S = source perimeter

**Non-linear and voltage-dependent!**

#### Total Device Capacitance

**Gate-related:**
- C_GS: Gate to source
- C_GD: Gate to drain  
- C_GB: Gate to bulk

**Junction-related:**
- C_SB: Source to bulk
- C_DB: Drain to bulk

**Important for:**
- Frequency response
- Settling time
- Noise analysis
- Power consumption (CV²f)

#### Gate Capacitance Simplification

**For hand analysis in saturation:**

$$C_{GS} \approx \frac{2}{3} W L C_{ox}$$
$$C_{GD} \approx 0$$
$$C_{GB} \approx 0$$

**Total gate capacitance:**

$$C_G = C_{GS} + C_{GD} + C_{GB} \approx \frac{2}{3} W L C_{ox}$$

**For digital switching (full swing):**

Average gate capacitance:

$$C_G \approx W L C_{ox}$$

---

### 2.4.3 MOS Small-Signal Model

For AC analysis, linearize around DC operating point.

#### Basic Small-Signal Model (Saturation)

```
         G o----||----o D
               C_GD   |
         |            |
        C_GS       g_m·v_gs || r_o
         |            |
         S o----------o S
```

**Components:**

1. **Transconductance (g_m):**
   - Controlled current source
   - $i_d = g_m v_{gs}$
   - $g_m = \sqrt{2\mu_n C_{ox} \frac{W}{L} I_D}$

2. **Output Resistance (r_o):**
   - Channel-length modulation
   - $r_o = \frac{1}{\lambda I_D}$

3. **Gate-Source Capacitance:**
   - $C_{GS} = \frac{2}{3} W L C_{ox} + C_{GSO}$

4. **Gate-Drain Capacitance:**
   - $C_{GD} = C_{GDO}$ (overlap only in saturation)

#### Complete Small-Signal Model

Including body effect and all capacitances:

```
     G o----C_GD----o D
        |           |
       C_GS    g_m·v_gs || r_o || C_DB
        |           |
        +           |
      v_gs     g_mb·v_bs
        -           |
        |           |
     S  o-----------o S
        |
       C_SB
        |
     B  o
```

**Additional components:**

5. **Body Transconductance (g_mb):**
   - $i_d = g_{mb} v_{bs}$
   - $g_{mb} = \eta g_m$ where η ≈ 0.1-0.3

6. **Source-Bulk Capacitance:**
   - $C_{SB}$ (junction capacitance)

7. **Drain-Bulk Capacitance:**
   - $C_{DB}$ (junction capacitance)

#### Small-Signal Parameters Summary

| Parameter | Expression | Typical Value |
|-----------|-----------|---------------|
| g_m | $\sqrt{2\mu_n C_{ox} \frac{W}{L} I_D}$ | 0.1-10 mS |
| r_o | $\frac{1}{\lambda I_D}$ | 10k-1MΩ |
| g_mb | $\eta g_m$ | 0.1g_m - 0.3g_m |
| C_GS | $(2/3) W L C_{ox}$ | 1-100 fF |
| C_GD | $C_{GDO}$ | 1-10 fF |
| C_DB, C_SB | Junction cap | 1-50 fF |

#### Intrinsic Gain

**Definition:** Voltage gain with no load

$$A_0 = g_m r_o = g_m \cdot \frac{1}{\lambda I_D}$$

Using $g_m = 2I_D/V_{OV}$:

$$A_0 = \frac{2}{\lambda V_{OV}}$$

**Typical values:**
- Long-channel (L = 1μm): A_0 = 50-200
- Short-channel (L = 65nm): A_0 = 5-20

**Key insight:**
- Intrinsic gain decreases with shorter channels
- Trade-off: Speed vs. gain
- Multi-stage amplifiers needed for high gain

#### Frequency Response

**Unity-gain frequency (f_T):**

Frequency where current gain = 1.

$$f_T = \frac{g_m}{2\pi C_{GS}} \approx \frac{g_m}{2\pi \cdot \frac{2}{3} W L C_{ox}}$$

Simplifying:

$$f_T \approx \frac{3}{2} \frac{\mu_n V_{OV}}{2\pi L^2}$$

**Key observations:**
- f_T ∝ 1/L² (shorter = faster)
- f_T ∝ V_OV (higher overdrive = faster)
- f_T independent of W (first order)

**Typical values:**
- 0.18μm CMOS: f_T = 50-80 GHz
- 28nm CMOS: f_T = 200-300 GHz

---

### 2.4.4 MOS SPICE Models

SPICE uses sophisticated models for accurate simulation.

#### Model Levels

**Level 1:** 
- Simple square-law model
- Good for hand calculations
- Not accurate for modern devices

**Level 2-3:**
- More accurate
- Include more second-order effects
- Rarely used today

**BSIM Models (Level 49+):**
- Industry standard
- BSIM3, BSIM4, BSIM6
- Hundreds of parameters
- Accurate for modern processes
- Sub-100nm devices

#### Key SPICE Parameters

**Basic:**
- VTO (V_TH0): Threshold voltage
- KP (k'): Process transconductance parameter
- LAMBDA (λ): Channel-length modulation
- GAMMA (γ): Body effect coefficient

**Capacitance:**
- CGSO, CGDO: Overlap capacitances
- CJ, CJSW: Junction capacitances
- TOX: Oxide thickness

**Advanced:**
- U0: Low-field mobility
- VSAT: Saturation velocity
- NFACTOR: Subthreshold slope
- Hundreds more...

#### Using SPICE Models

**Model statement:**

```spice
.MODEL NMOS_MODEL NMOS (LEVEL=49 VTO=0.4 KP=200u LAMBDA=0.05 ...)
```

**Device instantiation:**

```spice
M1 drain gate source bulk NMOS_MODEL W=10u L=0.18u
```

**Best practices:**
- Use foundry-provided models
- Verify across corners (TT, FF, SS, FS, SF)
- Check temperature  variations
- Monte Carlo for mismatch

---

### 2.4.5 NMOS Versus PMOS Devices

Key differences between n-channel and p-channel MOSFETs.

#### Physical Differences

| Property | NMOS | PMOS |
|----------|------|------|
| Substrate | p-type | n-type |
| Source/Drain | n+ | p+ |
| Carriers | Electrons | Holes |
| Channel | n-type (inversion) | p-type (inversion) |

#### Electrical Differences

**Mobility:**
- μ_n ≈ 2-3 × μ_p
- Electrons more mobile than holes
- NMOS faster for same geometry

**Threshold Voltage:**
- NMOS: V_TH,n ≈ +0.3 to +0.7V
- PMOS: V_TH,p ≈ -0.3 to -0.7V (negative)

**Current:**

For same W/L and |V_GS - V_TH|:
- I_D,NMOS ≈ 2-3 × I_D,PMOS

**To match currents:**
- (W/L)_PMOS ≈ 2-3 × (W/L)_NMOS

#### Design Implications

**NMOS advantages:**
- Higher g_m for same size
- Smaller area for same current
- Preferred for high-speed circuits

**PMOS advantages:**
- Lower 1/f noise (typically)
- Can be used as pull-up
- Complementary to NMOS

**CMOS Circuits:**
- Use both NMOS and PMOS
- Complementary operation
- Full rail-to-rail swing
- Zero static power (in digital)

---

### 2.4.6 Long-Channel Versus Short-Channel Devices

Trade-offs between device length.

#### Long-Channel Devices (L >> minimum)

**Advantages:**
- Higher output resistance (r_o)
- Higher intrinsic gain (A_0 = g_m r_o)
- Better matching
- More predictable behavior
- Lower 1/f noise

**Disadvantages:**
- Lower f_T (speed)
- Larger capacitances
- More area
- Lower g_m for same current

**Applications:**
- Current mirrors
- Bias circuits
- High-gain stages
- Precision circuits

#### Short-Channel Devices (L ≈ minimum)

**Advantages:**
- Higher f_T (faster)
- Lower capacitances
- Higher g_m for same I_D
- Smaller area

**Disadvantages:**
- Lower r_o (worse gain)
- More second-order effects
- Worse matching
- Process variations

**Applications:**
- High-speed circuits
- RF circuits
- Digital logic
- When area is critical

#### Design Guidelines

**For analog circuits:**
- Input devices: Long L (matching, noise)
- Load devices: Long L (high r_o)
- Cascodes: Can use shorter L
- High-speed paths: Short L

**Typical choices:**
- L_min = 65nm process: Use L = 100-200nm for analog
- L_min = 0.18μm: Use L = 0.36-1μm for analog
- Current mirrors: 2-4 × L_min
- Precision matching: 5-10 × L_min

---

## 2.5 Appendix A: FinFETs

Advanced 3D transistor structure for sub-22nm nodes.

### Structure

**Planar MOSFET limitations:**
- Short-channel effects severe below 22nm
- Poor gate control
- High leakage

**FinFET Solution:**
- 3D structure with vertical fin
- Gate wraps around channel (3 sides)
- Better electrostatic control

### Key Features

**Advantages:**
- Excellent short-channel control
- Lower leakage
- Higher drive current per area
- Better subthreshold slope

**Challenges:**
- Limited W sizing (quantized by fin width)
- More complex layout
- Different parasitics

**Not covered in detail** (beyond basic analog course).

---

## 2.6 Appendix B: Behavior of a MOS Device as a Capacitor

MOSFETs can be used as capacitors in analog circuits.

### Gate-Channel Capacitor

**Configuration:**
- Gate as one terminal
- Source, drain, bulk shorted as other terminal

**Capacitance value:**

$$C = W \cdot L \cdot C_{ox}$$

**Applications:**
- Switched-capacitor circuits
- Compensation capacitors
- Coupling capacitors
- Trimming capacitors

### Advantages

1. **Precision**: Determined by lithography
2. **Matching**: Excellent capacitor ratios (0.1%)
3. **Density**: Higher than metal-metal capacitors
4. **Availability**: No special process needed

### Disadvantages

1. **Voltage dependence**: Non-linear
2. **Parasitics**: Bottom-plate capacitance to substrate
3. **Matching with bias**: Need careful biasing

### Design Techniques

**Bottom-plate node:**
- Connect source/drain/bulk (less parasitic-sensitive)
- Use as sampling node in SC circuits

**Top-plate node:**
- Gate terminal
- Connect to virtual ground (reduces parasitics)

**Linearization:**
- Bias at constant voltage
- Use in differential configuration
- Special layout techniques

---

## Summary

### Key Concepts

**1. MOSFET Basics:**
- Voltage-controlled current source
- Four-terminal device (G, D, S, B)
- Forms channel when V_GS > V_TH

**2. I-V Characteristics:**
- **Cutoff**: V_GS < V_TH, I_D ≈ 0
- **Triode**: V_DS < V_GS - V_TH, resistive
- **Saturation**: V_DS ≥ V_GS - V_TH, current source

**3. Square-Law Model:**
- $I_D = \frac{1}{2}\mu_n C_{ox} \frac{W}{L}(V_{GS}-V_{TH})^2$ (saturation)
- $g_m = \sqrt{2\mu_n C_{ox} \frac{W}{L} I_D}$

**4. Second-Order Effects:**
- Channel-length modulation (r_o)
- Body effect (g_mb)
- Velocity saturation
- Mobility degradation
- Short-channel effects

**5. Small-Signal Model:**
- Transconductance: g_m
- Output resistance: r_o
- Body transconductance: g_mb
- Capacitances: C_GS, C_GD, C_DB, C_SB

**6. Key Trade-offs:**
- Speed vs. gain (L)
- Power vs. performance (I_D, W/L)
- Area vs. matching (W, L)
- Headroom vs. g_m (V_OV)

### Design Parameters

**Controllable:**
- W, L (geometry)
- I_D (bias current)
- V_GS (or V_OV = V_GS - V_TH)

**Process-determined:**
- μ_n, C_ox, V_TH
- λ, γ
- Capacitances per unit area

**Performance metrics:**
- g_m: Gain, speed
- r_o: Voltage gain
- f_T: Frequency response
- Matching: Precision

This foundation is essential for understanding more complex analog circuits in subsequent chapters.

## 2.6 Common Questions: Device Geometry

**Question:** Why is the aspect ratio $W/L$ considered a 2D parameter, not 3D?

**Answer:**
In planar MOSFET technology (the focus of this text), the transistor is treated as a surface device defined by the layout mask.

1.  **Designer Control (2D):** The designer draws the **Width ($W$)** and **Length ($L$)** on a 2D layout. These are the only geometric parameters available for tuning.
2.  **Process Control (3rd Dimension):** The vertical dimensions, such as **Oxide Thickness ($t_{ox}$)** and **Junction Depth**, are fixed by the manufacturing process. The oxide thickness is captured in the parameter $C_{ox} = \epsilon_{ox} / t_{ox}$.
3.  **Current Equation:** The drain current equation $I_D = \frac{1}{2} \mu C_{ox} \frac{W}{L} (V_{GS} - V_{TH})^2$ separates the process parameters ($C_{ox}$) from the design parameters ($W/L$).

*Note: In modern FinFET technologies, the "width" effectively wraps around the 3D fin structure, but for fundamental analog design, we assume planar devices.*
