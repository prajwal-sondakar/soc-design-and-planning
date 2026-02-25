# Day 2 — Floorplanning and Physical Design Preparation

---

## PART 1 — THEORY

### Understanding Floorplanning in Physical Design

Day-2 focuses on the transition from logical synthesis to physical implementation through the floorplanning stage. Floorplanning defines the physical dimensions of the chip and establishes how the design will occupy silicon area. Unlike synthesis, which operates on logical structures, floorplanning introduces spatial awareness by assigning boundaries to the design and preparing regions for cell placement.

The chip layout is divided into the die area and the core area. The die represents the total silicon region, while the core contains the digital logic generated during synthesis. Proper definition of these regions is essential because it directly affects routing efficiency, timing performance, and power distribution.

A major concept discussed during this stage is utilization, which represents how much of the available core area is occupied by standard cells. Balanced utilization allows sufficient routing space and avoids congestion. Aspect ratio is another important factor, describing the relationship between core width and height. Designs with balanced proportions typically result in shorter interconnects and improved timing.

---

### Power Planning and Physical Infrastructure

Floorplanning also introduces power distribution planning. To ensure reliable operation, power and ground networks are established across the design. Special structures such as tap cells and decap cells are inserted to maintain substrate stability and reduce noise. These physical support cells do not implement logic but play an important role in maintaining electrical integrity.

Another key theoretical aspect is understanding how library cells influence physical design. During floorplanning, the tool reads LEF files from the standard cell library to determine cell dimensions and pin locations. This information allows the design tool to create placement rows aligned with power rails, ensuring that all cells can be placed in a manufacturable manner.

---

### Good Floorplan Characteristics

A well-defined floorplan ensures efficient placement and routing. Balanced utilization, appropriate aspect ratio, and evenly distributed power structures help minimize congestion and timing issues. Poor floorplanning decisions, such as extremely high utilization or irregular shapes, can lead to routing failures and increased delays later in the design flow.

The theoretical takeaway from this stage is that floorplanning acts as the foundation of physical design. Decisions made at this step strongly influence placement quality, clock tree synthesis, and final routing performance.

---

## PART 2 — IMPLEMENTATION

### Section 1 — Floorplan Initialization and Design Setup

The implementation of Day-2 begins by transitioning from logical synthesis into physical planning. At this stage, the synthesized netlist generated during Day-1 is used as the primary input for defining the physical structure of the chip. The main task performed here is initializing the floorplan environment inside OpenLANE, where the tool calculates the die boundary and core region based on utilization targets and configuration parameters.

The design tool reads the technology LEF and merged LEF files to understand the dimensions of standard cells. Using this information, placement rows are generated across the core region. These rows act as alignment guides that ensure standard cells will later sit correctly on power rails. This step establishes the geometric framework that will support all subsequent physical design stages.

---

### Section 2 — Die Area, Core Area and Utilization Analysis

After initialization, the implementation focuses on defining physical dimensions such as die size, core area, and utilization ratio. These parameters determine how densely the synthesized logic occupies the available silicon space. Designers observe how configuration settings influence the shape and size of the core, as well as how aspect ratio affects routing efficiency.

The objective of this section is to ensure that the design maintains a balanced layout. Excessively high utilization may lead to congestion during placement, while extremely low utilization wastes silicon area.

---

### Section 3 — Power Planning and Tap Cell Insertion

A key task during floorplanning is the preparation of the power infrastructure. The implementation includes automatic insertion of tap cells and other supporting elements required for reliable substrate connection. Tap cells help prevent latch-up issues by maintaining proper electrical biasing within the silicon.

During this stage, the tool also prepares the initial power distribution structure by aligning placement rows with predefined power rails.

---

### Section 4 — Introduction to Library Cells in Physical Context

The implementation then shifts toward understanding how library cells interact with the physical layout. Instead of focusing on logical functionality, this stage examines the physical characteristics of standard cells such as height, width, and pin alignment.

The merged LEF file plays a crucial role here because it combines technology information with cell definitions. By interpreting this data, the floorplanning process ensures that every cell from the synthesized netlist can be placed within the defined rows without violating design rules.

---

### Section 5 — Floorplan Quality Observation: Good vs Bad Layout Preparation

Another important activity during Day-2 implementation is evaluating how floorplan parameters affect design quality. Balanced aspect ratios and moderate utilization levels contribute to efficient layout preparation, while uneven shapes or overly dense configurations could lead to routing congestion in later stages.

---

### Section 6 — Generation of Initial Physical Representation

The final implementation step in Day-2 involves generating the initial DEF representation of the design. This DEF file reflects the first physical view of the processor, including die boundaries, core area, placement rows, and inserted tap cells.

This generated structure becomes the foundation for Day-3 placement.

---

## PART 3 — COMMANDS USED + OUTPUT RESULTS

### Running the Floorplanning Stage

```bash
run_floorplan
```

This command reads the synthesized gate-level netlist along with the technology library information and calculates the physical dimensions of the design. It defines the core boundary, creates placement rows, and inserts initial power planning structures.

---

### Files and Data Generated During Day-2

- Floorplan DEF File  
  A DEF file representing the initial physical layout is generated. It includes die boundaries, core dimensions, power rails, and placement rows.

- Floorplan Reports  
  Reports describing utilization, aspect ratio, and core area are produced.

- Tapcell and Power Planning Logs  
  Log files show the insertion of tap cells and other support structures.

- Updated Run Directory Data  
  New subdirectories related to floorplanning appear inside the run folder.

---

### Important Technical Observations from Day-2

- The `run_floorplan` command reads the synthesized netlist and standard cell LEF files to determine physical dimensions.
- Core area and utilization influence routing congestion and timing performance.
- Tap cells are automatically inserted to maintain substrate stability.
- The generated DEF file represents the first physical view of the design.
- Library cell dimensions define placement rows and alignment with power rails.

---

## Day-2 Flowchart — Synthesis Output to Floorplan

```
Synthesized Netlist
        |
[Read LEF & Technology Data]
        |
Core Area Calculation
        |
Placement Row Generation
        |
Tapcell & Power Structure Insertion
        |
Initial Floorplan DEF + Reports
```

The floorplanning stage converts logical synthesis results into a spatial framework by defining chip dimensions, power infrastructure, and placement rows that prepare the design for placement.