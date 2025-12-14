**Chapter 9 – Operational Amplifiers**

Chapter 9 of the sources provides a detailed explanation of Operational Amplifiers (Op Amps), focusing on design considerations, fundamental topologies, performance parameters, and mitigation techniques essential for Analog CMOS Integrated Circuit (IC) design.

## 9.1 General Considerations

An operational amplifier is fundamentally defined as a **high-gain differential amplifier**. Historically, the goal was to create an ideal op amp (very high gain, high input impedance, low output impedance), but modern design focuses on **multidimensional optimization** to achieve adequate performance for specific applications, balancing trade-offs across metrics like gain, speed, power, and noise.

### 9.1.1 Performance Parameters

1.  **Gain ($A_v$)**: The open-loop gain determines the **precision of the feedback system** that utilizes the op amp. A high gain is necessary to minimize closed-loop gain error and suppress nonlinearity.
2.  **Small-Signal Bandwidth ($f_u$ or GBW)**: Determines the high-frequency limits of operation. It is often characterized by the **unity-gain frequency ($f_u$)**.
3.  **Large-Signal Behavior (Slew Rate, SR)**: Determines the speed during large transient signal changes, characterized by a **constant slope (slew rate)**. Unlike linear circuits where the slope scales with input amplitude, SR is often limited by the constant current available to charge the dominant capacitance.
4.  **Output Swing**: The available voltage excursion at the output, which may be specified differentially. Output swing often limits the dynamic range and trades off critically with speed and device size, especially under low supply voltages.
5.  **Linearity**: Op amps must process signals without introducing excessive distortion. Linearity is often governed by the **maximum voltage swing** allowed before the open-loop gain drops significantly.
6.  **Noise and Offset**: These parameters limit the minimum detectable signal. Noise performance typically trades with power consumption and device size.
7.  **Power Supply Rejection Ratio (PSRR)**: Measures the op amp’s ability to suppress the effect of supply voltage noise at the output, typically necessitating differential topologies.
8.  **Power Consumption**: A primary constraint in modern design, requiring careful partitioning of current to meet other performance specifications.

---

## 9.2 One-Stage Op Amps

One-stage op amps perform voltage-to-current conversion only once, limiting the gain to the product of the input pair transconductance and the output impedance.

### 9.2.1 Basic Topologies

Simple differential pairs loaded by active current mirrors or resistive loads achieve gains generally limited to **less than 10 in nanometer technologies**. The gain ($A_v$) is approximated as $-g_{mN}(r_{oN} \| r_{oP})$.

#### Telescopic Cascode Op Amps

*   **Topology and Purpose**: Achieves **high voltage gain** by maximizing the output impedance using cascode devices, such as the differential cascode amplifier.
*   **Drawbacks**: Telescopic cascodes suffer from **limited output swings** due to stacking multiple overdrive voltages ($V_{OD}$) and threshold voltages ($V_{TH}$). The minimum output voltage is limited to approximately four overdrives ($4V_{OD}$) above ground. It is also difficult to achieve equal input and output Common-Mode (CM) levels, restricting its use in unity-gain configurations.
*   **Gain and Impedance**: The gain is proportional to $g_{m1}[(g_{m3}r_{o3}r_{o1}) \| (g_{m5}r_{o5}r_{o7})]$, approximately the **square of the intrinsic gain** ($g_m r_o$).

### 9.2.2 Design Procedure

The design of a telescopic cascode op amp typically follows a systematic approach to satisfy specifications for gain, bandwidth, swing, and power:

1.  **Select Channel Lengths ($L$)**: Choose $L$ for all devices. Short channels ($L_{min}$) are typically used for high speed, while longer channels ($2L_{min}$ to $5L_{min}$) are used to increase output resistance ($r_o$) and intrinsic gain.
2.  **Determine Bias Current ($I_{SS}$)**: Calculate the required tail current based on the **Slew Rate (SR)** requirement ($I_{SS} = SR \cdot C_L$) or the total power budget.
3.  **Allocate Overdrive Voltages**:
    *   Select the overdrive voltage of the input pair, $(V_{GS}-V_{TH})_{1,2}$, to set the transconductance $g_{m1,2}$. A lower overdrive increases $g_m$ (and gain) but reduces linearity and speed.
    *   Select the overdrives of the cascode and load transistors to meet the **Output Swing** specification. The swing is limited by $V_{DD} - \sum V_{OD}$.
4.  **Calculate Transistor Widths ($W$)**: Using the chosen currents and overdrive voltages, determine the widths using the square-law equation: $W = \frac{2I_D}{\mu C_{ox} (V_{GS}-V_{TH})^2} L$.
5.  **Verify Gain and Bandwidth**: Calculate the DC gain ($A_v \approx g_{m1} R_{out}$) and unity-gain bandwidth ($f_u \approx g_{m1}/C_L$). If the gain is insufficient, increase $L$ or reduce the overdrive voltages (increasing $W$).

### 9.2.3 Linear Scaling

This principle relates to design for power optimization. Scaling all transistor **widths** and **bias currents ($I_D$)** by a factor of $K$ (or $1/K$) while keeping the channel **lengths** constant:
*   The **voltage gain, output swing, and overdrive voltage** ($V_{GS}-V_{TH}$) remain constant.
*   **Transconductance ($g_m$)** scales linearly by $K$.
*   **Output resistance ($r_o$)** scales by $1/K$ (inversely).
*   **Power consumption and total input capacitance** scale linearly by $K$.

This principle confirms that design iterations can initially ignore power budgets, and then linearly scale the final dimensions and currents to meet power targets. Scaling down reduces power but **increases input-referred noise** (since $V_{n,in}^2 \propto 1/g_m$) and **reduces speed** by lowering $g_m$ for a fixed load capacitance.

### 9.2.4 Folded-Cascode Op Amps

*   **Topology and Purpose**: Solves the severe swing limitation of telescopic cascodes by replacing the input device with the opposite type (e.g., PMOS input pair) and using **current mirrors (folding)** to connect to the cascode devices.
*   **Advantages**: Allows the **input and output CM levels to be chosen equally** without limiting differential swings, and generally accommodates a wider input CM range.
*   **Drawbacks**: The voltage gain is typically **two to three times lower** than a comparable telescopic cascode due to parallel output resistances and usually requires **more power** because the bias current for the input stage is not reused by the cascode stage. The **mirror pole** at the folding node can also limit stability.

### 9.2.5 Folded-Cascode Properties

The Folded-Cascode Operational Amplifier (Op Amp) topology is developed primarily to address the shortcomings of the telescopic cascode structure, particularly concerning output swing limitations and the flexibility of setting common-mode (CM) levels.

**Comparison with Telescopic Cascode:**

1.  **Output Swing and Headroom:** While the telescopic cascode suffers from severe output swing limitations due to the stacking of multiple overdrive voltages ($V_{OD}$) and threshold voltages ($V_{TH}$), the folded-cascode design is superior in handling voltage margins. A telescopic cascode may require up to $4V_{OD}$ plus $V_{TH}$ of headroom, severely restricting swings, especially under low supply voltages. The folded cascode alleviates this stacking constraint, offering a somewhat higher overall output swing than its telescopic counterpart.
2.  **Gain and Power:** The voltage gain achieved by a folded cascode is typically **two to three times lower** than that of a comparable telescopic cascode. This is primarily because the folding structure often leads to lower output impedance due to the parallel combination of output resistances ($r_o$) and requires **more power** than the telescopic structure. The input pair in a folded cascode requires its own bias current, which is not reused by the cascode stage, unlike in a telescopic topology.
3.  **Input/Output Common-Mode Levels:** A critical advantage of the folded cascode is its ability to allow the **input and output CM levels to be chosen equally** without limiting differential swings. This contrasts sharply with the limitations of the telescopic cascode, which restricts its use in unity-gain configurations. The folded cascode also generally **accommodates a wider input CM range**.
4.  **Noise and Stability:** Folded cascodes potentially suffer from greater **input-referred noise** than telescopic counterparts. Furthermore, the complexity of the topology introduces more capacitance at the "folding nodes" (where the input differential pair connects to the cascode devices). This means the **first non-dominant pole** (or "mirror pole") in a folded cascode is often closer to the origin than in a telescopic topology, potentially degrading the phase margin and limiting speed.

**Design Suitability:**

Folded-cascode Op Amps are widely utilized despite their generally lower gain, mainly because they offer:

*   **Greater flexibility** in setting input and output CM levels, making them excellent choices for circuits requiring equal input/output CM levels, such as unity-gain configurations.
*   A design that allows a **wider input CM range** (e.g., accommodating input CM levels near ground potential if using a PMOS input pair).
*   A structure that, when combined with gain boosting, can achieve high performance while minimizing the constraints on voltage headroom imposed by low supply voltages.

### 9.2.6 Design Procedure

The design procedure for folded-cascode Op Amps, as placed within the context of Chapter 9 of the sources, serves the purpose of reinforcing core analog circuit design concepts established previously. While specific step-by-step guidance is not explicitly detailed in the provided excerpts, the comprehensive approach required for this task relies on applying fundamental principles:

1.  **Meeting Performance Specifications (Gain, Swing, Speed):** The designer must select transistor dimensions ($W/L$) and bias currents to achieve the required voltage gain, typically by **maximizing the output impedance** ($R_{out}$) using cascode devices and aiming for a high output swing. Given the inherent gain limitation of the folded cascode, designers often rely on using **longer channel lengths** for cascode devices or active load transistors to boost $r_o$ and, consequently, the overall gain.
2.  **Setting Voltage Headroom ($V_{DS,min}$):** A crucial step is allocating the **overdrive voltages ($V_{OD}$) of the transistors** to ensure that they remain in the saturation region while accommodating the specified differential output swing. The folded cascode allows a wider range for the output CM level compared to the telescopic cascode.
3.  **Biasing and Current Partitioning:** The designer must determine the total bias current ($I_{D}$) based on the power budget and allocate currents to the input differential pair and the folding/cascode branches. Since the input current is not reused, the total required power consumption is typically higher than for a telescopic design to maintain comparable $g_m$ and speed.
4.  **Application of Linear Scaling:** Once the target performance (gain, speed, swing) has been met with an arbitrary power consumption, the principle of **linear scaling** can be applied. The transistor widths ($W$) and bias currents ($I_{D}$) can be scaled linearly (while keeping $L$ constant) to meet the specified power budget, without altering voltage gain, output swing, or overdrive voltages ($V_{GS}-V_{TH}$). Scaling down reduces power but inversely affects noise performance and speed.
5.  **Common-Mode Feedback (CMFB) Provision:** Unlike some single-ended circuits, the high-gain fully differential nature of the folded cascode **requires a CMFB circuit** to set and regulate the output CM level, preventing drift towards the supply rails due to device mismatches.

---

## 9.3 Two-Stage Op Amps

Two-stage op amps are defined by cascading multiple gain elements, distinguishing their structure from one-stage amplifiers (like telescopic or folded cascodes) where voltage-to-current conversion occurs only once.

### Structure and Necessity

The primary motivation for using a two-stage architecture is that it allows the separation of critical performance requirements across the cascade:

1.  **Stage 1 (Input/Gain Stage):** This stage is typically designed to provide high voltage gain. It often employs a cascode configuration to achieve the required gain necessary to ensure precision when the op amp is ultimately used within a feedback loop.
2.  **Stage 2 (Output/Buffer Stage):** This stage is primarily dedicated to providing **maximum output voltage swing** (headroom) and adequate drive capability for external loads. The second stage is commonly implemented as a simple Common-Source (CS) stage to exploit its inherent rail-to-rail swing capability.

The overall voltage gain ($A_v$) of the op amp is the product of the gains of the two individual stages ($A_{v1} \cdot A_{v2}$).

### Stability Considerations

A major challenge introduced by two-stage op amps is **stability**. Because the cascading of stages inherently introduces multiple poles (at the output of the first stage and the output of the second stage), these amplifiers are **more susceptible to instability** when integrated into a feedback loop compared to one-stage topologies. This issue necessitates careful frequency response analysis and often requires dedicated compensation techniques, such as Miller compensation, to ensure robust operation.

## 9.3.1 Design Procedure

Designing a two-stage operational amplifier requires an iterative and structured approach, typically involving the partitioning of constraints (such as power, swing, and gain) between the two distinct amplifier stages. Although specific step-by-step guidance is not explicitly detailed in the provided excerpts for Section 9.3.1, the general procedure requires synthesizing the knowledge of transistor modeling, power constraints, and gain requirements, exemplified by the detailed analysis of the components:

1.  **Power and Current Allocation:** The total power budget ($P_{total}$) must first be partitioned between the two stages. For instance, the total supply current ($I_{total}$) might be split equally between Stage 1 and Stage 2, or partitioned based on speed/noise requirements for the input devices versus drive requirements for the output stage. Once the current ($I_D$) for each branch is known, it serves as a fixed constraint for subsequent transistor sizing in that stage.

2.  **Output Stage (Stage 2) Sizing (Swing Priority):**
    *   The primary constraint here is achieving the maximum possible **output voltage swing** (Vout).
    *   The designer must carefully choose the overdrive voltages ($V_{OD}$) of the transistors in the output stage to ensure they remain in saturation across the full required output excursion.
    *   $V_{OD}$ allocation involves trade-offs: lowering $V_{OD}$ maximizes swing but decreases $g_m$ (for fixed $I_D$), impacting the stage's speed and noise contribution.
    *   Once $I_D$ and $V_{OD}$ are selected, the necessary **aspect ratios ($W/L$)** and resulting $g_m$ are calculated, ensuring that the second stage achieves sufficient gain ($A_{v2}$) to meet the overall target gain.

3.  **Input Stage (Stage 1) Sizing (Gain/Noise Priority):**
    *   The required **voltage gain of the first stage ($A_{v1}$) is determined by the total gain requirement ($A_v = A_{v1} \cdot A_{v2}$).
    *   $V_{OD}$ for the input devices ($M_1, M_2$) is often chosen to be small to maximize their transconductance ($g_m = 2I_D / V_{OD}$), which in turn minimizes the **input-referred noise**.
    *   The output swing requirements of Stage 1 are often less stringent than the final output swing, as the second stage amplifies those signals. However, the chosen $V_{OD}$ for Stage 1 devices sets the **minimum intermediate node voltage** (output of Stage 1), constraining the $V_{DS}$ available for the load devices of Stage 1.

4.  **Frequency Compensation:** Due to the inherent instability arising from two dominant poles (one at the high-impedance output of Stage 1 and one at the output of Stage 2), the final design must be stabilized. This typically involves:
    *   Introducing a **compensating capacitor ($C_C$)** (Miller compensation) between the input and output nodes of the second stage.
    *   Ensuring the necessary **phase margin (PM)** (often 60 degrees) by iteratively adjusting $C_C$ to move the dominant pole to a low frequency and split the non-dominant poles away from the unity-gain frequency.
    *   The Miller compensation capacitor may introduce a troublesome **right-half-plane zero** that degrades stability, potentially requiring a compensating resistor ($R_Z$) to shift the zero into the left half plane for stability.

5.  **Common-Mode Feedback (CMFB):** In fully differential two-stage op amps, CMFB is necessary to define and stabilize the intermediate and final output Common-Mode (CM) levels. This often requires **two separate CMFB loops**, one for each stage, complicating stability analysis due to the introduction of additional poles from the CMFB circuitry. If CMFB is derived from the final output and returned to the first stage, the instability risk is high.

## 9.4 Gain Boosting

Gain boosting is a technique applied primarily to cascode op amps (one-stage) to **increase the output impedance and voltage gain** without adding cascaded devices or sacrificing voltage headroom.

### 9.4.1 Basic Idea

1.  **Principle of Operation**: A small auxiliary amplifier ($A_1$) senses the cascode device's source voltage ($V_S$) or overdrive and feeds a correction signal back to its gate ($V_G$). This effectively increases the cascode device's small-signal resistance to ground. The source voltage $V_S$ is "pinned" close to a reference voltage ($V_{ref}$).
2.  **Equivalent $g_m$ Boost**: The addition of $A_1$ raises the effective transconductance of the cascode device ($M_2$) by approximately a factor of $A_1+1$.
3.  **Output Impedance Boost**: This effective $g_m$ boost increases the output impedance ($R_{out}$) by approximately a factor of $A_1+1$: $R_{out} \approx (A_1+1)g_{m2}r_{o2}r_{o1}$. The overall voltage gain scales proportionally.

### 9.4.2 Circuit Implementation

The auxiliary amplifier ($A_1$) is typically a simple common-source stage or a differential pair. Careful bias matching is required to set the operating point correctly. Gain boosting can be applied to both the signal path (NMOS cascode) and the load devices (PMOS cascode load) to achieve very high overall gain.

### 9.4.3 Frequency Response

While gain boosting significantly increases the DC gain, it does **not sacrifice bandwidth** in the same manner as adding a second stage, because the auxiliary amplifier only processes a small correction signal (the error component).

*   **Poles and Zero**: The auxiliary amplifier introduces a **second pole** into the system, typically located at $f_{p2} \approx (A_0+1)f_0$, where $f_0$ is the bandwidth of $A_1$. The system also introduces a **left-half-plane zero** at $f_z \approx (A_0+1)f_0$, which is used to cancel the detrimental effects of the second pole.
*   **Intuition**: The technique successfully boosts the gain while ensuring that the unity-gain bandwidth remains high, limited only by the speed of the cascode transistors themselves (rather than the DC gain of the auxiliary amplifier).

## 9.5 Comparison

The comparison of operational amplifier performance parameters across four main topologies—Telescopic, Folded-Cascode, Two-Stage, and Gain-Boosted—is crucial for understanding trade-offs in analog IC design. The key differences derive from how each structure achieves gain (one stage vs. two stages, cascoding vs. folding) and manages voltage headroom.

Here is a structured comparison of the performance characteristics for the major op amp topologies, derived from the general concepts covered in Chapter 9 of the sources:

| Metric | Telescopic | Folded−Cascode | Two−Stage | Gain−Boosted |
| :--- | :--- | :--- | :--- | :--- |
| **Gain** | High | Medium | High | **Highest** |
| **Output Swing** | **Low** | Medium | **Highest** | Medium |
| **Speed** | High | Medium | **Low** | High |
| **Power Dissipation** | **Low** | Medium | Medium | Medium |
| **Noise** | **Low** | Medium | Medium | Medium |

### Detailed Comparison Rationale

#### Gain

*   **Telescopic Op Amp**: Provides **High** gain by maximizing output impedance using cascoded devices (often approximated as the square of the intrinsic gain, $g_m r_o$).
*   **Folded-Cascode Op Amp**: Typically results in **Medium** (lower) gain compared to the telescopic structure due to the inherent loss associated with current mirrors and parallel output resistances.
*   **Two-Stage Op Amp**: Offers **High** gain because the overall gain is the product of the gains of two cascaded stages.
*   **Gain-Boosted Op Amp**: Provides the **Highest** gain potential. By utilizing auxiliary amplifiers to effectively increase the $g_m$ of the cascode devices by a factor of $A_1+1$, the overall output impedance—and thus the gain—is drastically increased without sacrificing voltage headroom.

#### Output Swing

*   **Telescopic Op Amp**: Has the most **Low** or limited output voltage swing because multiple transistors are stacked vertically, severely restricting voltage headroom (often limited by $\approx 4V_{OD}$).
*   **Folded-Cascode Op Amp**: Offers **Medium** output swing, generally wider than the telescopic cascode, because the folding technique avoids stacking input devices on top of cascode devices.
*   **Two-Stage Op Amp**: Achieves the **Highest** output swing, as the second stage is typically a simple Common-Source configuration optimized explicitly for maximizing rail-to-rail voltage excursion (constrained only by output device $V_{OD}$ and the supply rail voltages).
*   **Gain-Boosted Op Amp**: The swing remains **Medium** (limited by the same stacking constraints as the basic cascode structure it enhances), typically offering little improvement over the unboosted version.

#### Speed (Unity-Gain Bandwidth / GBW)

*   **Telescopic Op Amp**: Offers **High** speed because it is a single-stage design, meaning the dominant pole is usually placed well below the first non-dominant pole (often located at the source of the cascode device).
*   **Folded-Cascode Op Amp**: Exhibits **Medium** speed. Although a single stage, the "folding" introduces a potentially low-frequency mirror pole at the folding node, which can compromise stability and limit the achievable speed compared to the telescopic variant.
*   **Two-Stage Op Amp**: Is generally the **Low**-est speed option. Cascading stages introduces multiple poles, requiring significant frequency compensation (e.g., Miller compensation) to ensure stability, which drastically reduces the available unity-gain bandwidth.
*   **Gain-Boosted Op Amp**: Maintains **High** speed. The technique increases the DC gain without sacrificing bandwidth in the way adding a second stage would, ensuring the unity-gain frequency remains high.

#### Power Dissipation

*   **Telescopic Op Amp**: Shows the **Low**-est power dissipation. The bias current supplied to the input differential pair is reused by the cascode load devices, leading to high power efficiency.
*   **Folded-Cascode Op Amp**: Requires **Medium** power. The input pair and the cascode devices must be supplied by separate current sources due to the folding structure; thus, the input current is not reused, leading to lower power efficiency compared to the telescopic structure.
*   **Two-Stage Op Amp**: Requires **Medium** power, as two separate power-consuming gain stages are necessary for operation.
*   **Gain-Boosted Op Amp**: Requires **Medium** power, as the auxiliary amplifiers needed for boosting consume additional current beyond the main amplifier core.

#### Noise (Input-Referred Noise)

*   **Telescopic Op Amp**: Can achieve **Low** noise, especially when designed with a PMOS input differential pair, as PMOS transistors exhibit lower flicker noise ($1/f$) than NMOS devices.
*   **Folded-Cascode Op Amp**: Tends to show **Medium** noise levels, potentially higher than comparable telescopic designs.
*   **Two-Stage Op Amp**: Generally exhibits **Medium** noise. Noise performance is dominated by the first stage. Noise from the second stage is usually negligible when referred back to the input, as it is divided by the square of the first stage's voltage gain.
*   **Gain-Boosted Op Amp**: Presents **Medium** noise. While the input pair contribution remains proportional to $1/g_{m,in}$, the noise injected by the auxiliary amplifier stages must also be accounted for.

## 9.6 Output Swing Calculations

The output swing determines the dynamic range of the op amp.

*   **Constraint**: The output voltage swing is fundamentally limited by the need to keep transistors, especially those connected to the output node, in the **saturation region**.
*   **Measurement**: Since the boundary between saturation and triode regions diminishes in modern processes, the maximum allowed swing is typically determined via simulation by identifying the point where the **open-loop gain drops by an acceptable margin** (e.g., 10%).
*   **CMOS Tradeoff**: Telescopic cascodes suffer the most severe swing limitations, typically constrained by the sum of four overdrive voltages ($4V_{OD}$) and one threshold voltage ($V_{TH}$) relative to the supply rails.

## 9.7 Common-Mode Feedback

CMFB is essential for **high-gain fully differential op amps**.

### 9.7.1 Basic Concepts

*   **Problem**: In high-gain differential circuits using current-source loads, the output Common-Mode (CM) level is defined by the balance between opposing NMOS and PMOS current sources. Due to inherent manufacturing **mismatches** and the high output impedance, even small imbalances cause the CM level to drift severely toward $V_{DD}$ or ground.
*   **Solution**: CMFB measures the output CM level ($V_{out,CM}$), compares it against a reference ($V_{REF}$), and feeds the resulting error back to control the bias current of the op amp, forcing $V_{out,CM}$ to settle at $V_{REF}$.
*   **Differential vs. CM Feedback**: Differential feedback cannot solve the CM drift problem.

### 9.7.2 CM Sensing Techniques

1.  **Resistive Sensing**: Using large resistors ($R_1, R_2$) to average $V_{out1}$ and $V_{out2}$ (if $R_1 = R_2$). *Drawback*: Resistors must be very large (M$\Omega$ range) to avoid loading the high output impedance of the op amp, requiring substantial area and adding parasitic capacitance.
2.  **MOSFET Sensing (Triode Mode)**: Using MOSFETs ($M_7, M_8$) operating in the deep triode region as variable resistors. The equivalent resistance is inversely proportional to $V_{out,CM}$. *Drawback*: This sensing method limits the output swing significantly ($V_{out,min} \approx V_{TH}$) and adds large capacitance to the output.
3.  **Source Follower Sensing**: Using source followers followed by small resistors (e.g., Fig. 11.27) to sense the voltage while reducing loading. *Drawback*: Limits differential output swing due to the $V_{GS}$ drop across the followers.

### 9.7.3 CM Feedback Techniques

The error signal ($V_{e}$) from the CM sense circuit typically controls either the **NMOS tail current** (Stage 1) or the **PMOS load current** (Stage 2).

### 9.7.4 CMFB in Two-Stage Op Amps

Two-stage op amps are particularly challenging because they have CM levels defined in both Stage 1 and Stage 2.

*   **Implementation**: CMFB must be applied to control the CM levels of both stages, often requiring **two separate CM loops**.
*   **Stability**: If CMFB is derived from the final output and returned to the first stage, the loop involves poles from both stages (and the CM sensor/amplifier), complicating stability analysis and often requiring extensive compensation.

---

## 9.8 Input Range Limitations

The operating range of the input CM level is typically limited by the voltages necessary to keep the input differential pair and the tail current source active. If the input CM level is too low, the tail current source may enter the triode region. If it is too high, the input transistors themselves may enter the triode region.

*   **Solution**: To widen the input CM range, modern designs often use **complementary differential pairs** (both NMOS and PMOS input pairs) in parallel, ensuring that when one pair turns off near a rail, the other remains active. However, this leads to a varying overall transconductance.

## 9.9 Slew Rate

SR is the maximum rate of change of the output voltage ($\text{SR} = dV_{out}/dt$), dictated by the maximum current available to charge the dominant load capacitance ($C_L$).

*   **Behavior**: When a large input step is applied, the input differential pair often turns one side completely off, limiting the available current to the constant tail current ($I_{SS}$) or its mirror current, leading to $\text{SR} \approx I_{SS}/C_L$. The output is a **linear ramp**, which is a nonlinear phenomenon.
*   **SR-Power Tradeoff**: Increasing SR requires increasing $I_{SS}$ for a fixed $C_L$, which directly increases power consumption.

## 9.10 High-Slew-Rate Op Amps

To mitigate the SR-Power tradeoff, **Class-AB** or **push-pull** techniques are used. These circuits automatically **increase the current available** to charge $C_L$ during large transients, minimizing the need for high quiescent $I_{SS}$.

### 9.10.1 One-Stage Op Amps
*(Content to be added detailing high-SR techniques for one-stage op amps)*

### 9.10.2 Two-Stage Op Amps
*(Content to be added detailing high-SR techniques for two-stage op amps)*

## 9.11 Power Supply Rejection

PSRR measures the ratio of differential gain ($A_{DM}$) to the gain from the power supply to the output ($A_{PS}$). PSRR is critical in mixed-signal environments.

*   **Considerations**: In simple op amps, the DC PSRR is often limited because the diode-connected devices used as loads clamp nodes to $V_{DD}$, leading to $A_{PS} \approx 1$. PSRR generally degrades at high frequencies because capacitances couple supply noise more effectively.
*   **Design Strategies**: Fully differential topologies inherently improve PSRR by converting supply noise into a CM disturbance, which is then rejected.

## 9.12 Noise in Op Amps

The total input-referred noise voltage squared ($V_{n,in}^2$) often determines the achievable precision.

*   **Dominant Sources**: In most op amp topologies (telescopic or folded-cascode), the noise is dominated by the **input differential pair** and the **load current sources**.
*   **Design Insight**: $V_{n,in}^2$ of the input pair is proportional to $1/g_{m,in}$. Thus, design strategies focus on maximizing $g_{m,in}$ (e.g., using wide devices or higher current) relative to the $g_m$ of the current source loads. The noise contribution of the load devices is proportional to $g_{m,load} / g_{m,in}^2$.
*   **Flicker Noise ($1/f$)**: Generally, **PMOS input devices** are preferred in folded-cascode designs because PMOS transistors typically exhibit **less flicker noise** than NMOS devices.
*   **Tradeoffs**: $V_{n,in}$ increases as the overdrive voltage of current sources is reduced (to maximize output swing), leading to a tradeoff between **output swing and noise**. The principle of **linear scaling** allows designers to trade power for noise performance ($V_{n,in}^2$ scales inversely with power).