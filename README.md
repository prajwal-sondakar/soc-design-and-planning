# soc-design-and-planning

# RTL-to-GDSII SoC Design Flow using OpenLANE & SKY130

---

##  Overview

This repository documents my complete learning journey through the open-source RTL-to-GDSII SoC design flow using **OpenLANE** and the **Sky130 PDK**.  
The objective of this project is to understand how a digital processor design moves from a high-level Verilog description to a manufacturable physical layout.

The work follows a structured day-wise approach covering:

- Synthesis  
- Floorplanning  
- Placement  
- Clock Tree Synthesis  
- Routing  
- Final Layout Generation  

Each stage focuses on both theoretical understanding and practical implementation supported by reports, command execution, and layout visualization.

---

##  Learning Objectives

The main goals of this project are:

- To understand the complete ASIC design flow using open-source EDA tools  
- To analyze how RTL logic is transformed into standard-cell hardware  
- To observe how physical design decisions influence timing and layout quality  
- To gain hands-on experience with OpenLANE, Magic, and Sky130 libraries  

This repository acts as a practical reference for anyone beginning digital physical design using open tools.

---

## 🛠 Tools and Technologies Used

The following tools were used throughout the design flow:

- **OpenLANE** — Automated RTL-to-GDS flow  
- **Yosys** — Logic synthesis  
- **OpenROAD** — Physical design and optimization  
- **Magic** — Layout visualization  
- **Sky130 PDK** — Standard cell library and technology data  

Each tool contributes to a specific stage of the ASIC workflow, from logic mapping to final layout verification.

---

##  RTL to GDSII Flow Summary

```
Verilog RTL
      |
Synthesis (Yosys + ABC)
      |
Floorplanning & Power Planning
      |
Placement
      |
Clock Tree Synthesis
      |
Routing
      |
GDSII Layout Generation
```

This progression demonstrates how a behavioral hardware description evolves into a complete physical chip structure.

---

##  Day-Wise Documentation

### Day 1 — Inception of Open-Source EDA and Synthesis

- Introduction to OpenLANE and Sky130  
- Understanding library cells and flop ratio  
- RTL synthesis and timing report analysis  

### Day 2 — Floorplanning and Physical Preparation

- Core area definition and utilization analysis  
- Power planning concepts and tap cell insertion  
- Evaluation of floorplan quality  

### Day 3 — Placement

- Global placement and density optimization  
- Detailed placement and legalization  
- Layout visualization after placement  

### Day 4 — Clock Tree Synthesis

- Clock buffer insertion  
- Skew and latency analysis  
- Preparation for routing stage  

### Day 5 — Routing and Final Layout

- Metal layer routing  
- Design rule verification  
- Final layout generation and visualization  

Each day includes screenshots, commands used, reports generated, and important observations from the implementation.

---

##  Repository Structure

```
├── Day1/
├── Day2/
├── Day3/
├── Day4/
├── Day5/
├── Day1.md
├── Day2.md
├── Day3.md
├── Day4.md
├── Day5.md
└── README.md
```

---

##  Acknowledgment

I would like to sincerely express my gratitude to **Mr. Kunal Ghosh**, Co-founder of VLSI System Design (VSD) Corp. Pvt. Ltd., for creating an open and practical learning environment that made it possible to explore the complete RTL-to-GDSII flow using open-source tools. His structured approach to teaching physical design concepts greatly helped in building a clear understanding of real-world ASIC workflows.

I would also like to thank **Mr. Nickson Jose** for his valuable support and for guiding participants throughout the Digital VLSI SoC Design and Planning workshop. The insights shared during the sessions, along with the hands-on demonstrations, played an important role in strengthening both theoretical knowledge and practical skills.

This project is a result of the guidance, resources, and learning opportunities provided through the VSD program, which encourages students to gain industry-relevant experience using open-source EDA tools.