# Day 1 — Inception of Open-Source EDA, OpenLANE and Sky130 PDK

---

## PART 1 — THEORY

### Overview of Chip Packaging and Design Fundamentals

The first part of Day-1 focused on understanding how a digital integrated circuit is physically organized inside a semiconductor package. A QFN-48 package was introduced to explain how the silicon die connects to external pins through bonding structures. The internal architecture of a chip includes several essential components that work together to enable signal transmission and processing.

Pads form the interface between the external environment and the internal circuitry of the chip. These metal structures carry input signals, output signals, power, and ground connections. Inside the chip boundary lies the core region, which contains the digital logic responsible for executing operations. The core is fabricated on a silicon die, which is the physical substrate manufactured using semiconductor processing techniques.

The session also introduced the concept of foundry IPs and digital macros. Foundry IPs are pre-validated blocks supplied by the fabrication process, including modules such as PLLs, SRAMs, and ADCs. Macros are reusable digital blocks built using standard cells, commonly used for interfaces like GPIO or SPI. Both IPs and macros help accelerate design development by providing reliable, pre-designed components.

---

### Introduction to OpenLANE and the RTL-to-GDSII Flow

OpenLANE was presented as a fully automated open-source ASIC design flow that integrates multiple tools into a single environment. Instead of relying on proprietary EDA software, OpenLANE combines Yosys for synthesis, OpenROAD for physical design, Magic for layout visualization, and TritonRoute for routing.

The RTL-to-GDSII flow begins with Verilog RTL, which describes the logical behaviour of the circuit. During synthesis, this RTL is translated into a gate-level netlist using standard cells from the Sky130 library. Floorplanning defines the physical dimensions of the chip and establishes the power network. Placement arranges cells inside the core area, while Clock Tree Synthesis distributes clock signals evenly to sequential elements. Routing connects the cells using metal layers, and finally the design is exported as a GDS-II layout ready for fabrication.

---

### Synthesis Basics and Flop Ratio

A major theoretical topic covered during Day-1 was the synthesis process. RTL descriptions written in Verilog are abstract and cannot be fabricated directly. Synthesis tools analyze the logic and map it into standard cells defined by the technology library.

One important metric discussed was the flop ratio, which represents the proportion of sequential elements relative to the total number of cells in the design. This metric helps designers estimate how much of the circuit is controlled by registers versus combinational logic. A balanced flop ratio often indicates a well-structured processor pipeline, while extreme values may signal timing or design complexity challenges.

---

### Standard Library Cells

Standard library cells are pre-designed building blocks used to implement digital logic on silicon. These cells include gates such as NAND, NOR, and inverters, as well as sequential elements like flip-flops. Each cell is defined by multiple library files:

* `.lib` → timing and power information
* `.lef` → physical dimensions and pin locations
* `.gds` → layout geometry used during fabrication

Understanding these libraries is essential because synthesis and floorplanning rely on them to determine how logic is mapped and how much physical space each cell occupies.

---

### Introduction to picorv32a Processor Core

The picorv32a RISC-V processor core was selected as the reference design for this lab. This lightweight processor provides a realistic digital architecture while remaining manageable within an open-source flow. It includes instruction decoding, arithmetic operations, and register storage, allowing designers to observe how a complete processor behaves during synthesis. Using a processor design instead of a small example circuit helps demonstrate how synthesis reports reflect real architectural complexity.

---

## PART 2 — IMPLEMENTATION

### SECTION 1 : Introduction of Open-Source EDA Tools, OpenLANE and SKY130 PDK

This section marks the beginning of the practical workflow. The main task here is to initialize the OpenLANE environment and prepare the picorv32a design for execution. The implementation involves launching the interactive flow, loading configuration files, and linking the Sky130 PDK libraries with the design sources. A dedicated run directory is created to store logs, reports, and generated files for this specific design instance.

The purpose of this stage is to ensure that all necessary inputs are correctly configured before moving into synthesis. By establishing the design environment, OpenLANE prepares the framework required for subsequent steps in the ASIC design flow.

---

### SECTION 2 : Good Floorplan vs Bad Floorplan and Preface to Library Cells

In this section, the focus shifts toward understanding how synthesis results influence physical design preparation. The tasks involve examining how utilization targets, aspect ratio, and the dimensions of standard cells affect the way a design will later be placed and routed. Instead of producing a final layout, this stage concentrates on preparing the design and recognizing how library cell characteristics contribute to efficient planning.

The workflow also introduces the idea of evaluating floorplan quality by comparing balanced layouts with inefficient ones. Observing how cell distribution and library parameters impact area usage helps designers anticipate potential congestion issues during placement and routing.

---

## PART 3 — COMMANDS USED + OUTPUT RESULTS

### 1. Cloning the Repository and Entering the Project Directory

```bash
git clone <repository_link>
cd <repository_folder>
```

**Purpose**

* Download RTL sources, configuration files, and OpenLANE setup.

**Result Generated**

* Local project directory created with design files.

---

### 2. Navigating to OpenLANE Environment

```bash
cd OpenLane
```

**Purpose**

* Enter the OpenLANE workspace where the RTL-to-GDS flow is executed.

---

### 3. Launching OpenLANE Interactive Mode

```bash
./flow.tcl -interactive
```

**Purpose**

* Start OpenLANE in interactive mode to manually control synthesis.

**Result**

```
OpenLane>
```

Environment variables and Sky130 PDK paths are loaded.

---

### 4. Preparing the picorv32a Design

```bash
prep -design picorv32a
```

**Purpose**

* Load RTL files
* Link Sky130 library files
* Initialize run directory

**Outputs Generated**

```
runs/picorv32a/
 ├── logs/
 ├── reports/
 ├── results/
 └── tmp/
```

---

### 5. Running Synthesis

```bash
run_synthesis
```

**Purpose**

* Convert Verilog RTL into gate-level netlist using Yosys and ABC.
* Optimize logic and perform timing-aware mapping.

**Outputs Generated**

* Gate-level netlist
* Timing reports
* Cell statistics report

---

### 6. Opening Synthesized Netlist Using Vim

```bash
vim runs/picorv32a/results/synthesis/*.v
```

Example cells observed:

```
sky130_fd_sc_hd__dfxtp
sky130_fd_sc_hd__nand2
```

Exit Vim:

```bash
ESC
:q!
```

---

### 7. Viewing Synthesis Reports

```bash
less runs/picorv32a/reports/synthesis/*.rpt
```

Observed:

* Arrival time
* Required time
* Slack information

---

### 8. Checking Cell Statistics (Flop Ratio Observation)

```bash
less runs/picorv32a/reports/synthesis/*stat.rpt
```

---

### 9. Inspecting Synthesis Logs

```bash
less runs/picorv32a/logs/synthesis/*.log
```

---

### 10. Opening Library LEF File

```bash
vim merged.lef
```

---

### 11. Opening DEF/Design View in Magic

```bash
magic -T sky130A.tech lef read merged.lef &
```

---

## Day-1 — Generated Outputs and Important Observations

### Files and Data Generated During Day-1

* Run directory structure created
* Gate-level netlist generated
* Timing analysis reports created
* Cell statistics reports produced
* Synthesis log files generated

---

### Important Technical Points Learned in Day-1

* `prep -design` initializes the design environment.
* `run_synthesis` invokes Yosys and ABC mapping.
* Behavioral RTL is converted into standard cell gate instances.
* Timing reports reveal the longest delay path.
* Cell statistics help estimate flop ratio.
* LEF and LIB files define electrical and physical properties.

---

## Day-1 : Flowchart — RTL TO SYNTHESIZED NETLIST

```
Verilog RTL
     |
[Yosys Parsing]
     |
Hierarchy Analysis & Logic Optimization
     |
[ABC Technology Mapping]
     |
Standard Cell Gate-Level Netlist
     |
Timing Analysis & Optimization
     |
Final Netlist + Reports Generated
```

The synthesis stage transforms the abstract RTL description into a technology-mapped hardware representation by optimizing logic and applying Sky130 standard cell constraints.
