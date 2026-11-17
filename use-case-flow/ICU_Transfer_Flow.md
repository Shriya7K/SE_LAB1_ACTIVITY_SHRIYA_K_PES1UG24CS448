# Use-Case Flow Specification

## UC-04: Initiate ICU Inter-Facility Transfer
**System:** Hospital Bed & ICU Allocation Dashboard  
**Problem Statement #20 | Healthcare & Telemedicine**  
**Course:** Software Engineering Lab 1 | Department of CSE, PES University  
**Author:** Shriya K (PES1UG24CS448)

---

## 1. Document Overview & Metadata

| Field | Specification |
|---|---|
| **Use Case ID** | **UC-04** |
| **Use Case Name** | **Initiate ICU Inter-Facility Transfer** |
| **Primary Actor** | Hospital Administrator |
| **Secondary / Supporting Actors** | Triage Nurse, Ambulance Service (External System), Receiving Hospital |
| **Stakeholders & Interests** | **Patient:** Receives timely critical ICU intervention without life-threatening delays.<br/>**Triage Nurse:** Rapidly escalates deteriorating patients beyond ER holding capacity.<br/>**Hospital Administrator:** Ensures optimal bed utilization, compliance, and inter-facility protocols.<br/>**Ambulance Service:** Receives accurate patient acuity data and destination routing.<br/>**Receiving Facility:** Obtains electronic clinical handover data in advance. |
| **Trigger** | A patient in the emergency department requires intensive care, and the system confirms zero local ICU beds are available (`Occupied` or `Cleaning`). |
| **Traceable Requirements** | **FR-004** (Mandatory), **FR-002**, **FR-005**, **NFR-001**, **NFR-002** |

---

## 2. Preconditions

1. **Authentication & Authorization:** The Hospital Administrator is logged in with verified credentials and role permissions for transfer authorization (satisfying **NFR-002**).
2. **Clinical Eligibility:** The patient is admitted to the ER with intake vitals, and the system has computed a high-acuity triage severity score requiring intensive care (satisfying **FR-001**).
3. **Internal Capacity Constraint:** The system has evaluated internal bed inventory and verified that all internal ICU beds are currently unavailable (either `Occupied` or `Cleaning`) (satisfying **FR-002**).

---

## 3. Postconditions

### Success Postconditions (Guaranteed Deliverable)
1. An inter-facility ICU transfer request is created and recorded in the database with status `Confirmed` (satisfying **FR-004**).
2. An automated transfer dispatch request is transmitted to the external Ambulance Service (satisfying **FR-005**, invoking **UC-05**).
3. The Ambulance Service confirmation code, assigned vehicle unit ID, and estimated arrival time (ETA) are logged.
4. The Real-Time Event Service broadcasts the transfer status transition to all connected ER dashboard clients in under 1 second via WebSockets (satisfying **NFR-001**).
5. The patient record is updated to `Transfer In Progress` with target facility details.

### Failure / Minimal Guarantees Postconditions
1. The patient remains safely allocated to an ER emergency holding bed under continuous vitals monitoring.
2. The transfer request is retained in `Pending - Capacity Escalation` state with reason logged in the audit trail.
3. System alerts supervisory clinical staff for manual telephone mutual-aid coordination.

---

## 4. Main Success Scenario (Happy Path)

| Step | Actor Action | System Response |
|:---:|---|---|
| **1** | — | System detects that a critical ER patient requires ICU admission and that all internal ICU beds are unavailable (`Occupied` or `Cleaning`). |
| **2** | — | System generates an urgent notification on the dashboard prompting ICU transfer initiation. |
| **3** | **Hospital Administrator** selects the notification and opens the **Transfer Management UI**. | System retrieves and displays the patient’s intake vitals, triage severity score, and required clinical equipment (e.g., ventilator, cardiac monitor). |
| **4** | **Hospital Administrator** triggers partner facility query. | System queries integrated regional partner hospitals and displays a real-time list of available ICU beds, distances, and transit times. |
| **5** | **Hospital Administrator** selects the optimal partner hospital and confirms the transfer request. | System validates selection and formats the standardized inter-facility transfer dossier. |
| **6** | — | System creates the transfer record with status `Requested` in the Transfer Database (**FR-004**). |
| **7** | — | System invokes **UC-05: Coordinate Ambulance Transfer**, sending an electronic dispatch request with patient acuity and destination coordinates to the **Ambulance Service** (**FR-005**). |
| **8** | **Ambulance Service** accepts the dispatch request. | System receives confirmation containing vehicle ID (ALS-104) and estimated arrival time (ETA: 12 minutes). |
| **9** | — | System updates transfer record status to `Confirmed` and logs transit telemetry. |
| **10** | — | System publishes transfer update through the Real-Time Event Service via WebSockets, updating all connected ER dashboards within 1 second (**NFR-001**). |
| **11** | **Triage Nurse** receives dashboard update and prepares patient for transport handover. | System marks ER bed status as `Pending Transfer Handover`. |

---

## 5. Alternate Flows

### Alternate Flow A: Local ICU Bed Transitions to Available Prior to External Dispatch
- **A1.** At Step 4 of the main flow, prior to finalizing an external hospital transfer, an internal ICU bed completes sanitization and transitions from `Cleaning` to `Available`.
- **A2.** The system detects local bed availability and displays an immediate modal alert: *"Local ICU Bed [ICU-B03] is now Available."*
- **A3.** The Hospital Administrator cancels external transfer initiation and confirms internal allocation to `ICU-B03`.
- **A4.** The system updates `ICU-B03` status to `Occupied`, cancels external transfer staging, and updates the patient location on the dashboard.

### Alternate Flow B: Multiple Partner Hospitals Available with Differentiated Capabilities
- **B1.** At Step 4, multiple partner facilities return available ICU capacity.
- **B2.** The system automatically sorts candidate hospitals by a multi-factor score: (Transit Time × 0.6) + (Specialty Capability Match × 0.4).
- **B3.** The Hospital Administrator reviews the top recommendation and accepts the optimal choice.
- **B4.** The flow resumes from Step 5 of the Main Success Scenario.

---

## 6. Exception Flows

### Exception Flow E1: No Regional Partner Hospital Has Available ICU Beds
- **E1.1.** At Step 4, query returns zero available ICU beds across all regional partner facilities.
- **E1.2.** System marks the transfer status as `Pending - Regional Capacity Exceeded`.
- **E1.3.** System elevates patient triage priority on the ER waitlist to top priority with an emergency hold alert.
- **E1.4.** System activates automated retries every 3 minutes while notifying the Chief Medical Officer for emergency manual inter-state coordination.

### Exception Flow E2: Ambulance Service Rejects Transfer Request or Times Out
- **E2.1.** At Step 8, the primary Ambulance Service responds with `Rejected` (due to fleet exhaustion) or fails to acknowledge within 90 seconds.
- **E2.2.** System records rejection status and reason in the audit log.
- **E2.3.** System automatically re-routes the dispatch request to the contracted secondary backup ambulance provider.
- **E2.4.** Upon secondary provider confirmation, the flow resumes at Step 9. If all ambulance services are unavailable, the system sounds an audible alert on the Administrator dashboard for manual dispatcher telephone override.

---

## 7. Special Requirements & Quality Attributes

- **Latency (NFR-001):** Transfer status transitions must propagate across all active WebSocket client connections in $\le 1.0$ second under peak concurrency.
- **Security (NFR-002):** Transfer initiation and destination confirmation require verified `ROLE_ADMINISTRATOR` or `ROLE_CHIEF_NURSE` authorization tokens.
- **Auditability:** Every state change (`Requested` $\rightarrow$ `Confirmed` $\rightarrow$ `In-Transit` $\rightarrow$ `Completed`) must be immutably recorded with ISO-8601 timestamps and user IDs.
