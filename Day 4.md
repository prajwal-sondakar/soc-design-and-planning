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

### 1. Entering the OpenLANE Environment

```bash
cd OpenLane
./flow.tcl -interactive
```

Purpose:

- Start OpenLANE and load Sky130 configuration.

---

### 2. Preparing the Design for CTS

```bash
prep -design picorv32a
```

Purpose:

- Load run directory containing placement results.

---

### 3. Running Clock Tree Synthesis

```bash
run_cts
```

Purpose:

- Build balanced clock distribution network
- Insert buffers and inverters along clock paths

Outputs Generated:

- Updated DEF file with clock buffers
- CTS timing reports
- CTS logs

---

### 4. Viewing CTS Reports

```bash
less runs/picorv32a/reports/cts/*.rpt
```

Observed:

- Clock network statistics
- Buffer count information

---

### 5. Inspecting CTS Logs

```bash
less runs/picorv32a/logs/cts/*.log
```

---

### 6. Opening Updated Layout in Magic

```bash
magic -T sky130A.tech lef read merged.lef def read <cts.def> &
```

Result:

- Magic window displays additional buffer cells along clock paths.

---

### 7. Checking DEF File After CTS

```bash
vim runs/picorv32a/results/cts/<cts.def>
```

Result:

- Additional COMPONENT entries representing clock buffers.

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