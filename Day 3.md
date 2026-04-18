# Day 3 — Placement and Physical Cell Arrangement

---

## PART 1 — THEORY

### Introduction to Placement in ASIC Physical Design

Day-3 focuses on the placement stage of the physical design flow, where standard cells generated during synthesis are arranged within the core area defined during floorplanning. Placement transforms the abstract physical framework into a structured layout by assigning coordinates to each cell based on optimization goals such as wirelength reduction, timing performance, and congestion control.

Placement is typically divided into two phases:

- Global Placement
- Detailed Placement

During global placement, cells are distributed across the core area to achieve balanced density and minimize interconnect length. Detailed placement aligns cells precisely with placement rows while ensuring that design rules are satisfied.

Unlike synthesis, which focuses on logic transformation, placement introduces spatial relationships between cells. The physical distance between elements directly affects signal delay, making placement decisions critical for timing closure.

---

### Global Placement and Density Optimization

Global placement uses algorithms that estimate optimal cell positions without enforcing strict design rule alignment. The main objective is to achieve an even distribution of cells while minimizing overall wirelength. Placement tools consider congestion estimation, timing paths, and power regions during this process.

Density plays an important role during placement. Excessively dense regions can create routing bottlenecks, while sparse regions may lead to inefficient silicon usage.

---

### Detailed Placement and Legalization

After global placement, the design undergoes detailed placement. This phase refines cell positions to ensure that all cells align correctly with placement rows and do not overlap. Legalization corrects violations that may have occurred during global placement.

Detailed placement prepares the design for Clock Tree Synthesis by stabilizing the physical structure and ensuring that sequential elements are positioned appropriately.

---

### Role of Library Cells During Placement

Standard cell libraries continue to play a critical role during placement. The physical dimensions defined in LEF files determine how cells fit within rows and how spacing rules are applied. Flip-flops, buffers, and combinational cells are arranged to balance performance and routing feasibility.

Placement is an optimization process that directly impacts timing, power, and routing complexity.

---

## PART 2 — IMPLEMENTATION

### Section 1 — Initiating Global Placement

The Day-3 implementation begins by executing the placement stage using the floorplan DEF generated in Day-2. The tool distributes synthesized standard cells across the core region by analyzing connectivity and optimization parameters.

---

### Section 2 — Density Control and Congestion Awareness

The placement engine adjusts cell distribution to avoid overcrowded regions that could create routing challenges later. Designers observe how density settings influence the spread of logic across the core area.

---

### Section 3 — Detailed Placement and Legalization

Once global placement is completed, detailed placement refines cell positions by aligning them with predefined placement rows and removing overlaps. Legalization ensures that all standard cells adhere to technology constraints.

---

### Section 4 — Library Cell Interaction During Placement

The placement tool references library LEF data to determine the exact dimensions and orientation of each cell. Designers observe how flip-flops and combinational cells are arranged relative to one another.

---

### Section 5 — Evaluating Placement Quality

Designers evaluate placement results by observing:

- Even cell distribution  
- Minimal overlaps  
- Logical grouping of related modules  

This confirms readiness for the next stage.

---

## PART 3 — COMMANDS USED + OUTPUT RESULTS

### 1. Cloning the Repository and Entering Project Directory

```bash
git clone <repository_link>
cd <repository_folder>
```

Purpose:

- Obtain RTL files, configuration scripts, and OpenLANE setup.

---

### 2. Navigating to OpenLANE Directory

```bash
cd OpenLane
```

---

### 3. Launching OpenLANE Interactive Mode

```bash
./flow.tcl -interactive
```

Result:

```
OpenLane>
```

---

### 4. Preparing the Design

```bash
prep -design picorv32a
```

Output:

```
runs/picorv32a/
 ├── logs/
 ├── reports/
 ├── results/
 └── tmp/
```

---

### 5. Executing Placement Stage

```bash
run_placement
```

Purpose:

1. Perform global placement
2. Perform detailed placement and legalization

Outputs Generated:

- Placement DEF file inside:

```
runs/picorv32a/results/placement/
```

Standard cells now have physical coordinates.

![placement def file](image-8.png)
---

### 6. Opening Layout Using Magic (Visual Verification)

```bash
magic -T sky130A.tech lef read merged.lef def read <placement.def> &
```

Result:

- Standard cells visible inside rows  
- Core boundary displayed  
- Power rails aligned  

![custom inverter](<images/day3.md/Screenshot from 2026-02-15 18-46-59.png>) ![custom inverter](<images/day3.md/Screenshot from 2026-02-15 18-47-35.png>) ![custom inverter](<images/day3.md/Screenshot from 2026-02-15 18-47-52.png>) ![custom inverter](<images/day3.md/Screenshot from 2026-02-15 18-48-10.png>) ![custom inverter](<images/day3.md/Screenshot from 2026-02-15 18-48-28.png>) ![custom inverter](<images/day3.md/Screenshot from 2026-02-15 18-48-38.png>) ![custom inverter](<images/day3.md/Screenshot from 2026-02-15 18-55-34.png>) ![custom inverter](<images/day3.md/Screenshot from 2026-02-17 16-37-08.png>) ![custom inverter](<images/day3.md/Screenshot from 2026-02-17 16-40-47.png>) ![custom inverter](<images/day3.md/Screenshot from 2026-02-17 16-48-44.png>)
---

### 7. Viewing Placement DEF File Using Vim

```bash
vim runs/picorv32a/results/placement/<placement.def>
```

Exit:

```bash
ESC
:q!
```

---

### 8. Checking Placement Reports

```bash
less runs/picorv32a/reports/placement/*.rpt
```
![placement report](<images/day3.md/Screenshot from 2026-02-17 17-35-37.png>) ![placement report](<images/day3.md/Screenshot from 2026-02-17 17-35-42.png>)
Observed:

- Estimated wirelength  
- Density distribution  
- Placement metrics  

---
### 9. analysis through ngspice

![ngspice output](<images/day3.md/Screenshot from 2026-02-18 19-20-30.png>) ![ngspice output](<images/day3.md/Screenshot from 2026-02-18 19-39-42.png>) ![ngspice output](<images/day3.md/Screenshot from 2026-02-18 21-48-26.png>) ![ngspice output](<images/day3.md/Screenshot from 2026-02-18 22-03-42.png>) ![ngspice output](<images/day3.md/Screenshot from 2026-02-18 22-04-27.png>) ![ngspice output](<images/day3.md/Screenshot from 2026-02-18 22-13-48.png>) ![ngspice output](<images/day3.md/Screenshot from 2026-02-18 22-14-35.png>) ![ngspice output](<images/day3.md/Screenshot from 2026-02-18 22-19-52.png>) ![ngspice output](<images/day3.md/Screenshot from 2026-02-18 22-23-44.png>) ![ngspice output](<images/day3.md/Screenshot from 2026-02-18 22-24-01.png>)
---
### 10. Inspecting Placement Logs

```bash
less runs/picorv32a/logs/placement/*.log
```

---

### 11. Opening Library LEF File

```bash
vim merged.lef
```
## 12. opening the pre-designed cells and correcting the error 

![alt text](<images/day3.md/Screenshot from 2026-02-18 22-39-24.png>) ![alt text](<images/day3.md/Screenshot from 2026-02-18 22-39-36.png>) ![alt text](<images/day3.md/Screenshot from 2026-02-18 22-41-52.png>) ![alt text](<images/day3.md/Screenshot from 2026-02-18 22-46-27.png>) ![alt text](<images/day3.md/Screenshot from 2026-02-18 23-04-19.png>) ![alt text](<images/day3.md/Screenshot from 2026-02-18 23-04-57.png>) ![alt text](<images/day3.md/Screenshot from 2026-02-18 23-05-38.png>) ![alt text](<images/day3.md/Screenshot from 2026-02-18 23-08-43.png>) ![alt text](<images/day3.md/Screenshot from 2026-02-18 23-10-19.png>) ![alt text](<images/day3.md/Screenshot from 2026-02-18 23-10-44.png>) ![alt text](<images/day3.md/Screenshot from 2026-02-18 23-57-15.png>) ![alt text](<images/day3.md/Screenshot from 2026-02-18 23-58-34.png>) ![alt text](<images/day3.md/Screenshot from 2026-02-19 17-03-04.png>) ![alt text](<images/day3.md/Screenshot from 2026-02-19 17-03-56.png>) ![alt text](<images/day3.md/Screenshot from 2026-02-19 17-14-17.png>) ![alt text](<images/day3.md/Screenshot from 2026-02-19 17-26-01.png>) ![alt text](<images/day3.md/Screenshot from 2026-02-19 17-30-11.png>) ![alt text](<images/day3.md/Screenshot from 2026-02-19 17-31-44.png>) ![alt text](<images/day3.md/Screenshot from 2026-02-19 17-43-30.png>) ![alt text](<images/day3.md/Screenshot from 2026-02-19 17-52-23.png>)
---

## Day-3 Outputs Generated

1. Placement DEF file containing coordinates of standard cells  
2. Placement reports describing density and optimization  
3. Updated logs documenting placement process  
4. Visual layout view showing placed cells  

---

## Files and Data Generated During Day-3

- Placement DEF File  
- Placement Reports  
- Placement Logs  
- Updated Layout Visualization  

---

## Important Technical Observations from Day-3

- `run_placement` reads floorplan DEF and library data.
- Global placement minimizes wirelength and congestion.
- Detailed placement aligns cells with rows and resolves overlaps.
- Placement DEF shows organized processor structure.
- Proper placement improves timing and prepares for CTS.

---

## Day-3 Flowchart — Floorplan to Placed Design

```
Floorplan DEF + Netlist
        |
[Global Placement Engine]
        |
Density Optimization & Wirelength Estimation
        |
Detailed Placement & Legalization
        |
Final Placement DEF + Reports
```

The placement stage converts the floorplan framework into an organized physical layout by assigning optimized positions to standard cells while maintaining density balance and design rule compliance.

---
### Required Calculation for day-3:
---
#### Rise transition time calculation 
#### Rise time = Time taken for the output to rise to 80% - Time taken for the output to rise to 20%
- 80% = 2.64mV
- 20% = 660mV
- Rise transition time = 2.246 - 2.182 = 0.064 ns = 64 ps

#### Fall transition time = Time taken for the output to fall to 20% - Time taken for the output to fall to 80 %
- 20%=660mV
- 80%=2.64mV
- Fall transition time = 4.0955 - 4.0536 = 0.0419 ns = 41.9 ps

#### Rise cell delay= Time taken for the output to rise to 50% - Time taken for the input to rise to 50 %
- 50% of 3.3V = 1.65V
- Rise cell delay= 2.21144 - 2.15008 = 0.06136ns = 61.36 ps

#### Fall cell delay= Time taken for the output to fall to 50% - Time taken for the input to fall to 50%
- 50% of 3.3V = 1.65V
- Fall cell delay= 4.07 - 4.05 = 0.02ns = 20 ps
---
