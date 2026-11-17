# Software Engineering Lab Activities Repository

**Student Name:** Shriya K  
**SRN:** PES1UG24CS448  
**Section:** 5H  
**Department:** Computer Science & Engineering, PES University  
**Course:** Software Engineering Lab (SE_LAB)  
**Assigned System:** Problem Statement #20 | Healthcare & Telemedicine: Hospital Bed & ICU Allocation Dashboard  

---

## Repository Structure & Modular Lab Packages

This repository contains all coursework submissions for Software Engineering Laboratory activities, cleanly organized into dedicated lab folders:

```text
SE_LAB1_ACTIVITY_SHRIYA_K_PES1UG24CS448/
├── README.md                                  # Repository overview and master lab index
│
├── lab1/                                      # Lab 1: Requirements Engineering & UML Use-Case Modelling
│   ├── README.md                              # Lab 1 overview & deliverables summary
│   ├── 20_SE_Lab1_SE_Problem_Statements.pdf   # Lab 1 problem statement reference
│   ├── requirements/
│   │   ├── Requirements_Table.md              # Complete 5 FRs & 2 NFRs specification table
│   │   ├── Requirements_Table.docx            # Word document deliverable
│   │   ├── Requirements_Table.pdf             # Landscape 1-page PDF deliverable
│   │   ├── Alternate_Flow.md                  # Alternate flow specifications
│   │   └── Exception_Flow.md                  # Exception flow specifications
│   ├── uml/
│   │   ├── Use_Case_Diagram.png               # UML Use-Case Diagram image
│   │   ├── Use_Case_Diagram.pdf               # Exported PDF Use-Case Diagram
│   │   └── Use_Case_Diagram.md                # Use-case specification & Mermaid diagram
│   └── use-case-flow/
│       ├── ICU_Transfer_Flow.md               # Formal 1-page IEEE Use-Case Flow Specification (UC-04)
│       ├── ICU_Transfer_Flow.docx             # Word document deliverable
│       └── ICU_Transfer_Flow.pdf              # 1-page PDF deliverable
│
├── lab2/                                      # Lab 2: Agile Project Management using Jira Kanban
│   ├── README.md                              # Lab 2 overview, Kanban walkthrough & screenshots
│   ├── LAB2_Kanban_Project_Documentation.docx # Word document documentation deliverable
│   ├── LAB2_Kanban_Project_Documentation.pdf  # PDF documentation deliverable
│   ├── Kanban_Work_Breakdown_Structure.md     # Full Jira WBS catalog (Epic, Stories, Tasks, Subtasks)
│   └── screenshots/                           # 8 high-resolution Jira screenshots
│       ├── 01_Project_Creation.png
│       ├── 02_Kanban_Board.png
│       ├── 03_Backlog_View.png
│       ├── 04_Epic_Details.png
│       ├── 05_Epic_Child_Items.png
│       ├── 06_Task_and_Subtasks.png
│       ├── 07_FR_Details_and_Acceptance_Criteria.png
│       └── 08_FR_Subtasks.png
│
└── lab3/                                      # Lab 3: Component Modelling & Architectural Pattern Selection
    ├── README.md                              # Lab 3 overview & architecture summary
    ├── Architecture_Selection_Justification.docx # Word document deliverable (1-page)
    ├── Architecture_Selection_Justification.pdf  # PDF version of justification document (1-page)
    ├── Architecture_Selection_Justification.md   # Markdown justification document
    ├── Component_Diagram.png                  # UML Component Diagram (11 components, 16 interfaces)
    ├── Component_Diagram.pdf                  # Exported high-resolution PDF Component Diagram
    ├── Architectural_Styles_Analysis.md       # Comparative analysis of Layered, Microservices & Client-Server
    └── Component_Modeling_Specification.md    # Formal component, ports, interfaces (ball & socket), and dependencies catalog
```

---

## Lab Modules Overview

### [Lab 1: Requirements Engineering & UML Use-Case Modelling](lab1/)
- **Objective:** Elicit verifiable functional and non-functional requirements, formulate measurable acceptance criteria, construct a UML Use-Case Diagram with `<<include>>` and `<<extend>>` relationships, and write a formal Use-Case Flow specification.
- **Key Deliverables:** Complete Requirements Table (**FR-001 to FR-005**, **NFR-001 & NFR-002**) in Markdown, Word (.docx), and PDF; UML Use-Case Diagram in PNG, PDF, and Mermaid; 1-page Use-Case Flow Specification for **UC-04: Initiate ICU Transfer**; and Alternate/Exception Flows.

### [Lab 2: Agile Project Management using Jira Kanban](lab2/)
- **Objective:** Configure an agile Kanban delivery framework in Jira Software for Problem Statement #20, establish Epics, User Stories, and Tasks with measurable acceptance criteria, decompose engineering work into subtasks, and track flow across a visual Kanban board.
- **Key Deliverables:** Jira Project `Kanban_BPS#20` (Key: `KBPS20`), Epic `KBPS20-1`, Backlog of 9 items, granular subtask decomposition (`KBPS20-10` to `KBPS20-18`, `KBPS20-22`, `KBPS20-23`), Word (.docx) & PDF documentation, and detailed Work Breakdown Structure.

### [Lab 3: Component Modelling & Architectural Pattern Selection](lab3/)
- **Objective:** Evaluate different architectural styles (Layered, Microservices, Client-Server), select and justify the optimal architecture for Problem Statement #20, and design a comprehensive UML Component Diagram with proper ports, provided/required interfaces (ball & socket notation), usage dependencies (`<<use>>`), and assembly connectors.
- **Key Deliverables:** UML Component Diagram (**11 components, 16 interfaces**) in PNG and PDF; 1-page Written Justification Document in Word (.docx), PDF, and Markdown; Architectural Styles Analysis; and Component Modeling Technical Specification.
