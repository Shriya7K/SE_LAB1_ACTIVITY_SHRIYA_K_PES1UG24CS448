# Software Engineering Lab Activities Repository

**Student Name:** Shriya K  
**SRN:** PES1UG24CS448  
**Department:** Computer Science & Engineering, PES University  
**Course:** Software Engineering Lab (SE_LAB)  
**Assigned Scenario:** Problem Statement #20 | Healthcare & Telemedicine: Hospital Bed & ICU Allocation Dashboard  

---

## Repository Organization

This repository contains all coursework submissions for Software Engineering Laboratory activities, structured into modular lab packages:

```text
SE_LAB1_ACTIVITY_SHRIYA_K_PES1UG24CS448/
├── README.md                                  # Repository overview and lab navigation index
│
├── lab1/                                      # Lab 1: Requirements Engineering & UML Use-Case Modelling
│   ├── README.md                              # Lab 1 summary & deliverables index
│   ├── 20_SE_Lab1_SE_Problem_Statements.pdf   # Lab 1 problem statement reference
│   ├── requirements/
│   │   ├── Requirements_Table.md              # Complete 5 FRs and 2 NFRs specification table
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
└── lab3/                                      # Lab 3: Component Modelling & Architectural Pattern Selection
    ├── README.md                              # Lab 3 summary & architecture index
    ├── Architecture_Selection_Justification.docx # Word document deliverable (1-page)
    ├── Architecture_Selection_Justification.pdf  # PDF version of justification document (1-page)
    ├── Architecture_Selection_Justification.md   # Markdown justification document
    ├── Component_Diagram.png                  # UML Component Diagram (11 components, ball & socket interfaces)
    ├── Component_Diagram.pdf                  # Exported high-resolution PDF Component Diagram
    ├── Architectural_Styles_Analysis.md       # Comparative analysis of Layered, Microservices & Client-Server
    └── Component_Modeling_Specification.md    # Formal component, ports, interfaces, and dependencies catalog
```

---

## Lab Modules Overview

### [Lab 1: Requirements Engineering & UML Use-Case Modelling](lab1/)
- **Objective:** Elicit verifiable functional and non-functional requirements, formulate measurable acceptance criteria, construct a UML Use-Case Diagram with `<<include>>` and `<<extend>>` relationships, and write a formal Use-Case Flow specification.
- **Key Deliverables:**
  - Complete Requirements Table (**FR-001 to FR-005**, **NFR-001 & NFR-002**) in Markdown, Word (.docx), and PDF.
  - UML Use-Case Diagram in PNG, PDF, and interactive Mermaid format.
  - Formal 1-page Use-Case Flow Specification for **UC-04: Initiate ICU Transfer** in Markdown, Word (.docx), and PDF.
  - Alternate Flows and Exception Flows specifications.

### [Lab 3: Component Modelling & Architectural Pattern Selection](lab3/)
- **Objective:** Evaluate different architectural styles (Layered, Microservices, Client-Server), select and justify the optimal architecture for Problem Statement #20, and design a comprehensive UML Component Diagram with proper ports, provided/required interfaces (ball & socket notation), usage dependencies (`<<use>>`), and assembly connectors.
- **Key Deliverables:**
  - UML Component Diagram (**11 components, 16 interfaces**) in PNG and PDF.
  - Written Justification Document for Layered Architecture in Word (.docx), PDF, and Markdown.
  - Comparative Architectural Styles Analysis evaluating trade-offs, security, and performance.
  - Detailed Component Modeling Technical Specification cataloging all ports, interfaces, and connectors.
