# Chapter 12: Bandgap References

## 12.1 General
**What the concept is**
A circuit that generates a precise, stable voltage (typically $V_{REF} \approx 1.25V$) that is independent of Process, Voltage (Supply), and Temperature (PVT).

**Why it is used**
**The Anchor.** It is the only "known" voltage on the chip. All ADCs, DACs, and Regulators measure signals relative to this reference. If the Bandgap is off by 1%, the entire chip's measurements are off by 1%.

**The Bad / Backpack**
- **Complexity:** It requires bipolar transistors (in CMOS), precision resistors, and careful trimming.
- **Noise:** It is often the dominant noise source in high-precision systems.

**Practical techniques to mitigate**
- **Trimming:** Almost all commercial bandgaps include a digital trim network (fuses) to calibrate the absolute voltage after manufacturing.

## 12.2 Supply-Independent Biasing
**What the concept is**
A "bootstrap" current source where the current is defined by a resistor and device geometry, not by $V_{DD}$.

**Why it is used**
To ensure the reference doesn't move when the battery voltage drops or the digital logic causes supply ripple.

**The Bad / Backpack**
- **The "Zero" State:** The circuit has two stable operating points: the desired one, and "zero current". If not careful, the chip will power up and stay dead.

**Practical techniques to mitigate**
- **Start-up Circuit:** A small circuit that detects if the bias line is low and injects a pulse of current to "kick" the loop into the active state, then disconnects itself.

## 12.3 Temperature-Independent References
**What the concept is**
Combining two voltages with opposite Temperature Coefficients (TC) to cancel out the temperature dependence.
$V_{REF} = V_{CTAT} + K \times V_{PTAT}$.

**Why it is used**
To achieve a near-zero TC (e.g., < 50 ppm/°C) over the industrial temp range (-40°C to 125°C).

**The Bad / Backpack**
- **Curvature:** The cancellation is only first-order. The resulting $V_{REF}$ vs. Temp curve looks like a bow (parabola), not a flat line.

**Practical techniques to mitigate**
- **Curvature Correction:** Advanced circuits add a non-linear term (e.g., $T^2$) to flatten the bow, achieving < 10 ppm/°C.

### 12.3.1 Negative-TC Voltage (CTAT)
**What the concept is**
Complementary To Absolute Temperature. The Base-Emitter voltage ($V_{BE}$) of a BJT (or diode) decreases with temperature at roughly -2 mV/°C.

**Why it is used**
It is the naturally available voltage reference in silicon.

**The Bad / Backpack**
- **Parasitic BJT:** In standard CMOS, we use the parasitic vertical PNP transistor (formed by p-substrate, n-well, p+ diffusion). The collector is tied to the substrate (ground), limiting circuit topologies.

**Practical techniques to mitigate**
- **Diode Mode:** Use the PNP as a diode by tying Base to Collector (Ground).

### 12.3.2 Positive-TC Voltage (PTAT)
**What the concept is**
Proportional To Absolute Temperature. The difference in $V_{BE}$ between two BJTs running at different current densities.
$\Delta V_{BE} = V_T \ln(n)$, where $V_T = kT/q$. Since $V_T \propto T$, this voltage rises linearly with temperature (+0.085 mV/°C).

**Why it is used**
To compensate for the negative TC of $V_{BE}$.

**The Bad / Backpack**
- **Small Signal:** The voltage is small (~50-100mV). It needs to be amplified.

**Practical techniques to mitigate**
- **Ratio $n$:** Use a large ratio of emitter areas (e.g., 1:8) to maximize $\Delta V_{BE}$. Layout them in a common-centroid square (3x3 with center as the 1 unit).

### 12.3.3 Bandgap Reference
**What the concept is**
The sum: $V_{REF} = V_{BE} + M \cdot \Delta V_{BE}$. By choosing $M$ correctly, the sum is $\approx 1.25V$ (the bandgap energy of silicon extrapolated to 0K).

**Why it is used**
The standard recipe for voltage references.

**The Bad / Backpack**
- **Fixed Voltage:** It naturally produces ~1.25V. This is a problem for chips running on 1.0V or 0.9V supplies.

**Practical techniques to mitigate**
- **Current Mode:** Generate $I_{PTAT}$ and $I_{CTAT}$ currents, sum them, and drive a resistor. The output voltage can be set to any value (e.g., 0.5V) by sizing the resistor.

## 12.4 PTAT Generation
**What the concept is**
A feedback loop that forces $\Delta V_{BE}$ across a resistor $R$. The resulting current is $I = \Delta V_{BE} / R = (V_T \ln n) / R$.

**Why it is used**
To generate the PTAT component.

**The Bad / Backpack**
- **Offset:** The OpAmp used in the loop has an input offset voltage. This offset is amplified and creates a huge error in the PTAT current.

**Practical techniques to mitigate**
- **Large Devices:** Use large input devices for the OpAmp to minimize offset.
- **Chopping:** Use chopper stabilization to dynamically cancel the OpAmp offset.

## 12.5 Constant-Gm Biasing
**What the concept is**
A biasing technique where the transconductance ($g_m$) of the MOSFETs is determined by a geometric ratio and a physical resistor, rather than by the supply voltage or absolute process parameters.
- **Circuit:** A current mirror loop where one side has a source degeneration resistor $R_S$.
- **Equation:** $g_m = \frac{2 (1 - 1/\sqrt{K})}{R_S}$, where $K$ is the mirror ratio.

**Why it is used**
**Stabilization.** Many analog parameters (Gain $= g_m R_{out}$, Bandwidth $\approx g_m/C_L$) depend directly on $g_m$. If $g_m$ is stabilized against PVT variations, the circuit performance becomes predictable.

**The Bad / Backpack**
- **Resistor Dependence:** The $g_m$ becomes inversely proportional to the resistor value ($R_S$). If $R_S$ varies with temperature (which it does), $g_m$ will vary.
- **Start-up:** Like all self-biased loops, it has a "zero-current" stable state and requires a start-up circuit.

**Practical techniques to mitigate**
- **Temperature Compensation:** Use a resistor material with a low Temperature Coefficient (TC) or a specific TC that cancels other variations.
- **Off-Chip Resistor:** For extreme precision, use an external precision resistor to set the internal bias current.

## 12.6 Speed and Noise Issues
**What the concept is**
Bandgap references are not ideal DC sources; they generate noise and have finite response times to supply transients.

**Why it is used**
Understanding these limitations is crucial for high-performance systems (e.g., RF or high-resolution ADCs).

**The Bad / Backpack**
- **Output Noise:** The resistors generate thermal noise ($4kTR$), and the OpAmp/Transistors generate flicker ($1/f$) and shot noise. This noise is amplified by the loop gain.
- **PSRR (Power Supply Rejection Ratio):** At high frequencies, the OpAmp loop gain drops, allowing supply noise to couple directly to the reference output.

**Practical techniques to mitigate**
- **Bypass Capacitor:** Place a large capacitor (e.g., $0.1\mu F - 10\mu F$) at the output to filter wideband noise and provide a charge reservoir for transients.
- **Low-Noise Design:** Use large devices for the input pair of the OpAmp to minimize flicker noise. Use low-resistance values (at the cost of higher power) to reduce thermal noise.

## 12.7 Low-Voltage Bandgap References
**What the concept is**
Architectures designed to operate with supply voltages ($V_{DD}$) below the standard bandgap voltage of $\approx 1.25V$ (e.g., for $0.9V$ or $1.0V$ supplies).

**Why it is used**
In modern deep-submicron processes (7nm, 5nm), the core supply voltage is often $< 1V$. A traditional bandgap that stacks a diode ($0.7V$) and requires headroom for current sources cannot fit.

**The Bad / Backpack**
- **Headroom Constraint:** The classic topology ($V_{ref} = V_{BE} + V_{PTAT}$) is physically impossible if $V_{DD} < 1.2V$.

**Practical techniques to mitigate**
- **Current-Mode Architecture:** Instead of summing voltages, sum currents.
    1.  Generate $I_{CTAT} = V_{BE} / R_1$.
    2.  Generate $I_{PTAT} = \Delta V_{BE} / R_2$.
    3.  Sum them: $I_{ref} = I_{CTAT} + I_{PTAT}$.
    4.  Drive a resistor $R_{out}$: $V_{ref} = R_{out} (I_{CTAT} + I_{PTAT})$.
    - By choosing $R_{out}$, $V_{ref}$ can be set to any level (e.g., $0.5V$) while maintaining zero TC.

## 12.8 Case Study
**What the concept is**
A holistic look at a complete bandgap subsystem, integrating the core, the start-up circuit, curvature correction, and trimming networks.

**Why it is used**
To ensure robustness in real-world conditions. A core might work in simulation but fail in production due to start-up issues or process corners.

**The Bad / Backpack**
- **Loop Interaction:** The start-up circuit might inadvertently trigger during a supply dip (brown-out), causing a glitch in the reference voltage.
- **Stability:** The complex feedback loops (especially with the large bypass capacitor) can ring or oscillate if not properly compensated.

**Practical techniques to mitigate**
- **Hysteresis:** Design the start-up circuit with hysteresis so it disengages decisively once the core is active.
- **Monte Carlo Simulation:** Run thousands of simulations varying process parameters to ensure the circuit starts up and remains stable across all possible manufacturing variations.
