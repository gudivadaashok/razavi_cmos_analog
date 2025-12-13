**Chapter 5 – Current Mirrors and Biasing Techniques**

Current mirrors and biasing techniques are essential components in analog CMOS integrated circuit design, primarily serving to establish stable and accurate DC operating points for active devices and providing high-impedance current loads for amplification stages. Achieving precision requires dealing with non-ideal effects like channel-length modulation, low supply voltages, and process variations.

## **5.1 Basic Current Mirrors**

### **1. What the concept is**

*   **Purpose:** A basic current mirror is designed to accurately **replicate a reference current ($I_{REF}$) to produce one or more output currents ($I_{out}$)**. This allows complex circuits to be biased reliably based only on dimensional ratios.
*   **Intuitive and Physical Operation:** The design leverages the MOSFET saturation relationship $I_D \propto (V_{GS} - V_{TH})^2$. A **diode-connected transistor** ($M_1$, where gate and drain are shorted) forces $M_1$ into saturation and generates a unique gate-source voltage ($V_{GS}$) corresponding to the input current $I_{REF}$ ($V_{GS} = f^{-1}(I_{REF})$). This $V_{GS}$ is then applied to the output transistor ($M_2$). If $M_2$ is identical to $M_1$ and operating in saturation, $I_{out}$ will be equal to $I_{REF}$.
    *   The current ratio is determined by the aspect ratios: $\mathbf{I_{out} = \frac{(W/L)_2}{(W/L)_1} I_{REF}}$.
*   **Requirement in CMOS ICs:** Simple resistive biasing leads to output current sensitivity dependent on $V_{DD}$ and highly variable $V_{TH}$. Current mirrors provide highly predictable bias currents defined by **geometric ratios** ($W/L$) rather than absolute electrical parameters.
*   **Typical Applications:** Bias networks for amplifiers, as active loads in common-source stages, and within larger Op Amps/OTAs.

### **2. Challenges and Limitations**

*   **Current Accuracy:** Accuracy is degraded by **Channel-Length Modulation ($\lambda$)** because the output impedance ($r_o$) is finite. If the $V_{DS}$ of the output transistor ($M_2$) deviates from the $V_{DS}$ of the reference transistor ($M_1$), $I_{out}$ will deviate from the expected ratio.
*   **Voltage Headroom:** The diode-connected device requires $V_{DS} = V_{GS}$ to remain saturated, limiting the available voltage headroom, especially in low-supply voltage technologies.
*   **Device Mismatch:** Small random variations in physical dimensions lead to unpredictable errors in the desired current replication.

### **3. Practical techniques to mitigate these issues**

*   **Device Sizing and Matching:** To maximize accuracy, all MOSFETs in the mirror should use the **same channel length ($L$)** to minimize the impact of variations in effective channel length and threshold voltage. Scaling to multiply the current must use **unit transistors** (or fingers) rather than simply drawing one device with a large width ($W$), which otherwise introduces errors due to poorly defined gate corners.
*   **Output Resistance Improvement:** **Cascode current mirrors** are used to dramatically increase the output resistance, reducing sensitivity to $V_{DS}$ variations.

***

## **5.2 Cascode Current Mirrors**

### **1. What the concept is**

*   **Purpose:** The cascode mirror significantly **improves the output current accuracy** and increases the output resistance ($r_o$) of the mirror by mitigating the detrimental effects of Channel-Length Modulation.
*   **Intuitive Operation:** A cascode device ($M_3$) is stacked above the output transistor ($M_2$). $M_3$ acts as a **shield** (common-gate stage) that isolates $M_2$ from output voltage changes, ensuring that $M_2$ maintains a stable, low $V_{DS2}$ and thus maximizing current accuracy.
*   **Physical Operation (High $r_o$):** The structure increases the overall output resistance ($R_{out}$) by approximately the square of the intrinsic gain ($g_m r_o$) of a single transistor, $\mathbf{R_{out} \approx (g_{m3}r_{o3})r_{o2}}$, making it behave as a superior current source.
*   **Typical Applications:** Used as **active loads** in high-gain amplifiers (Op Amps, OTAs) and as precision bias current sources.

### **2. Challenges and Limitations**

*   **Headroom Constraint (Standard Cascode):** The standard cascode mirror (Fig. 5.12(c)) requires a minimum output voltage headroom of approximately **$V_{P,min} \approx 2V_{D,sat} + V_{TH}$** (two overdrive voltages plus one threshold voltage) to keep all transistors saturated. This constraint severely limits output swing in low-voltage circuits.
*   **Bias Generation Complexity:** Generating the cascode bias voltage ($V_b$) accurately while ensuring proper voltage levels (e.g., $V_{DS2} = V_{GS1}$) adds complexity to the design.

### **3. Practical techniques to mitigate these issues**

*   **Low-Voltage Cascodes (LVC):** This technique modifies the bias generation such that the output transistors ($M_2, M_3$) are biased near the edge of saturation ($V_{D,sat} \approx V_{GS} - V_{TH}$), reducing the required headroom to approximately **$V_{P,min} \approx 2V_{D,sat}$** (two overdrive voltages).
*   **Regulated Cascodes (Gain Boosting):** To achieve even higher output resistance, an auxiliary **gain-boosting amplifier** ($A_1$) can be used to monitor the source voltage of the cascode device and adjust its gate voltage via negative feedback, keeping the voltage nearly constant. This boosts the output resistance by a factor proportional to the gain of $A_1$.

***

## **5.3 Active Current Mirrors (Five-Transistor OTA)**

The five-transistor Operational Transconductance Amplifier (OTA) is a core differential amplifier block utilizing an active current mirror as a load to achieve high gain and differential-to-single-ended conversion.

### **5.3.1 Large-Signal Analysis**

*   **Concept/Purpose:** To find the relationship between the large input voltage swing and the resulting output voltage/current swing, confirming the circuit's high gain and operational limits.
*   **Operation:** The input differential pair ($M_1, M_2$) steers the tail current ($I_{SS}$). The active mirror ($M_3, M_4$) copies $I_{D1}$ to the output, where it adds constructively with $I_{D2}$.
*   **Limitation:** The minimum output DC level is constrained by the necessity of keeping the input transistors saturated ($V_{out} \ge V_{in,CM} - V_{THN}$), limiting maximum output swing.

### **5.3.2 Small-Signal Analysis**

*   **Concept/Purpose:** To determine the precise voltage gain $|A_v|$ and output resistance ($R_{out}$) around the DC bias point.
*   **Operation:** The active mirror results in a short-circuit transconductance $G_m$ that is approximately twice the $g_m$ of one input transistor ($\mathbf{G_m \approx g_{m1,2}}$) because both input currents contribute actively to the output. The output resistance is $R_{out} \approx r_{o2} \parallel r_{o4}$.
*   **Gain:** $\mathbf{|A_v| \approx g_{m1,2} (r_{o2} \parallel r_{o4})}$.

### **5.3.3 Common-Mode Properties**

*   **Concept/Purpose:** To analyze the circuit's rejection of Common-Mode (CM) noise and disturbances.
*   **Challenges:** The OTA inherently suffers from **Common-Mode to Differential-Mode Conversion** ($A_{CM-DM}$) due to the asymmetrical load structure and device mismatches. The finite output impedance of the tail current source is a major contributor to CM gain.
*   **CMRR:** The CMRR is degraded by the $g_m$ mismatch ($\Delta g_m$) and the output resistance of the tail current source ($R_{SS}$). High accuracy requires maximizing $R_{SS}$ and minimizing transistor mismatches.

### **5.3.4 Other Properties of the Five-Transistor OTA**

*   **Tradeoffs:** The OTA suffers from **inferior Power Supply Rejection** compared to fully differential architectures because the single-ended output node is coupled directly to the supply line.
*   **Mitigation:** Low-voltage techniques (like level shifting the PMOS gate) can reduce headroom constraints. However, achieving the highest performance (in terms of CMRR and PSRR) generally requires migrating to **fully differential topologies** rather than single-ended OTAs.

***

## **5.4 Biasing Techniques**

The comprehensive discussion of biasing techniques, covered primarily in Section 5.4 of the analog integrated circuit (IC) study material, details the crucial mechanisms required to establish stable and accurate DC operating points (quiescent points) for transistors used in amplification stages. Biasing is necessary because transistors in analog circuits operate as controlled elements, and defining their DC currents ($I_D$) and voltages ($V_{GS}$, $V_{DS}$) directly sets their performance parameters like transconductance ($g_m$) and output resistance ($r_o$).

Establishing reliable bias currents is traditionally handled using **current mirrors**, which leverage geometric ratios rather than absolute electrical parameters, thus providing predictable current references. The following subsections detail how these established reference currents and voltages are then applied to specific amplifier topologies.

### **I. Common-Source (CS) Biasing (Section 5.4.1)**

The purpose of CS biasing is to set a constant bias current ($I_D$) and define the necessary DC terminal voltages for CS amplifiers, ensuring the transistor operates efficiently for amplification.

**Concept and Operation**

In typical biasing, the input signal ($V_{in}$) is applied through a **coupling capacitor ($C_B$)** to block the DC bias voltage ($V_B$). $V_B$ is then applied to the gate through a high resistance ($R_B$), which sets the required $V_{GS}$ reliably. The DC bias voltage ($V_B$) itself is typically generated by a **diode-connected device** (a reference derived from a current mirror).

**Challenges and Limitations**

*   **Area Consumption:** Large passive components like $C_B$ and $R_B$ are often required for low-frequency signals, consuming excessive chip area.
*   **Fighting Current Sources:** If a CS stage uses a current-source load, the input transistor ($M_1$) and the load transistor ($M_2$) may "fight" to impose their current, leading to an unstable DC output voltage.
*   **PVT Sensitivity:** In cascaded stages, the interdependence of DC bias conditions amplifies Process, Voltage, and Temperature (PVT) variations.

**Mitigation Techniques**

*   **Area Saving:** MOSFETs operating in the **deep triode region** can replace the large bias resistor ($R_B$) to save area.
*   **Self-Biasing:** When using a current-source load, the load device ($M_2$) can be configured as a diode-connected transistor at DC (by bypassing its gate to ground with a large capacitor, $C_G$) to resolve fighting issues, allowing $M_2$ to accept the current imposed by $M_1$.

### **II. Common-Gate (CG) Biasing (Section 5.4.2)**

CG stages require the bias current ($I_{D1}$) to be established, typically as a multiple of a reference current ($I_B$) using a current mirror.

**Concept and Operation**

The transistor senses the input signal at its source terminal, meaning the source cannot be tied directly to ground. A passive element like a resistor ($R_S$) or an active element like a current source connects the source terminal to ground.

**Challenges and Limitations**

*   **Headroom Constraint:** If a resistor ($R_S$) connects the source to ground, maximizing $R_S$ (necessary to prevent signal attenuation) creates a large DC voltage drop, severely limiting the available voltage headroom for the load and restricting achievable voltage gain.
*   **Accuracy Degradation:** Biasing current accuracy can suffer due to channel-length modulation if the current source defining $I_{D1}$ is not cascoded.

**Mitigation Techniques**

*   **Current-Source Solution:** Replacing the resistor ($R_S$) with a high-impedance current source ($M_2$) limits current flow while minimizing the voltage headroom consumed ($V_{DS,min}$).
*   **Improved Accuracy:** Using **low-voltage cascode** topologies for the biasing current source is necessary to improve the accuracy of the established bias current.

### **III. Source Follower Biasing (Section 5.4.3)**

Source followers define $I_{D1}$ using a current source ($M_2$) and are less sensitive to variations in their gate voltage compared to CS amplifiers.

**Concept and Operation**

A current source ($M_2$) establishes the bias current ($I_{D1}$). Due to the inherent voltage tracking, the source follower's bias current exhibits less sensitivity to its gate voltage variation compared to a CS amplifier.

**Challenges and Limitations**

*   **Cascading Constraint:** When a source follower buffers a preceding CS stage, the minimum drain voltage of the CS stage must meet the high minimum voltage requirement of the follower ($V_{GS} + V_{DS,min}$). This constraint dramatically reduces the achievable CS stage voltage gain due to limited voltage drop across the CS load.

**Mitigation Techniques**

*   **Decoupling DC Bias:** **Capacitive coupling** should be used between the preceding CS stage and the source follower to decouple their DC bias points, allowing the CS gain to be maximized independently of the source follower's high DC headroom requirements.

### **IV. Differential Pair Biasing (Section 5.4.4)**

Biasing differential pairs requires defining two elements: the tail current source and the input common-mode voltage ($V_{in,CM}$).

**Concept and Operation**

A **tail current source ($I_{SS}$)** is used to ensure that the sum of the drain currents ($I_{D1} + I_{D2}$) remains constant, regardless of $V_{in,CM}$ variations. The purpose is to define a stable DC operating point for the input differential pair.

**Challenges and Limitations**

*   **Input CM Range Limitation:** The minimum acceptable input CM voltage is constrained by the necessity of keeping all three transistors (the input pair $M_{1,2}$ and the tail current source $M_3$) operating in saturation: $V_{in,CM} \ge V_{GS1,2} + V_{DS3,min}$.
*   **Direct Coupling Difficulty:** The high output CM level of preceding amplifier stages often conflicts with the required input CM voltage of the differential pair, limiting the overall cascaded gain potential.

**Mitigation Techniques**

*   **Maximizing Swing:** $V_{in,CM}$ is typically selected to be **as low as possible** (i.e., equal to $V_{GS1,2} + V_{DS3,min}$) to maximize the available output voltage swings.
*   **Cascading:** **Capacitive coupling** is often utilized when cascading differential pairs to allow local bias points to be defined independently, facilitating the maximization of multi-stage gain.