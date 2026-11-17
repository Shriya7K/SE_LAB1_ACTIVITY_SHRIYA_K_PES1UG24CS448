# Jira Kanban Work Breakdown Structure (WBS)

## Software Engineering Lab 2: Agile Project Management
**Course:** Software Engineering Lab | Department of CSE, PES University  
**Student Name:** Shriya K  
**SRN:** PES1UG24CS448  
**Section:** 5H  
**Project Name:** `Kanban_BPS#20`  
**Space Key:** `KBPS20`  
**Space Owner:** `Shriya .K`  
**Scenario:** Problem Statement #20 (Hospital Bed & ICU Allocation Dashboard)

---

## 1. Epic Definition

| Issue Key | Issue Type | Summary | Description | Status | Priority |
|:---:|:---:|---|---|:---:|:---:|
| **KBPS20-1** | **Epic** | **Hospital Bed & ICU Allocation** | Manage emergency room, ward, and ICU bed allocation by prioritizing patients based on triage severity, monitoring real-time bed availability, and coordinating inter-facility ambulance transfers. | To Do | High |

---

## 2. Product Backlog Items (Requirements & Core Tasks)

All items are linked to parent Epic **KBPS20-1**:

| Issue Key | Type | Summary | Priority | Status | Description / Acceptance Criteria |
|:---:|:---:|---|:---:|:---:|---|
| **KBPS20-2** | Story (FR) | **FR-001 - Compute Triage Severity and Prioritize ER Waitlist** | High | To Do | **Description:** The system shall compute triage severity scores from intake vitals and dynamically order the ER bed allocation waitlist.<br/>**Acceptance Criteria:** Pass: Higher acuity patient automatically moves ahead of lower acuity patient. Fail: FIFO queue ignores critical emergency. |
| **KBPS20-3** | Story (FR) | **FR-002 - Display Real-Time Bed Status** | High | To Do | **Description:** The system shall display the real-time status of ER, ward, and ICU beds as Available, Occupied, or Cleaning across connected client dashboards.<br/>**Acceptance Criteria:** Pass: Bed status updates reflect on dashboards in &lt; 1 sec. Fail: Dashboard displays stale status after 1 second. |
| **KBPS20-4** | Story (FR) | **FR-003 - Allocate Available ER Bed** | High | To Do | **Description:** The system shall allocate an available ER bed to a patient according to triage priority and bed availability.<br/>**Acceptance Criteria:** Pass: When an available suitable ER bed exists, the highest-priority waiting patient is assigned and bed status updates to Occupied. Fail: Lower-priority patient assigned while higher-priority patient waits. |
| **KBPS20-5** | Story (FR) | **FR-004 - Generate ICU Inter-Facility Transfer Request** | High | To Do | **Description:** The system shall identify patients requiring ICU transfer and generate an inter-facility transfer request when no suitable internal ICU bed is available.<br/>**Acceptance Criteria:** Pass: Generates transfer request with patient and partner hospital details when local ICU is full. Fail: No transfer request generated. |
| **KBPS20-6** | Story (FR) | **FR-005 - Send Ambulance Transfer Request** | Medium | To Do | **Description:** The system shall send ambulance transfer requests to external ambulance services and record resulting transfer status.<br/>**Acceptance Criteria:** Pass: Submitted request is sent to ambulance API and records status (Requested, Confirmed, Rejected). Fail: Sent without tracking response status. |
| **KBPS20-7** | Story (NFR) | **NFR-001 - Propagate Bed Status Transitions in Under 1 Second** | High | To Do | **Description:** Bed status transitions (Occupied, Cleaning, Available) must propagate to all connected ER dashboard clients in under 1 second via WebSockets.<br/>**Acceptance Criteria:** Pass: Latency benchmarks confirm &ge; 95% of updates reach clients in &lt; 1.0 s under peak concurrency. |
| **KBPS20-8** | Story (NFR) | **NFR-002 - Restrict Access Based on Role-Based Access Control** | High | To Do | **Description:** The system shall restrict patient and bed-management functions to authenticated and authorized users based on role-based access control (RBAC).<br/>**Acceptance Criteria:** Pass: Nurses access triage/allocation; Admins access transfer workflows; unauthorized actions return HTTP 403. |
| **KBPS20-9** | Task | **Implement Triage Severity Algorithm and Priority Engine** | Medium | To Do | Core engineering implementation of the clinical triage calculation algorithm and dynamic waitlist queue engine. |
| **KBPS20-19** | Task | **Implement Real-Time Bed State Synchronization Service** | Medium | To Do | WebSocket server implementation to broadcast state transitions for ER, Ward, and ICU beds. |

---

## 3. Engineering Subtasks Decomposition

### Subtasks for Task `KBPS20-9` (Implement Triage Severity Algorithm)

| Subtask Key | Summary | Priority | Status | Parent Task |
|:---:|---|:---:|:---:|:---:|
| **KBPS20-10** | Calculate Acuity Score from Physiological Intake Vitals | Medium | To Do | KBPS20-9 |
| **KBPS20-11** | Validate Triage Input Fields & Bounds Check | Medium | To Do | KBPS20-9 |
| **KBPS20-12** | Prioritize ER Bed Allocation Waitlist by Score | Medium | To Do | KBPS20-9 |
| **KBPS20-13** | Reorder Waitlist Dynamically on Patient Arrival / Re-Triage | Medium | To Do | KBPS20-9 |
| **KBPS20-14** | Handle Equal-Acuity Tie-Breaker via Arrival Timestamp | Medium | To Do | KBPS20-9 |
| **KBPS20-15** | Store Patient Triage Records in Patient Database | Medium | To Do | KBPS20-9 |
| **KBPS20-16** | Test Triage Scoring Algorithm with Clinical Vitals Edge Cases | Medium | To Do | KBPS20-9 |
| **KBPS20-17** | Verify Waitlist Preemption for High-Acuity Emergencies | Medium | To Do | KBPS20-9 |
| **KBPS20-18** | Verify FIFO Queue Fallback within Same Acuity Tier | Medium | To Do | KBPS20-9 |

### Subtasks for Story `KBPS20-4` (FR-003: Allocate Available ER Bed)

| Subtask Key | Summary | Priority | Status | Parent Story |
|:---:|---|:---:|:---:|:---:|
| **KBPS20-22** | Check Eligible Available ER Bed Matching Patient Requirements | Medium | To Do | KBPS20-4 |
| **KBPS20-23** | Assign Bed to Patient and Transition Bed Status to Occupied | Medium | To Do | KBPS20-4 |

---

## 4. Kanban Workflow States

The Kanban board defines four distinct stages of delivery:
1. **To Do:** Item is prioritized in the backlog ready for implementation.
2. **In Progress:** Developer or clinical engineer is actively implementing or testing the item.
3. **In Review:** Code review, acceptance criteria verification, and clinical safety checks.
4. **Done:** Successfully tested, peer-reviewed, and merged into the main release branch.
