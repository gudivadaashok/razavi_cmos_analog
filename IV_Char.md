# How to Generate I-V Characteristics for NMOS and PMOS in LTspice

This guide demonstrates how to generate and analyze the I-V characteristics of MOS transistors, showing the different regions of operation: **cutoff**, **triode (linear)**, and **saturation**.

## Table of Contents
1. [NMOS I-V Characteristics](#nmos-i-v-characteristics)
2. [PMOS I-V Characteristics](#pmos-i-v-characteristics)
3. [Understanding Regions of Operation](#understanding-regions-of-operation)
4. [Advanced Analysis](#advanced-analysis)

---

## NMOS I-V Characteristics

### Basic NMOS I-V Curve Setup

**Circuit Description:**
- Sweep VDS (drain-to-source voltage) while varying VGS (gate-to-source voltage)
- Measure IDS (drain-to-source current) for different VGS values

**SPICE Netlist:**
```spice
* NMOS I-V Characteristics
.title NMOS IV Curve

* NMOS transistor
M1 Vd Vg 0 0 NMOS W=10u L=180n

* Voltage sources
Vds Vd 0 0
Vgs Vg 0 0

* Model parameters (adjust for your technology)
.model NMOS NMOS (
+ VTO=0.7 KP=200u LAMBDA=0.05
+ TOX=4n PHI=0.7
+ CGSO=0.2p CGDO=0.2p
+ CJ=1m CJSW=0.3p)

* DC sweep: VDS from 0 to 3.3V, VGS stepped from 0 to 2V
.dc Vds 0 3.3 0.01 Vgs 0 2 0.2

* Measurements
.meas DC Ids_sat FIND I(Vds) WHEN Vds=3.3 AT Vgs=1.5
.meas DC gm DERIV I(Vds) AT Vgs=1.2

.control
plot I(Vds)
.endc

.end
```

**Step-by-Step Instructions:**

1. **Create New Schematic in LTspice:**
   - File → New Schematic
   - Press `F2` or click "Component" button

2. **Add Components:**
   - Add NMOS: Type `nmos` and select
   - Add 2 voltage sources: Type `voltage`
   - Connect circuit:
     ```
     Vgs → Gate
     Vds → Drain
     Source and Bulk → Ground (0)
     ```

3. **Set Component Values:**
   - Right-click NMOS → Set W=10u, L=180n
   - Right-click Vgs → Advanced → DC value = 0
   - Right-click Vds → Advanced → DC value = 0

4. **Add SPICE Directives:**
   - Press `S` (or Edit → SPICE Directive)
   - Add model: `.model NMOS NMOS (VTO=0.7 KP=200u LAMBDA=0.05)`
   - Add DC sweep: `.dc Vds 0 3.3 0.01 Vgs 0 2 0.2`

5. **Run Simulation:**
   - Click Run button or press `Ctrl+R`
   - Plot will show IDS vs VDS for different VGS values

6. **Analyze Results:**
   - Click on drain node to plot `I(Vds)`
   - You'll see multiple curves (one for each VGS)
   - Right-click plot → View → Mark Data Points for VGS values

**Expected Output:**
- Family of curves showing IDS vs VDS
- Each curve represents a different VGS value
- Clear visualization of **triode** and **saturation** regions

---

## PMOS I-V Characteristics

### Basic PMOS I-V Curve Setup

**Circuit Description:**
- For PMOS, voltages are typically negative
- Sweep VSD (source-to-drain) while varying VSG (source-to-gate)

**SPICE Netlist:**
```spice
* PMOS I-V Characteristics
.title PMOS IV Curve

* PMOS transistor (source at VDD, drain swept)
M1 Vd Vg Vdd Vdd PMOS W=20u L=180n

* Voltage sources
Vdd Vdd 0 3.3
Vds Vd 0 3.3
Vgs Vg 0 3.3

* Model parameters
.model PMOS PMOS (
+ VTO=-0.7 KP=80u LAMBDA=0.06
+ TOX=4n PHI=0.7
+ CGSO=0.2p CGDO=0.2p
+ CJ=1m CJSW=0.3p)

* DC sweep: VDS from 3.3V down to 0V, VGS from 3.3V to 1V
* This sweeps VSD from 0 to 3.3V and VSG from 0 to 2.3V
.dc Vds 3.3 0 -0.01 Vgs 3.3 1 -0.2

* Measurements
.meas DC Isd_sat FIND -I(Vds) WHEN Vds=0 AT Vgs=1.5
.meas DC gm_p DERIV -I(Vds) AT Vgs=2.0

.control
plot -I(Vds)
.endc

.end
```

**Alternative: Referenced to Source at VDD**

For easier interpretation, you can reference everything to VDD:

```spice
* PMOS I-V with VSG and VSD
.title PMOS IV Curve (VSD/VSG notation)

Vdd Vdd 0 3.3
M1 Vd Vg Vdd Vdd PMOS W=20u L=180n
Vsd Vdd Vd 0  ; Source-to-Drain voltage
Vsg Vdd Vg 0  ; Source-to-Gate voltage

.model PMOS PMOS (VTO=-0.7 KP=80u LAMBDA=0.06)

* Sweep VSD from 0 to 3.3V, VSG from 0 to 2.3V
.dc Vsd 0 3.3 0.01 Vsg 0 2.3 0.2

.control
plot I(Vsd)
.endc

.end
```

**Step-by-Step Instructions:**

1. **Create New Schematic:**
   - Add PMOS transistor (press F2, type `pmos`)
   
2. **Add Components:**
   - Add 3 voltage sources: Vdd, Vds, Vgs
   - Connect:
     ```
     Source and Bulk → Vdd
     Gate → Vgs
     Drain → Vds
     ```

3. **Set Values:**
   - PMOS: W=20u, L=180n (PMOS typically wider due to lower mobility)
   - Vdd = 3.3V
   - Vgs = 3.3V (starting value)
   - Vds = 3.3V (starting value)

4. **Add Directives:**
   - Model: `.model PMOS PMOS (VTO=-0.7 KP=80u LAMBDA=0.06)`
   - Sweep: `.dc Vds 3.3 0 -0.01 Vgs 3.3 1 -0.2`

5. **Run and Plot:**
   - Run simulation
   - Plot `-I(Vds)` (negative because current flows opposite direction)
   - Curves show |ISD| vs VSD for different VSG values

---

## Understanding Regions of Operation

### NMOS Operating Regions

#### 1. **Cutoff Region** (VGS < VTH)
```spice
* Demonstrating Cutoff
.dc Vds 0 3.3 0.01 Vgs 0 0.6 0.1

* When VGS < VTH (~0.7V):
* IDS ≈ 0 (only subthreshold leakage)
```

**Characteristics:**
- IDS ≈ 0 (practically zero, except subthreshold current)
- Transistor is OFF
- Used in digital circuits for logic '0' or '1' state

#### 2. **Triode/Linear Region** (VGS > VTH and VDS < VGS - VTH)
```spice
* Demonstrating Triode Region
.dc Vds 0 0.5 0.01 Vgs 0.8 2 0.2

* Current equation (simplified):
* IDS ≈ KP*(W/L)*[(VGS-VTH)*VDS - VDS²/2]
```

**Characteristics:**
- IDS increases linearly with VDS (for small VDS)
- Channel is continuous from source to drain
- Acts like a voltage-controlled resistor
- RDS,on = 1 / [KP*(W/L)*(VGS-VTH)]

#### 3. **Saturation Region** (VGS > VTH and VDS ≥ VGS - VTH)
```spice
* Demonstrating Saturation
.dc Vds 0 3.3 0.01 Vgs 0.8 2 0.2

* Current equation (simplified):
* IDS ≈ (KP/2)*(W/L)*(VGS-VTH)² * (1 + LAMBDA*VDS)
```

**Characteristics:**
- IDS relatively constant with VDS (flat curves)
- Slight increase due to channel-length modulation (λ)
- Channel is pinched off at drain end
- Used in analog amplifiers (constant current source)

### Identifying Regions on I-V Curves

**Visual Indicators:**

```spice
* Generate annotated I-V curves
.dc Vds 0 3.3 0.01 Vgs 0.6 2.0 0.2

* Add measurements to identify regions
.meas DC Vdsat FIND Vds WHEN d(I(Vds))/d(Vds)=0.1 AT Vgs=1.5
.meas DC Ids_tri FIND I(Vds) WHEN Vds=0.2 AT Vgs=1.5
.meas DC Ids_sat FIND I(Vds) WHEN Vds=3.3 AT Vgs=1.5
.meas DC rds_on PARAM 0.2/Ids_tri
```

**Boundary Conditions:**
- **Triode-Saturation boundary:** VDS = VGS - VTH (knee of the curve)
- **Cutoff-Triode boundary:** VGS = VTH (where curves lift off from zero)

---

## Advanced Analysis

### 1. Extract Key Parameters

**Threshold Voltage (VTH):**
```spice
* Extract VTH using constant IDS method
.dc Vgs 0 2 0.01
.meas DC Vth FIND Vgs WHEN I(Vds)=100n

* Or using transconductance method
.meas DC Vth_gm FIND Vgs WHEN deriv(I(Vds))=MAX deriv(I(Vds))
```

**Transconductance (gm):**
```spice
* gm = dIDS/dVGS at constant VDS
.dc Vgs 0 2 0.01
.meas DC gm DERIV I(Vds)

* Plot gm:
.control
let gm = deriv(I(Vds))
plot gm
.endc
```

**Output Resistance (rds):**
```spice
* rds = dVDS/dIDS at constant VGS
.dc Vds 0 3.3 0.01
.param vgs_fixed=1.5
.meas DC rds DERIV 1/I(Vds) AT Vds=1.65

* Or calculate from lambda:
* rds = 1/(LAMBDA*IDS)
```

**Channel-Length Modulation (λ):**
```spice
* Extract lambda from slope in saturation
.dc Vds 0 3.3 0.01
.param Vgs_op=1.5
.meas DC Ids1 FIND I(Vds) AT Vds=1.65
.meas DC Ids2 FIND I(Vds) AT Vds=3.3
.meas DC lambda PARAM (Ids2/Ids1-1)/(3.3-1.65)
```

### 2. Temperature Effects

```spice
* Sweep temperature to see mobility variation
.dc Vds 0 3.3 0.01 Vgs 1.2 2 0.2
.step TEMP -40 125 25

* Temperature affects:
* - VTH decreases with temperature (~-2mV/°C)
* - Mobility decreases with temperature
* - IDS decreases with temperature in strong inversion
```

### 3. Width/Length Ratio Effects

```spice
* Compare different W/L ratios
.param W_val=10u
.step PARAM W_val LIST 5u 10u 20u 40u

M1 Vd Vg 0 0 NMOS W={W_val} L=180n

.dc Vds 0 3.3 0.01 Vgs 1.0 2.0 0.25

* Observe:
* - IDS scales linearly with W/L
* - gm scales linearly with W/L
* - Same VTH for all W/L (ideally)
```

### 4. Subthreshold Characteristics

```spice
* Zoom into subthreshold region
.dc Vgs 0 1.0 0.01
.param Vds_fixed=1.8

Vds Vd 0 {Vds_fixed}
Vgs Vg 0 0

* Plot on log scale:
.control
plot abs(I(Vds)) ylog
.endc

* Subthreshold swing measurement:
.meas DC SS DERIV Vgs/log10(I(Vds)) AT Vgs=0.5
* Ideal SS = 60 mV/decade at room temperature
```

### 5. Body Effect (Bulk Bias)

```spice
* Vary bulk voltage to see threshold voltage change
.dc Vds 0 3.3 0.01 Vgs 1.0 2.0 0.2
.step Vsb 0 1.5 0.3

M1 Vd Vg 0 Vsb NMOS W=10u L=180n
Vsb Vsb 0 0

* Observe VTH increase with VSB (body effect)
* VTH = VTH0 + gamma*(sqrt(2*phi_F + VSB) - sqrt(2*phi_F))
```

### 6. Early Voltage

```spice
* Extract Early voltage (VA = 1/lambda)
.dc Vds 0 3.3 0.01
.param Vgs_test=1.5

* Extrapolate saturation region line to x-axis
.meas DC slope DERIV I(Vds) WHEN Vds=2.0
.meas DC Ids_at_2V FIND I(Vds) WHEN Vds=2.0
.meas DC Early_V PARAM -Ids_at_2V/slope + 2.0

* Higher Early voltage = better current source
```

### 7. Complete Characterization Circuit

```spice
* Comprehensive MOSFET Characterization
.title Complete NMOS Characterization

* Device Under Test
M1 Vd Vg Vs Vb NMOS W=10u L=180n

* Voltage sources
Vds Vd Vs 0
Vgs Vg Vs 0
Vbs Vb Vs 0
Vss Vs 0 0

* Model
.model NMOS NMOS (VTO=0.7 KP=200u LAMBDA=0.05 GAMMA=0.4 PHI=0.7)

* Analysis 1: IDS vs VDS for different VGS
.dc Vds 0 3.3 0.01 Vgs 0.6 2.0 0.2

* Analysis 2: IDS vs VGS (transfer characteristics)
;.dc Vgs 0 2.0 0.01 Vds 0.1 3.3 0.8

* Analysis 3: Body effect
;.dc Vgs 0 2.0 0.01 Vbs 0 -1.5 -0.3

* Measurements
.meas DC Vth FIND Vgs WHEN I(Vds)=100n
.meas DC gm_max MAX deriv(I(Vds))
.meas DC Ids_sat FIND I(Vds) WHEN Vds=3.3 AT Vgs=1.8

.end
```

---

## Practical Tips

### Plotting Tips in LTspice

1. **Add cursor measurements:**
   - Right-click plot → Add Cursor
   - Shows exact coordinates

2. **View multiple parameters:**
   ```
   Plot I(Vds), V(Vg), V(Vd) on same graph
   ```

3. **Calculate derivatives:**
   - Right-click trace → View → Derivative
   - Shows gm or output conductance

4. **Export data:**
   - File → Export → Export data as text

### Common Issues and Solutions

**Issue: Convergence problems**
```spice
.options GMIN=1e-15 ABSTOL=1e-15 RELTOL=0.001
.options ITL1=300 ITL2=200 ITL4=100
```

**Issue: Not seeing saturation region**
- Increase VDS sweep range
- Check if lambda is too small

**Issue: Curves look non-ideal**
- Check model parameters
- Verify LAMBDA > 0 for saturation slope
- Ensure proper W and L values

---

## References

- **LTspice Documentation:** [LTspice Help](https://www.analog.com/en/design-center/design-tools-and-calculators/ltspice-simulator.html)
- **MOSFET Models:** [BSIM Model Documentation](http://bsim.berkeley.edu/)
- **Razavi, "Design of Analog CMOS Integrated Circuits"** - Chapter 2: Basic MOS Device Physics
- **Gray & Meyer, "Analysis and Design of Analog Integrated Circuits"** - Chapter 1: Models for Integrated Circuit Active Devices