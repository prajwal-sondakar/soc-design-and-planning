# Day 5 — Routing, Sign-Off Checks and Final Layout Generation

---

## PART 1 — THEORY

### Signal Routing and Physical Connectivity

Day-5 concentrates on completing the physical connections between all standard cells and preparing the design for final verification. After Clock Tree Synthesis, cells and clock buffers are already positioned, but electrical connections still need to be established. Routing is the stage where metal layers are used to create wires that connect pins according to the gate-level netlist.

Routing occurs in two main phases:

- Global Routing
- Detailed Routing

Global routing estimates paths for interconnections by dividing the layout into routing regions and calculating congestion. Detailed routing assigns exact tracks while respecting design rules such as spacing, metal width, and via placement.

Signal integrity becomes important at this stage. Long wires introduce resistance and capacitance, increasing delay. The routing tool balances path length with congestion avoidance to maintain timing performance.

---

### Design Rule Checking and Layout Verification

Once routing is completed, the design must undergo verification to confirm that it meets fabrication constraints.

- Design Rule Checking (DRC) validates spacing, width, and alignment requirements.
- Layout Versus Schematic (LVS) confirms that the routed layout matches the original logical netlist.

These checks ensure that the design can be manufactured without errors.

---

### Generation of Final Layout Data

After routing and verification, the design is exported into a GDS-II file. This file contains all geometric information required by the foundry, including:

- Cell placements
- Routing layers
- Power structures

The GDS-II file represents the physical outcome of the entire RTL-to-GDS process.

---

## PART 2 — IMPLEMENTATION

### Section 1 — Initiating Routing Stage

The routing stage begins using the DEF produced after Clock Tree Synthesis. The routing engine reads connectivity and placement coordinates to determine optimal signal paths.

Both global routing estimation and detailed routing execution are performed automatically.

---

### Section 2 — Metal Layer Utilization and Wire Optimization

Different metal layers are used for horizontal and vertical routing directions. The tool minimizes wirelength while maintaining spacing rules and inserting vias between layers for connectivity.

The routing structure becomes visible in layout visualization tools.

---

### Section 3 — Design Rule Checking and Verification Preparation

After routing completes, verification tasks confirm adherence to fabrication rules. Designers review DRC and LVS reports to ensure there are no violations.

---

### Section 4 — Final Layout Preparation

The final step prepares the design for export into the GDS-II format. The layout now contains:

- Placed cells
- Clock buffers
- Routed metal layers

Successful reports indicate readiness for final layout generation.

---

## PART 3 — DAY-5 COMMAND TIMELINE + OUTPUT RESULTS

### 1. Entering the OpenLANE Interactive Environment

```bash
cd OpenLane
./flow.tcl -interactive
```

Purpose:

- Launch OpenLANE and load technology configuration.

---

### 2. Preparing the Design

```bash
prep -design picorv32a
```

Purpose:

- Load previous stages including placement and CTS results.

---

```bash
# To include newly added lef to openlane flow merged.lefs
set lefs [glob $::env(DESIGN_DIR)/src/*.lef] add_lefs -src &lefs
# Command to set new value for SYNTH_STRATEGY
set ::env(SYNTH_STRATEGY) 
# Command to set new value for SYNTH_STRATEGY
set ::env(SYNTH_SIZING) 1
# After design is ready now we can run synthesis
run_synthesis
# optional use this or run_floorplan
init_floorplan
place_io
tap_decap_or
# Now we are ready for the placement
run_placement
# optional incase getting error
unset ::env(LIB_CTS)
# After placement now we are ready for the CTS
run_cts
# After the CTS is done now we can do power distribution network
gen_pdn

```

![run synthesis](image-9.png)
---

### screenshots of PDN

![placement stage run successful](<images/day5.md/Screenshot from 2026-02-22 19-27-10.png>) 

![placement stage run successful](<images/day5.md/Screenshot from 2026-02-22 19-28-25.png>) 

![placement stage run successful](<images/day5.md/Screenshot from 2026-02-22 19-30-12.png>) 

![placement stage run successful](<images/day5.md/Screenshot from 2026-02-22 19-30-29.png>) 
```bash
# Change directory to path of generated PDN def
cd /Desktop/worl/tools/openlane_working_dir/openlane
# Command to load the pdn def in magic tool
magic -T /Desktop/worl/tools/openlane_working_dir/openlane/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read 14-pdn.def &
```
![placement stage run successful](<images/day5.md/Screenshot from 2026-02-22 19-35-47.png>) 

![placement stage run successful](<images/day5.md/Screenshot from 2026-02-22 19-36-07.png>) 

![placement stage run successful](<images/day5.md/Screenshot from 2026-02-22 19-37-03.png>) ![placement stage run successful](<images/day5.md/Screenshot from 2026-02-22 19-37-09.png>) 

![placement stage run successful](<images/day5.md/Screenshot from 2026-02-22 19-38-17.png>) 
---
### Prefrom detailed routing using TritonRoute and explore routed layout

```bash
# Check value of current def
echo $::env(CURRENT_DEF)
# Check value of Routing_strategy
echo $::env(ROUTING_STRATEGY)
# route using Triton Route
run_routing

```


---


![Inspecting Routing Logs](<images/day5.md/Screenshot from 2026-02-22 20-46-55.png>) ![Inspecting Routing Logs](<images/day5.md/Screenshot from 2026-02-22 20-47-26.png>) ![Inspecting Routing Logs](<images/day5.md/Screenshot from 2026-02-22 20-50-26.png>)
---

##### command to load routed def in magic 

```bash
# Change directory to path of generated PDN def
cd /Desktop/worl/tools/openlane_working_dir/openlane
# Command to load the pdn def in magic tool
magic -T /Desktop/worl/tools/openlane_working_dir/openlane/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read 14-pdn.def &
```
---

![magic output](<images/day5.md/Screenshot from 2026-02-22 20-54-21.png>) ![magic output](<images/day5.md/Screenshot from 2026-02-22 20-54-40.png>) ![magic output](<images/day5.md/Screenshot from 2026-02-22 20-55-06.png>) ![magic output](<images/day5.md/Screenshot from 2026-02-22 20-55-21.png>)
---

###  Checking Design Rule Violations

```bash
# Change directory
cd Desktop/worl/tools/SPEF_EXTRACTOR

# Command extract spef
python main.py /Desktop/work/tools/openlane_working_dir/desings/picorv32a/runs/22-02_13-44/tmp/merged.lef /Desktop/work/tools/openlane_working_dir/desings/picorv32a/runs/22-02_13-44/results/routing/picorv32a.def
```

Purpose:

- Review DRC or verification results before final export.

![final results](<images/day5.md/Screenshot from 2026-02-22 20-57-23.png>)

 ![final results](<images/day5.md/Screenshot from 2026-02-22 20-58-54.png>) 
 
 ## Post route openSTA timming analysis
 
 ```bash
openroad

read_lef /openLANE_flow/designs/picorv32a/runs/22-02_13-44

read_def /openLANE_flow/designs/picorv32a/runs/22-02_13-44/results/routing/picorv32a.def

write_db pico_route.db

read_verilog /openLANE_flow/designs/picorv32a/runs/22-02_13-44/results/synthesis/picorv32a.synthesis_preroute.v

read_liberty $::env(LIB_SYNTH_COMPLETE)

link_design picorv32a

read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

set_propagated_clock [all_clock]

read_spef /openLANE_flow/designs/picorv32a/runs/22-02_13-44/results/routing/picorv32a.spef

reportt_check -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

exit
 ```
 ![final results](<images/day5.md/Screenshot from 2026-02-22 22-27-51.png>) ![final results](<images/day5.md/Screenshot from 2026-02-22 22-39-52.png>) ![final results](<images/day5.md/Screenshot from 2026-02-22 22-40-36.png>) ![final results](<images/day5.md/Screenshot from 2026-02-22 22-40-51.png>) ![final results](<images/day5.md/Screenshot from 2026-02-22 22-40-56.png>)
---

### 8. Generating Final GDS Layout

```bash
run_magic
```

Purpose:

- Export the final physical layout.

Output:

- `.gds` file representing the complete chip layout.

---

## Day-5 Outputs Generated

- Routed DEF file containing final metal interconnections
- Routing reports describing congestion and layer usage
- Sign-off verification reports
- Final GDS-II layout file
- Layout visualization showing full chip structure

---

## Day-5 Flowchart — CTS to Final Layout

```
CTS DEF + Netlist
        |
[Global Routing]
        |
Detailed Routing
        |
Design Rule Checking
        |
Final Layout Generation (GDS)
```

The routing stage converts placement and clock distribution into a fully connected physical design by creating metal interconnections that satisfy technology rules and timing constraints.