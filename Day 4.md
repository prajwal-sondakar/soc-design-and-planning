# Day 4 — Clock Tree Synthesis and Routing Preparation

---

## PART 1 — THEORY

### Clock Distribution and the Need for Clock Tree Synthesis

After placement, the design contains thousands of sequential elements such as flip-flops that must receive the clock signal simultaneously. If the clock reaches different parts of the design at different times, it can lead to setup and hold violations. To avoid this, a structured clock network is created during the Clock Tree Synthesis (CTS) stage.

Clock Tree Synthesis builds a hierarchical network of buffers and inverters that distribute the clock signal evenly across the core. Instead of a single long wire, the clock is propagated through a balanced tree structure, which reduces skew and improves timing reliability. The CTS engine considers placement information, cell locations, and timing constraints while constructing this network.

Clock skew refers to the difference in arrival time of the clock signal at various flip-flops. Minimizing skew ensures predictable timing behavior across the design. CTS also controls clock latency, which is the delay introduced by the clock network itself.

---

### Routing Concepts and Signal Interconnection

Once cells and clock buffers are positioned, the next step is preparing the design for routing. Routing creates physical metal connections between cells based on the netlist connectivity. The process uses multiple metal layers, each assigned specific routing directions to reduce interference and congestion.

Routing is divided into:

- Global Routing
- Detailed Routing

Global routing estimates paths for wires across the design, while detailed routing assigns exact tracks that satisfy design rules. Design Rule Checks (DRC) ensure that spacing, width, and overlap constraints defined by the technology library are maintained.

---

### Role of Library Cells During CTS

During Clock Tree Synthesis, the tool uses specific buffer and inverter cells from the standard cell library to build the clock network. These cells are chosen based on drive strength and timing requirements. Library timing data helps determine where buffers should be inserted to maintain balanced clock distribution.

CTS is therefore both a logical and physical optimization process guided by library characteristics.

---

## PART 2 — IMPLEMENTATION

### Section 1 — Initiating Clock Tree Synthesis

The implementation begins by executing the CTS stage using the placement DEF generated in Day-3. The CTS engine analyzes flip-flop locations and determines optimal buffer insertion points to maintain uniform clock arrival times.

---

### Section 2 — Clock Buffer Insertion and Timing Preparation

Clock buffers are inserted automatically based on timing requirements. Designers observe additional standard cells appearing in the layout, representing the clock distribution network.

This process prepares the design for accurate timing analysis by stabilizing clock paths and minimizing skew.

---

### Section 3 — Preparing for Routing

After CTS, the implementation focuses on verifying integration between placement and clock structures. Designers observe routing channels and ensure that clock buffers align with placement rows.

Although detailed routing occurs later, Day-4 prepares the framework required for routing.

---

### Section 4 — Evaluating CTS Quality

Designers review timing reports to observe:

- Clock latency
- Clock skew
- Buffer distribution

Balanced values indicate readiness for routing and sign-off checks.

---

## PART 3 — DAY-4 COMMAND TIMELINE + OUTPUT RESULTS

```bash 
cd Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign

magic -T sky130A.tech sky130_inv.mag &
```
---
![alt text](<images/day4.md/Screenshot from 2026-02-19 18-35-17.png>) ![alt text](<images/day4.md/Screenshot from 2026-02-19 18-36-50.png>)
---

```bash
# Get syntax for grid command
held grid
# Set grid values aaccordingly
grid 0.46um 0.34um 0.23um 0.17um
```
#### Condition 1 verified
![magic](<images/day4.md/Screenshot from 2026-02-19 22-14-52.png>)
 #### Condition 2 verified
 ##### horizontal\track\pitch=0.46\um
 ![](<images/day4.md/Screenshot from 2026-02-19 22-34-48.png>)
#### condition 3 verified
##### Vertical\track\pitch =0.34\um
 ![ magic  ](<images/day4.md/Screenshot from 2026-02-19 22-36-36.png>)

![magic](<images/day4.md/Screenshot from 2026-02-19 22-37-40.png>)
```bash
# lef write
lef write
```
 ![alt text](<images/day4.md/Screenshot from 2026-02-19 22-50-38.png>)
 ##### screenshot of newly created lef file
  ![alt text](<images/day4.md/Screenshot from 2026-02-19 22-57-05.png>) 
  ```bash
# copy lef file
cp sky130_vsdinv.lef ~/Desktop/worl/tools/openlane_working_dir/openlane/design/picorv32a/src/
# List and check whether its copied
ls ~/Desktop/worl/tools/openlane_working_dir/openlane/design/picorv32a/src/
# Copy lib files
cp libs/sky130_fd_sc_hd__* ~/Desktop/worl/tools/openlane_working_dir/openlane/design/picorv32a/src/
# Check
ls /Desktop/worl/tools/openlane_working_dir/openlane/design/picorv32a/src/
  ```
  ![alt text](<images/day4.md/Screenshot from 2026-02-19 23-08-02.png>)
#### edit "config.tcl" to change lib file and add the new extra lef into the openlane flow.

##### Commands to be added to config.tcl to include our custom  cell in the openlane flow
```bash
set ::env(LIB_SYNTH) "$::env(OPENLANE_ROOT)/design/picorv32a/src/sky130_fd_sc_hd__typical.lib"

set ::env(LIB_FASTEST) "$::env(OPENLANE_ROOT)/design/picorv32a/src/sky130_fd_sc_hd__fast.lib"

set ::env(LIB_SLOWEST) "$::env(OPENLANE_ROOT)/design/picorv32a/src/sky130_fd_sc_hd__slow.lib"

set ::env(LIB_TYPICAL) "$::env(OPENLANE_ROOT)/design/picorv32a/src/sky130_fd_sc_hd__typical.lib"
```
##### Edited config.tcl 
![alt text](<images/day4.md/Screenshot from 2026-02-19 23-42-23.png>) 

![magic](<images/day4.md/Screenshot from 2026-02-20 00-02-18.png>) 
```bash
# Entered in the project 
cd /Desktop/work/tools/openlane_working_dir/openlane
Docker
# Openalne in interactive mode
./flow.tcl -interactive
# input all the require packages for the design
package require openlane 0.9
# Now our design is ready for synthesis
prep -design picorv32a
# Additional commands to include newly added lef 
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
# Now design is ready ,so run the synthesis using the following command
run_synthesis
```

![alt text](<images/day4.md/Screenshot from 2026-02-20 17-15-46.png>)

![alt text](<images/day4.md/Screenshot from 2026-02-20 17-29-21.png>) 

![alt text](<images/day4.md/Screenshot from 2026-02-20 17-29-43.png>) 
```bash
prep -design picorv32a -tag 22-02_13-44 -overwrite

set lefs [glob $::env(DESIGN_DIR)/src/*.lef]

echo $::env(SYNTH_STRATEGY)

set $::env(SYNTH_STRATEGY)

echo $::env(SYNTH_BUFFERING)

echo $::env(SYNTH_SIZING)

set $::env(SYNTH_SIZING) 1

echo $::env(SYNTH_DRIVING_CELL)

run_synthesis
```
![alt text](<images/day4.md/Screenshot from 2026-02-20 17-33-51.png>)
   
 ![alt text](<images/day4.md/Screenshot from 2026-02-20 17-52-09.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-20 17-52-28.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-20 17-53-18.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-20 17-54-25.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-20 17-54-45.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-20 17-55-33.png>) 
 ```bash
# Now we can run floorplan
run_floorplan
 ```
 ![alt text](<images/day4.md/Screenshot from 2026-02-20 18-18-13.png>) 
 ##### If we face error while using run_floorplan then use these following commands 
 ```bash
init_floorplan
place_io
tap_decap_or
 ```
 ![alt text](<images/day4.md/Screenshot from 2026-02-20 18-18-27.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-20 18-19-28.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-20 18-19-44.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-20 18-20-54.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-20 18-21-28.png>) 
 #### Change directory to path containing placement def 
 ```bash
cd /Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/22-02_13-44/results/placement/
# command in the magic tool
magic -T /Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
 ```
 ![alt text](<images/day4.md/Screenshot from 2026-02-20 18-44-27.png>) 
 

 ![alt text](<images/day4.md/Screenshot from 2026-02-20 19-28-39.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-20 19-30-11.png>) 
 ```bash
 # command to view internal connectivity layers
 expand
 ```

 ![alt text](<images/day4.md/Screenshot from 2026-02-20 19-30-42.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-20 19-45-45.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-20 19-46-37.png>) 
 
 #### Newly created "my_base.sdc"
 ![alt text](<images/day4.md/Screenshot from 2026-02-21 18-20-03.png>) 
 #### Newly created pre_sta.conf
 ![alt text](<images/day4.md/Screenshot from 2026-02-21 18-21-14.png>) 
 ```bash
# change directory to openlane 
cd Desktop/work/tools/openlane_working_dir/openlane
# invoke openSTA tool with script
sta pre_sta.conf
 ```
 ![alt text](<images/day4.md/Screenshot from 2026-02-21 19-40-11.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-21 19-40-40.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-21 19-41-21.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-21 19-41-30.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-21 19-41-41.png>) 

 ![alt text](<images/day4.md/Screenshot from 2026-02-21 20-36-06.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-21 21-09-33.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-21 21-17-03.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-21 21-18-47.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-21 21-23-14.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-21 22-52-02.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-21 22-53-05.png>) 
  ```bash
# Report_net -connection to a net
report_net -connections _11672_
# Checking command syntax
help replace_cell
# Replace cell
replace_cell _14510_ sky130_fd_sc_hd__or3_4
# Genrating custom timinf report
report_checks -fields {net cap slew input_pins} -digits 4

 ```
 ![alt text](<images/day4.md/Screenshot from 2026-02-21 22-53-19.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-21 22-53-25.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-21 22-53-31.png>) 

 ![alt text](<images/day4.md/Screenshot from 2026-02-21 23-09-36.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-21 23-09-48.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-21 23-10-25.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-21 23-10-51.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-21 23-12-00.png>) 
  ```bash
# Report_net -connection to a net
report_net -connections _11675_
# Checking command syntax
help replace_cell
# Replace cell
replace_cell _14514_ sky130_fd_sc_hd__or3_4
# Genrating custom timinf report
report_checks -fields {net cap slew input_pins} -digits 4

 ```
 ![alt text](<images/day4.md/Screenshot from 2026-02-21 23-18-42.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-21 23-19-37.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-21 23-19-59.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-21 23-25-31.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-21 23-38-46.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-21 23-39-34.png>) 
  ```bash
# Report_net -connection to a net
report_net -connections _11643_
# Checking command syntax
help replace_cell
# Replace cell
replace_cell _14481_ sky130_fd_sc_hd__or4_4
# Genrating custom timinf report
report_checks -fields {net cap slew input_pins} -digits 4

 ```
 ![alt text](<images/day4.md/Screenshot from 2026-02-21 23-55-47.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-21 23-56-03.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-21 23-58-48.png>) 
  ```bash
# change directory to synthesis results directory
cd Desktop/work/tools/openlane_working_dir/openlane/design/picorv32a/22-02_13-44/synthesis/
# list contents of the current directory
ls
# copy and rename the netlist
cp picorv32a.synthesis.v picorv32a.synthesis_old.v
#list
ls

 ```
 ![alt text](<images/day4.md/Screenshot from 2026-02-22 00-18-22.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-22 00-40-43.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-22 00-43-40.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-22 00-44-08.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-22 00-44-46.png>) 
 ```bash
prep -design picorv32a -tag 22-02_13-44 -overwrite

set lefs [glob $::env(DESIGN_DIR)/src/*.lef] add_lefs -src &lefs

set ::env(SYNTH_STRATEGY) 

set ::env(SYNTH_SIZING) 1

run_synthesis

init_floorplan
place_io
tap_decap_or

run_placement

unset ::env(LIB_CTS)

run_cts
 ```
 ![alt text](<images/day4.md/Screenshot from 2026-02-22 12-01-01.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-22 12-22-13.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-22 12-25-30.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-22 12-33-42.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-22 13-00-01.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-22 13-00-57.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-22 13-01-28.png>) 
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-22 13-02-22.png>)
 
![alt text](<images/day4.md/Screenshot from 2026-02-22 13-04-06.png>)
  
![alt text](<images/day4.md/Screenshot from 2026-02-22 13-29-49.png>)
   
![alt text](<images/day4.md/Screenshot from 2026-02-22 13-29-53.png>)
    
![alt text](<images/day4.md/Screenshot from 2026-02-22 13-47-09.png>)
     
![alt text](<images/day4.md/Screenshot from 2026-02-22 13-53-30.png>)
     
![alt text](<images/day4.md/Screenshot from 2026-02-22 13-53-44.png>)
 
![alt text](<images/day4.md/Screenshot from 2026-02-22 13-53-48.png>)
  
![alt text](<images/day4.md/Screenshot from 2026-02-22 13-53-51.png>)
```bash
echo $::env(CTS_CLK_BUFFER_LIST)

set ::env(CTS_CLK_BUFFER_LIST) [lreplace $::env(CTS_CLK_BUFFER_LIST)0 0]

echo $::env(CURRENT_DEF)

set ::env(CURRENT_DEF) /openLANE_flow/designs/picorv32a/runs/22-02_13-44/results/plaement/picorv32a.placement.def

run_ctc

echo $::env(CTS_CLK_BUFFER_LIST)

openroad

read_lef /openLANE_flow/designs/picorv32a/runs/22-02_13-44/tmp/merged.lef

read_def /openLANE_flow/designs/picorv32a/runs/22-02_13-44/results/cts/picorv32a.cts.def

write_db pico_cts1.db

read_db pico_cts.db

read_verilog /openLANE_flow/designs/picorv32a/runs/22-02_13-44/results/synthesis/picorv32a.synthesis_cts.v

```
![alt text](<images/day4.md/Screenshot from 2026-02-22 14-10-11.png>)
    
![alt text](<images/day4.md/Screenshot from 2026-02-22 14-12-53.png>)
 
![alt text](<images/day4.md/Screenshot from 2026-02-22 14-14-21.png>)
  
![alt text](<images/day4.md/Screenshot from 2026-02-22 14-23-00.png>)
 
![alt text](<images/day4.md/Screenshot from 2026-02-22 14-23-05.png>)
 
![alt text](<images/day4.md/Screenshot from 2026-02-22 14-23-13.png>)
  
![alt text](<images/day4.md/Screenshot from 2026-02-22 14-23-32.png>)
 
 ![alt text](<images/day4.md/Screenshot from 2026-02-22 14-23-41.png>)
 
![alt text](<images/day4.md/Screenshot from 2026-02-22 14-24-36.png>)
 
![magic](<images/day4.md/Screenshot from 2026-02-22 14-26-17.png>)
---

## Day-4 Outputs Generated

- CTS DEF file with inserted clock buffers
- Clock timing reports showing skew and latency
- Updated layout view displaying clock network structure
- CTS logs documenting optimization

---

## Day-4 Flowchart — Placement to Clock Tree

```
Placed DEF + Netlist
        |
[Clock Tree Synthesis Engine]
        |
Clock Buffer & Inverter Insertion
        |
Clock Skew Optimization
        |
Updated DEF + CTS Reports
```

Clock Tree Synthesis transforms a single clock net into a balanced distribution network by inserting library buffer cells to reduce skew and maintain timing stability.