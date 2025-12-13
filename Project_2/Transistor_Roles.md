# Transistor Roles in Folded Cascode Op-Amp

This document details the specific roles of every transistor in the `FOLDED_CASCODE_OPAMP.cir` netlist.

## 🔍 Discrepancy Note: Textbook vs. Netlist
*   **Textbook (Core Only):** Typically shows **7 NMOS** and **4 PMOS** (Total 11).
*   **Netlist (Simulation Ready):** Includes **3 extra NMOS** and **2 extra PMOS** for the **Bias Circuits**.
    *   *Real circuits need these extra transistors to generate the bias voltages ($V_b$) that are often shown as ideal batteries in textbooks.*

---

## 1. The Core Amplifier (Matches Textbook Figure)
These are the transistors that actually process the signal.

### NMOS Transistors (7 Total)
1.  **M1**: Input Pair (Non-inverting).
2.  **M2**: Input Pair (Inverting).
3.  **M11**: Tail Current Source (Sets $I_{SS}$).
4.  **M7**: Output Cascode (Shields the output).
5.  **M8**: Load Cascode (Matches M7 on the left).
6.  **M9**: Output Current Mirror (Bottom).
7.  **M10**: Load Current Mirror (Bottom - Diode Connected).

### PMOS Transistors (4 Total)
1.  **M3**: Top Current Source (Left).
2.  **M4**: Top Current Source (Right).
3.  **M5**: Folding Cascode (Right - Output side).
4.  **M6**: Folding Cascode (Left).

---

## 2. The Bias Circuits (Auxiliary)
These transistors do **not** amplify signals. They just turn on the other transistors.

### NMOS Bias Transistors (3 Total)
1.  **Mb1**: Generates bias for the Tail Source (M11).
2.  **Mb3_sink**: Part of the bias stack for the bottom mirror.
3.  **Mb3_casc**: Generates the bias voltage `node_b3` for the NMOS cascodes (M7, M8).

### PMOS Bias Transistors (2 Total)
1.  **Mb2_src**: Part of the bias stack for the top PMOS.
2.  **Mb2_casc**: Generates the bias voltage `node_b2` for the PMOS cascodes (M5, M6).

---

## Summary Table

| Section | NMOS Count | PMOS Count | Role |
| :--- | :---: | :---: | :--- |
| **Core Amp** | **7** | **4** | Signal Amplification |
| **Bias Circuit** | **3** | **2** | DC Voltage Generation |
| **TOTAL** | **10** | **6** | Full Simulation Circuit |
