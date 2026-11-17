# Complete Requirements Table

## Hospital Bed & ICU Allocation Dashboard
**Problem Statement #20 | Healthcare & Telemedicine**  
**Course:** Software Engineering Lab 1 (Requirements Engineering & UML Use-Case Modelling)  
**Department of Computer Science & Engineering, PES University**  
**Student:** Shriya K (PES1UG24CS448)

---

### Overview
This document specifies the software requirements for the **Hospital Bed & ICU Allocation Dashboard** as mandated by Lab 1 deliverables:
- **Exactly 5 Functional Requirements (FR-001 to FR-005)**
- **2 Non-Functional Requirements (NFR-001 & NFR-002)**
- Complete specification with ID, Type, Description, Priority, Acceptance Criteria, and Rationale.

---

## Requirements Specification Table

| Req ID | Type | Description | Priority | Acceptance Criteria | Rationale | Mapped Use Case |
|:---:|:---:|---|:---:|---|---|:---:|
| **FR-001** | Functional | The system shall compute triage severity scores from intake vitals (BP, HR, SpO2, Temp, RR, Consciousness) and dynamically order the ER bed allocation waitlist. | High | **Pass:** A patient with a higher acuity score is automatically placed ahead of a lower-acuity patient on the waitlist.<br/><br/>**Fail:** The waitlist remains strictly FIFO when a critical patient arrives. | Ensures critical and life-threatened patients receive priority clinical intervention based on medical urgency rather than arrival sequence. | UC-01 |
| **FR-002** | Functional | The system shall display the real-time status of ER, ward, and ICU beds as Available, Occupied, or Cleaning across connected client dashboards. | High | **Pass:** When a bed status changes, the updated status is displayed on all connected client dashboards within 1 second.<br/><br/>**Fail:** The dashboard continues displaying stale bed status after 1 second of transition. | Provides clinical frontline staff and administrators with continuous visibility into hospital-wide bed availability for rapid decision-making. | UC-02 |
| **FR-003** | Functional | The system shall allocate an available ER bed to a patient according to triage priority and bed capability compatibility. | High | **Pass:** When an eligible ER bed becomes available, the highest-priority waiting patient is assigned and the bed status transitions to `Occupied`.<br/><br/>**Fail:** A lower-priority patient is assigned while a higher-priority eligible patient remains waiting. | Prevents misallocation, enforces clinical fairness, and minimizes emergency room boarding delays for acute patients. | UC-03 |
| **FR-004** | Functional | The system shall identify patients requiring ICU transfer and generate an inter-facility transfer request when no suitable internal ICU bed is available. | High | **Pass:** When an ICU transfer is requested and no suitable local ICU bed is available, the system generates an inter-facility transfer request containing patient and target hospital details.<br/><br/>**Fail:** No transfer request is generated under full internal ICU occupancy. | Supports timely clinical escalation and life-saving transfers when local intensive care capacity is exhausted. | UC-04 |
| **FR-005** | Functional | The system shall send ambulance transfer requests to external ambulance services and record the resulting transfer status (Requested, Confirmed, Rejected). | Medium | **Pass:** A submitted transfer request is transmitted to the ambulance dispatch API, and its status transitions to `Requested`, `Confirmed`, or `Rejected` based on the response.<br/><br/>**Fail:** The request is sent without capturing and recording its resulting dispatch status. | Enables real-time tracking, accountability, and coordination of critical inter-facility patient transit. | UC-05 |
| **NFR-001** | Non-Functional (Performance & Real-Time) | The system shall propagate bed status transitions (Occupied, Cleaning, Available) to all connected ER dashboard clients in under 1 second via WebSockets. | High | **Pass:** Under simulated peak concurrency load, at least 95% of bed-status updates reach all connected client dashboards in under 1.0 second.<br/><br/>**Fail:** More than 5% of bed-status updates exceed the 1.0-second propagation threshold. | Real-time visibility is vital in emergency medicine; delayed bed status updates can lead to fatal triage delays and misallocation. | UC-02, UC-04 |
| **NFR-002** | Non-Functional (Security & Access Control) | The system shall restrict patient and bed-management functions to authenticated and authorized users based on their assigned role (RBAC). | High | **Pass:** Triage Nurses can access triage and bed-allocation workflows, Hospital Administrators can access management and transfer approval functions, and unauthorized users are blocked with HTTP 403 Forbidden.<br/><br/>**Fail:** Any user can access or execute operations outside their assigned role permissions. | Protects sensitive Protected Health Information (PHI) and critical hospital infrastructure control from unauthorized tampering and unauthorized access. | All Use Cases |

---

## Traceability to Architecture Components

- **FR-001:** Handled by `Triage Service` and `Patient Database`.
- **FR-002:** Handled by `Bed Management Service`, `Real-Time Event Service (WebSocket)`, and `Bed Inventory Database`.
- **FR-003:** Handled by `Bed Allocation Service`, `Bed Management Service`, and `Patient Database`.
- **FR-004:** Handled by `ICU Transfer Service`, `Transfer Database`, and `Partner Hospital APIs`.
- **FR-005:** Handled by `Ambulance Transfer Service` and `External Ambulance Service`.
- **NFR-001:** Implemented via `Real-Time Event Service (WebSocket Server)`.
- **NFR-002:** Implemented via `API Gateway / Controller` and `Authentication & Authorization Service (RBAC)`.
