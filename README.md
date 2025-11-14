# **README – 1T1R ReRAM Circuit Design, Simulation & Analysis**  
### *(Simulation Results Report)*

## **📌 Project Overview**
This project implements and analyzes a **1T1R ReRAM (Resistive RAM) memory cell** using **NGSpice**, **Cadence Virtuoso**, and a **custom Verilog-A RRAM model**.  
The objective is to verify **SET/RESET switching**, **read operations**, **current behavior**, **resistance states**, and **sense amplifier performance**.

This README summarizes the full methodology, netlist, waveforms, layout steps, and simulation results reported in the project.

---
## **Tools Used**
- **NGSpice** for netlist-based simulation of the 1T1R RRAM circuit.  
- **Cadence Virtuoso** for schematic design and sense amplifier simulation.  
- **Microwind** for layout design of the 1T1R cell and 2×2 RRAM array.

## **📁 Contents**
- Introduction  
- 1T1R ReRAM Cell Architecture  
- NGSpice Verilog-A Model  
- Simulation Waveforms & Interpretation  
- Sense Amplifier Design & Simulations  
- Layout-Level SET/RESET Experiments  
- 2×2 ReRAM Array Design  
- Final Results Summary  

---

# **1. Introduction**
This study compares:  
- **Real RRAM devices** → very low resistances (4–14 Ω LRS, 30–70 Ω HRS) & high currents (20–100 mA).  
- **Simulated model** → higher resistances (1 kΩ LRS, 100 kΩ HRS) suitable for array-level low-power simulations.

The simulations focus on:
- Switching operations  
- Current tracking  
- State retention  
- Readout sensing margins  
- Sense amplifier correctness  

This report contains **all simulation results** used to verify functionality.

---

# **2. 1T1R ReRAM Cell**
### **Architecture**
A **1 Transistor + 1 RRAM cell** configuration ensures:
- Selective activation  
- No sneak-path currents  
- Reliable read/write operations  

### **Operating Timeline**
| Time (µs) | BL Voltage | Operation | Expected Result |
|----------|------------|-----------|-----------------|
| 1.1–2.0 | +0.2 V | READ HRS | Low current |
| 3.1–4.0 | +1.5 V | SET | Switch to LRS |
| 5.1–6.0 | +0.2 V | READ LRS | High current |
| 7.1–8.0 | –1.5 V | RESET | Switch to HRS |
| 9.1–10 | +0.2 V | READ HRS | Low current |

---

# **3. Verilog-A Model (Stanford RRAM Based)**

A stable analog-friendly model is used, featuring:

- SET and RESET trigger detectors  
- SR-latch for storing resistance state  
- Smooth tanh-based NOR gates  
- Controlled resistor emulating resistive switching  

This ensures **numerical stability** in NGSpice simulations.

---

# **4. NGSpice Simulation Results**
The report includes three key plots:

### ✔ **1. RRAM Current vs Time**
Shows:
- Low current in HRS  
- High current in LRS  
- Switching spikes during SET/RESET  

### ✔ **2. RRAM Resistance (log scale)**
Demonstrates:
- Clear transitions between HRS ↔ LRS  
- Strong binary behavior  

### ✔ **3. BL & WL Input Voltages**
Verifies proper stimulus and timing.

---

# **5. Latch-Type Sense Amplifier**
A transistor-level sense amplifier is designed to:
- Compare cell voltage vs reference  
- Amplify small differences  
- Correctly sense HRS/LRS states  

Reference resistors and selection transistors allow multiple sensing thresholds.

### **Note**
RRAM is **nonlinear & state-dependent**, so static resistor values representing HRS/LRS were manually applied to test amplifier behavior.

---

# **6. Sense Amplifier Simulation Results**

### **Case 1: BL = 0.85 V, BLB = 0 V**
- Precharge: 0–5 ns  
- Evaluation: 5–10 ns  
- Correct latch operation observed  

### **Case 2: BL = 0.85 V, BLB = 0.80 V**
- Increased transistor width improves sensitivity  
- Observed changes in latching behavior  

---

# **7. Layout-Level SET/RESET/READ Experiments**

Because actual RRAM physics cannot be modeled directly in Virtuoso, the resistance was **manually modified** to represent HRS/LRS.

## **SET Operation**
- WL = 1.2 V  
- BL = 1.5 V  
- High current spike indicates switching  

## **READ ‘1’ (LRS)**
- WL = 1.2 V  
- BL = 0.2 V  
- Expected: `ILRS = 0.2V / 1kΩ = 0.2 mA`  

## **RESET Operation**
- WL = 1.2 V  
- BL = 0.8 V  

## **READ ‘0’ (HRS)**
- WL = 1.2 V  
- BL = 0.2 V  
- Expected: `IHRS = 0.2V / 100kΩ = 2 µA`  

---

# **8. Measured Currents (From Layout Simulations)**

| Operation | Measured Current | Interpretation |
|-----------|------------------|----------------|
| SET | 15 µA | Programming current |
| READ ‘1’ | 0.177 mA | LRS State ✔ |
| RESET | 0.15 mA | Reset current |
| READ ‘0’ | 2 µA | HRS State ✔ |

---

# **9. Layout Output Waveforms**

### **Read ‘0’ (HRS)**
- Very low current  
- ~0.305 µW power consumption  
- Appears near-zero in plot due to mA-scale axis  

### **SET Current**
- Strong switching pulse  

### **RESET Current**
- Moderate current pulse  

### **Read ‘1’ (LRS)**
- ~0.18 mA current, confirming low-resistance state  

---

# **10. 2×2 ReRAM Array Layout**
The four cells were configured manually:

| Position | State |
|----------|--------|
| Top-Left | LRS |
| Top-Right | HRS |
| Bottom-Left | HRS |
| Bottom-Right | LRS |

### **Example Read (Bottom-Left Cell)**
- Measured: **0.25 µA**  
- Matches HRS behavior  

---

# **11. Conclusion**
This simulation report successfully demonstrates:

- Correct behavior of a **1T1R ReRAM cell**  
- Successful SET/RESET switching  
- Accurate distinction between LRS and HRS through current  
- Functional latch-type sense amplifier  
- Verified read operations using both circuit and layout simulations  
- Complete working 2×2 ReRAM array with identifiable cell states  

The analysis aligns with realistic RRAM operation while remaining stable for analog simulation tools.

---
