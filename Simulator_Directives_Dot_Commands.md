# Simulator Directives -- Dot Commands

To run a simulation, not only must the circuit be defined, but also the type of analysis to be performed. There are six different types of analyses: linearized small signal AC, DC sweep, noise, DC operating point, small signal DC transfer function and transient analysis. Precisely one of these six analyses must be specified when you run a simulation.

Whereas the circuit topology is typically schematically drafted, the commands are usually placed on the schematic as text. All such commands start with a period and are called "dot commands".

**Reference:** [LTspice Help Documentation](https://www.analog.com/en/design-center/design-tools-and-calculators/ltspice-simulator.html)

## Analysis Commands

### .AC -- Perform an Small Signal AC Analysis Linearized About the DC Operating Point

Performs a small-signal AC analysis by linearizing the circuit around its DC operating point.

**Syntax:**
```spice
.AC <oct|dec|lin> <Nsteps> <StartFreq> <EndFreq>
```

**Example:**
```spice
.AC dec 10 1 100k
* Decade sweep, 10 points per decade, from 1Hz to 100kHz

.AC lin 100 1k 10k
* Linear sweep, 100 points, from 1kHz to 10kHz
```

**Documentation:** [LTspice .AC Documentation](https://www.analog.com/en/technical-articles/ltspice-basic-ac-analysis.html)

---

### .DC -- Perform a DC Source Sweep Analysis

Sweeps a DC source and computes the DC operating point at each step.

**Syntax:**
```spice
.DC <source> <start> <stop> <increment> [<source2> <start2> <stop2> <increment2>]
```

**Example:**
```spice
.DC Vin 0 5 0.1
* Sweep Vin from 0V to 5V in 0.1V steps

.DC Vdd 0 3.3 0.1 Temp 0 100 25
* Nested sweep: Vdd from 0 to 3.3V, at temperatures 0, 25, 50, 75, 100°C
```

**Documentation:** [LTspice .DC Documentation](https://www.analog.com/en/technical-articles/ltspice-dc-sweep-analysis.html)

---

### .NOISE -- Perform a Noise Analysis

Computes the noise contributions from all noise sources in the circuit.

**Syntax:**
```spice
.NOISE V(<output>) <source> <oct|dec|lin> <Nsteps> <StartFreq> <EndFreq>
```

**Example:**
```spice
.NOISE V(out) Vin dec 10 1 100k
* Noise analysis from 1Hz to 100kHz, referred to input source Vin
```

**Documentation:** [LTspice .NOISE Documentation](https://www.analog.com/en/technical-articles/ltspice-noise-analysis.html)

---

### .OP -- Find the DC Operating Point

Computes and outputs the DC operating point of the circuit.

**Syntax:**
```spice
.OP
```

**Example:**
```spice
.OP
* Displays all node voltages and device operating points
```

**Documentation:** [LTspice .OP Documentation](https://ltwiki.org/LTspiceHelp/LTspiceHelp/OP_Find_DC_Operating_Point.htm)

---

### .TF -- Find the DC Small-Signal Transfer Function

Calculates the small-signal DC transfer function, input resistance, and output resistance.

**Syntax:**
```spice
.TF V(<output>) <source>
```

**Example:**
```spice
.TF V(out) Vin
* Computes dV(out)/dVin, input resistance at Vin, output resistance at out
```

**Documentation:** [LTspice .TF Documentation](https://ltwiki.org/LTspiceHelp/LTspiceHelp/TF_Small_Signal_DC_Transfer_Function.htm)

---

### .TRAN -- Do a Nonlinear Transient Analysis

Performs time-domain transient analysis of the circuit.

**Syntax:**
```spice
.TRAN <Tstep> <Tstop> [Tstart] [Tmax] [uic]
```

**Example:**
```spice
.TRAN 10n 1u
* Simulate from 0 to 1μs with suggested timestep of 10ns

.TRAN 1n 100n 0 1n uic
* Simulate 100ns, max timestep 1ns, use initial conditions
```

**Documentation:** [LTspice .TRAN Documentation](https://www.analog.com/en/technical-articles/ltspice-transient-analysis.html)

---

## Circuit Definition Commands

### .BACKANNO -- Annotate the Subcircuit Pin Names on Port Currents

Annotates subcircuit pin names on port currents for easier identification in simulation results.

**Syntax:**
```spice
.BACKANNO
```

**Example:**
```spice
.BACKANNO
* Enables annotation of subcircuit pin names on currents
```

**Documentation:** [LTspice .BACKANNO Documentation](https://ltwiki.org/LTspiceHelp/LTspiceHelp/BACKANNO.htm)

---

### .END -- End of Netlist

Marks the end of the netlist. Required in SPICE netlists.

**Syntax:**
```spice
.END
```

**Example:**
```spice
R1 N001 0 1k
C1 N001 0 1u
.TRAN 1m
.END
```

**Documentation:** [LTspice .END Documentation](https://ltwiki.org/LTspiceHelp/LTspiceHelp/END_End_of_Netlist.htm)

---

### .ENDS -- End of Subcircuit Definition

Marks the end of a subcircuit definition.

**Syntax:**
```spice
.ENDS [subcircuit_name]
```

**Example:**
```spice
.SUBCKT opamp inp inn out vdd vss
* ... circuit elements ...
.ENDS opamp
```

**Documentation:** [LTspice .ENDS Documentation](https://ltwiki.org/LTspiceHelp/LTspiceHelp/ENDS_End_of_Subcircuit_Definition.htm)

---

### .GLOBAL -- Declare Global Nodes

Declares nodes as global, making them accessible across all subcircuits.

**Syntax:**
```spice
.GLOBAL <node1> [node2] [node3] ...
```

**Example:**
```spice
.GLOBAL VDD VSS GND
* Makes VDD, VSS, and GND accessible in all subcircuits
```

**Documentation:** [LTspice .GLOBAL Documentation](https://ltwiki.org/LTspiceHelp/LTspiceHelp/GLOBAL_Declare_Global_Nodes.htm)

---

### .MODEL -- Define a SPICE Model

Defines a device model with specified parameters.

**Syntax:**
```spice
.MODEL <model_name> <model_type> ([parameter=value] ...)
```

**Example:**
```spice
.MODEL NMOS_CUSTOM NMOS (VTO=0.7 KP=200u LAMBDA=0.01)
* Define custom NMOS model

.MODEL D1N4148 D (IS=2.52n RS=0.568 N=1.752 CJO=4p)
* Define diode model
```

**Documentation:** [LTspice .MODEL Documentation](https://ltwiki.org/LTspiceHelp/LTspiceHelp/MODEL_Model_Definition.htm)

---

### .SUBCKT -- Define a Subcircuit

Defines a subcircuit that can be instantiated multiple times.

**Syntax:**
```spice
.SUBCKT <name> <pin1> [pin2] [pin3] ...
```

**Example:**
```spice
.SUBCKT voltage_divider in out gnd ratio=2
R1 in out {R*ratio}
R2 out gnd {R}
.ENDS voltage_divider

* Usage:
X1 Vin Vout 0 voltage_divider ratio=3
```

**Documentation:** [LTspice .SUBCKT Documentation](https://ltwiki.org/LTspiceHelp/LTspiceHelp/SUBCKT_Subcircuit_Definition.htm)

---

## Data and Parameter Commands

### .FUNC -- User Defined Functions

Defines user-defined functions for use in expressions.

**Syntax:**
```spice
.FUNC <function_name>([arg1], [arg2], ...) {<expression>}
```

**Example:**
```spice
.FUNC parallel(a, b) {a*b/(a+b)}
R1 n1 n2 {parallel(1k, 2k)}
* R1 = 666.67Ω (parallel combination)

.FUNC db20(x) {20*log10(abs(x))}
* Use in .MEAS: .MEAS AC gain_db FIND db20(V(out)/V(in)) AT 1k
```

**Documentation:** [LTspice .FUNC Documentation](https://ltwiki.org/LTspiceHelp/LTspiceHelp/FUNC_User_Defined_Functions.htm)

---

### .PARAM -- User-Defined Parameters

Defines parameters that can be used throughout the netlist.

**Syntax:**
```spice
.PARAM <name>=<value> [name2=value2] ...
```

**Example:**
```spice
.PARAM Vdd=3.3 Rload=10k Cload=10p
Vpower vdd 0 {Vdd}
Rload out 0 {Rload}
Cload out 0 {Cload}

.PARAM pi=3.14159 fc=1k
.PARAM C={1/(2*pi*fc*10k)}
* Calculate capacitor value for RC filter
```

**Documentation:** [LTspice .PARAM Documentation](https://ltwiki.org/LTspiceHelp/LTspiceHelp/PARAM_Parameter_Definition.htm)

---

### .STEP -- Parameter Sweeps

Sweeps parameters, component values, or temperature across multiple simulation runs.

**Syntax:**
```spice
.STEP PARAM <name> <start> <stop> <increment>
.STEP PARAM <name> LIST <val1> [val2] [val3] ...
.STEP <component> <model> <parameter> <start> <stop> <increment>
```

**Example:**
```spice
.PARAM Rload=1k
.STEP PARAM Rload 100 10k 100
* Sweep Rload from 100Ω to 10kΩ in 100Ω steps

.STEP PARAM Vdd LIST 2.7 3.0 3.3 3.6
* Simulate at 4 supply voltage values

.STEP TEMP -40 125 25
* Temperature sweep from -40°C to 125°C
```

**Documentation:** [LTspice .STEP Documentation](https://ltwiki.org/LTspiceHelp/LTspiceHelp/STEP_Parameter_Sweeps.htm)

---

### .TEMP -- Temperature Sweeps

Sets the simulation temperature or defines a temperature sweep.

**Syntax:**
```spice
.TEMP <temp1> [temp2] [temp3] ...
```

**Example:**
```spice
.TEMP 27
* Simulate at 27°C (default is 27°C)

.TEMP -40 0 27 85 125
* Simulate at 5 different temperatures

* Alternatively, use with .STEP:
.STEP TEMP -40 125 25
```

**Documentation:** [LTspice .TEMP Documentation](https://ltwiki.org/LTspiceHelp/LTspiceHelp/TEMP_Temperature_Sweep.htm)

---

## Initial Conditions and Operating Point Commands

### .IC -- Set Initial Conditions

Sets initial voltage conditions for nodes in transient analysis.

**Syntax:**
```spice
.IC V(<node>)=<value> [V(<node2>)=<value2>] ...
```

**Example:**
```spice
.IC V(out)=2.5 V(n1)=0
* Start transient simulation with out at 2.5V and n1 at 0V

* Use with uic in .TRAN to skip DC operating point calculation:
.TRAN 1n 100n uic
.IC V(cap)=0
```

**Documentation:** [LTspice .IC Documentation](https://ltwiki.org/LTspiceHelp/LTspiceHelp/IC_Set_Initial_Conditions.htm)

---

### .LOADBIAS -- Load a Previously Solved DC Solution

Loads a previously saved DC operating point to use as initial conditions.

**Syntax:**
```spice
.LOADBIAS <filename.txt>
```

**Example:**
```spice
.LOADBIAS bias_point.txt
* Load operating point from file saved with .SAVEBIAS

.TRAN 100n
* Transient analysis starts from loaded bias point
```

**Documentation:** [LTspice .LOADBIAS Documentation](https://ltwiki.org/LTspiceHelp/LTspiceHelp/LOADBIAS.htm)

---

### .NODESET -- Supply Hints for Initial DC Solution

Provides initial guesses for node voltages to help DC convergence (softer than .IC).

**Syntax:**
```spice
.NODESET V(<node>)=<value> [V(<node2>)=<value2>] ...
```

**Example:**
```spice
.NODESET V(out)=1.5 V(gate)=0.7
* Suggest starting values for DC iteration

* Unlike .IC, .NODESET is only a hint - final DC solution may differ
```

**Documentation:** [LTspice .NODESET Documentation](https://ltwiki.org/LTspiceHelp/LTspiceHelp/NODESET_Hints_for_Initial_DC_Solution.htm)

---

### .SAVEBIAS -- Save Operating Point to Disk

Saves the computed DC operating point to a file for later use with .LOADBIAS.

**Syntax:**
```spice
.SAVEBIAS <filename.txt> [internal] [temp] [time=<value>]
```

**Example:**
```spice
.OP
.SAVEBIAS bias_point.txt
* Save DC operating point to file

.TRAN 1u
.SAVEBIAS transient_state.txt time=500n
* Save circuit state at 500ns during transient
```

**Documentation:** [LTspice .SAVEBIAS Documentation](https://ltwiki.org/LTspiceHelp/LTspiceHelp/SAVEBIAS.htm)

---

## Analysis and Measurement Commands

### .FOUR -- Compute a Fourier Component

Computes Fourier components (THD, harmonics) of a signal in transient analysis.

**Syntax:**
```spice
.FOUR <fundamental_freq> <harmonic_count> V(<node>) [I(<device>)]
```

**Example:**
```spice
.TRAN 0 10m 5m
.FOUR 1k 10 V(out)
* Compute fundamental frequency at 1kHz and first 10 harmonics
* Analysis starts after 5ms (to skip startup transient)

.FOUR 100k 5 V(out) I(Rload)
* Analyze both voltage and current
```

**Documentation:** [LTspice .FOUR Documentation](https://ltwiki.org/LTspiceHelp/LTspiceHelp/FOUR_Compute_Fourier_Component.htm)

---

### .MEASURE -- Evaluate User-Defined Electrical Quantities

Measures specific quantities from simulation results (delay, bandwidth, power, etc.).

**Syntax:**
```spice
.MEAS <AC|DC|TRAN|OP> <measurement_name> <measurement_type> <expression> [conditions]
```

**Example:**
```spice
* Transient measurements:
.MEAS TRAN trise TRIG V(out)=0.5 RISE=1 TARG V(out)=2.5 RISE=1
* Measure rise time from 0.5V to 2.5V

.MEAS TRAN avg_power AVG -V(vdd)*I(Vdd) FROM 0 TO 1u
* Measure average power

* AC measurements:
.MEAS AC gain_3db WHEN V(out)/V(in)={gain_dc/sqrt(2)} CROSS=1
.MEAS AC bandwidth PARAM f_3db

* Find max value:
.MEAS TRAN vout_max MAX V(out)
```

**Documentation:** [LTspice .MEAS Documentation](https://ltwiki.org/LTspiceHelp/LTspiceHelp/MEAS_Measure_User_Defined_Electrical_Quantities.htm)

---

### .NET -- Compute Network Parameters in a .AC Analysis

Computes S, Y, Z, H, or G parameters for a network.

**Syntax:**
```spice
.NET V(<port+>) <port_name> V(<port->) [ports...]
```

**Example:**
```spice
.AC dec 10 100k 10G
.NET I(Vport1) port1 I(Vport2) port2
* Compute 2-port S-parameters

* Typically used with:
.NET V(in) input V(out) output
* For amplifier or filter characterization
```

**Documentation:** [LTspice .NET Documentation](https://ltwiki.org/LTspiceHelp/LTspiceHelp/NET_Network_Parameters.htm)

---

## File and Library Commands

### .FERRET -- Download a File Given the URL

Downloads a file from a URL and includes it in the simulation.

**Syntax:**
```spice
.FERRET <URL>
```

**Example:**
```spice
.FERRET http://www.example.com/models/spice_model.lib
* Download and include model file from URL

.FERRET https://www.analog.com/media/en/simulation-models/spice-models/LTC6268.lib
* Include ADI component model directly from web
```

**Documentation:** [LTspice .FERRET Documentation](https://ltwiki.org/LTspiceHelp/LTspiceHelp/FERRET.htm)

---

### .INCLUDE -- Include Another File

Includes another netlist or model file into the current simulation.

**Syntax:**
```spice
.INCLUDE <filename>
```

**Example:**
```spice
.INCLUDE models/nmos_models.lib
* Include local model file

.INCLUDE "C:\Users\Models\opamp.sub"
* Include file with absolute path (use quotes for spaces)

.INCLUDE ../common/passive_models.lib
* Include using relative path
```

**Documentation:** [LTspice .INCLUDE Documentation](https://ltwiki.org/LTspiceHelp/LTspiceHelp/INCLUDE_Include_File.htm)

---

### .LIB -- Include a Library

Includes a library file, optionally selecting a specific section.

**Syntax:**
```spice
.LIB <filename> [section_name]
```

**Example:**
```spice
.LIB standard.mos
* Include entire library

.LIB technology.lib typical
* Include only the "typical" section from library

.LIB C:\CAD\models\vendor.lib fast_corner
* Include specific process corner
```

**Documentation:** [LTspice .LIB Documentation](https://ltwiki.org/LTspiceHelp/LTspiceHelp/LIB_Library_File.htm)

---

## Output and Configuration Commands

### .OPTIONS -- Set Simulator Options

Sets various simulator control options and tolerances.

**Syntax:**
```spice
.OPTIONS [option=value] [option=value] ...
```

**Example:**
```spice
.OPTIONS PLOTWINSIZE=0
* Disable waveform compression

.OPTIONS ABSTOL=1p RELTOL=0.001 VNTOL=1u
* Tighten convergence tolerances

.OPTIONS GMIN=1e-15 ITL1=300 ITL4=100
* Advanced convergence settings

.OPTIONS TEMP=27 TNOM=27
* Set simulation and model temperature

.OPTIONS NUMDGT=8
* Set output precision to 8 digits
```

**Documentation:** [LTspice .OPTIONS Documentation](https://ltwiki.org/LTspiceHelp/LTspiceHelp/OPTIONS_Simulator_Options.htm)

---

### .SAVE -- Limit the Quantity of Saved Data

Specifies which voltages and currents to save, reducing memory usage.

**Syntax:**
```spice
.SAVE <node1> <node2> ... <I(device1)> ...
```

**Example:**
```spice
.SAVE V(out) V(in) I(Rload)
* Save only specified voltages and currents

.SAVE allv
* Save all node voltages (default)

.SAVE alli
* Save all device currents

.SAVE none
* Save nothing (use with .MEAS only)
```

**Documentation:** [LTspice .SAVE Documentation](https://ltwiki.org/LTspiceHelp/LTspiceHelp/SAVE_Limit_Saved_Data.htm)

---

### .WAVE -- Write Selected Nodes to a .Wav File for LTspice

Writes waveform data to a WAV audio file for playback.

**Syntax:**
```spice
.WAVE <filename.wav> <Nbits> <SampleRate> V(<node>) [V(<node2>)]
```

**Example:**
```spice
.TRAN 0 0.1
.WAVE output.wav 16 44.1k V(out)
* Write mono audio at 16-bit, 44.1kHz

.WAVE stereo.wav 24 48k V(left) V(right)
* Write stereo audio at 24-bit, 48kHz

* Nbits: 8, 16, 24, or 32
* SampleRate: typically 8k, 11.025k, 22.05k, 44.1k, 48k, 96k Hz
```

**Documentation:** [LTspice .WAVE Documentation](https://ltwiki.org/LTspiceHelp/LTspiceHelp/WAVE_Write_to_WAV_File.htm)

---

## State Machine Commands

### .MACHINE -- Arbitrary State Machine

Defines an arbitrary state machine for digital control logic.

**Syntax:**
```spice
.MACHINE <name> [initial_state]
+ <state1>: <output_values> -> <next_state> [conditions]
+ <state2>: <output_values> -> <next_state> [conditions]
```

**Example:**
```spice
.MACHINE FSM S0
+ S0: out=0 -> S1 WHEN V(clk)>2.5 & V(enable)>2.5
+ S1: out=3.3 -> S0 WHEN V(clk)<0.5

* Simple toggle state machine with clock and enable
* Outputs 0V in S0, 3.3V in S1
```

**Documentation:** [LTspice .MACHINE Documentation](https://ltwiki.org/LTspiceHelp/LTspiceHelp/MACHINE_Arbitrary_State_Machine.htm)

---

## Additional Resources

- **LTspice Main Help**: [https://ltwiki.org/LTspiceHelp/](https://ltwiki.org/LTspiceHelp/)
- **Analog Devices LTspice**: [https://www.analog.com/en/design-center/design-tools-and-calculators/ltspice-simulator.html](https://www.analog.com/en/design-center/design-tools-and-calculators/ltspice-simulator.html)
- **LTspice Tutorial**: [https://www.analog.com/en/education/education-library/videos/5821806245001.html](https://www.analog.com/en/education/education-library/videos/5821806245001.html)
- **SPICE Reference Guide**: [https://www.analog.com/media/en/simulation-models/spice-models/spice-reference-guide.pdf](https://www.analog.com/media/en/simulation-models/spice-models/spice-reference-guide.pdf)
