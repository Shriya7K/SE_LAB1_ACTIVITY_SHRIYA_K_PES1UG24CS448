# Alternate Flow Specifications

## System
Hospital Bed & ICU Allocation Dashboard  
**Problem Statement #20 | Healthcare & Telemedicine**  
**Course:** Software Engineering Lab 1 | Department of CSE, PES University  
**Student:** Shriya K (PES1UG24CS448)

---

## FR-001: Compute Triage Severity and Prioritize ER Waiting List

### Main Flow
1. Patient arrives at the Emergency Room intake desk.
2. Triage Nurse enters patient intake vitals (BP, HR, SpO2, Temp, RR, Consciousness level).
3. The system computes the patient's triage severity score (e.g., ESI level 1 to 5).
4. The patient is added to the dynamic ER waiting list.
5. The system sorts patients strictly by triage severity score (highest acuity first).

### Alternate Flow 1A: Equal Triage Severity (Arrival Time Tie-Breaker)
1. If two or more patients are evaluated with the identical triage severity score, the system examines their verified intake arrival timestamps.
2. The patient who arrived earlier (First-Come, First-Served within the same acuity tier) is placed higher on the waitlist.
3. The system updates the ER waitlist queue order dynamically.
4. The updated queue is rendered on all connected nurse consoles.

### Alternate Flow 1B: Clinical Condition Deterioration (Dynamic Re-Triage)
1. A waiting patient's vitals change or symptoms worsen while in the waiting area.
2. The Triage Nurse performs a rapid re-assessment and updates intake vitals.
3. The system recalculates the triage severity score immediately.
4. The patient is elevated automatically to a higher acuity position on the waitlist, bypassing lower-acuity patients.

---

## FR-002: Real-Time Multi-Ward Bed Status Monitoring

### Main Flow
1. Connected client dashboards open the Bed Status view.
2. The system queries current bed inventory across ER, General Ward, and ICU wards.
3. The dashboard displays each bed with color-coded status (`Available`, `Occupied`, `Cleaning`).
4. Real-Time Event Service pushes status changes over WebSockets within 1 second.

### Alternate Flow 2A: WebSocket Disconnection Fallback
1. If the client browser loses active WebSocket connection due to transient network interruption, the system detects connection timeout.
2. The dashboard activates an automatic fallback to HTTP long-polling (2-second interval).
3. A visual reconnection badge is displayed to the user while background auto-reconnect retries.
4. Once the WebSocket handshake succeeds, real-time push streaming resumes automatically.

### Alternate Flow 2B: Ward and Specialty Filtering
1. User filters the dashboard view by specific ward (ER, Medical ICU, Surgical ICU, General Ward) or isolation capability (Negative Pressure, Standard).
2. The system renders the filtered subset without interrupting the real-time event listener for updates.

---

## FR-003: Allocate Available ER Bed

### Main Flow
1. An ER bed transitions from `Cleaning` to `Available`.
2. The system identifies the top-ranked patient on the dynamic ER waitlist.
3. The Triage Nurse confirms the bed assignment.
4. The system updates the bed status to `Occupied` and removes the patient from the active waitlist.

### Alternate Flow 3A: Multiple Beds Available with Specialty Matching
1. Multiple ER beds are simultaneously available (e.g., trauma bay, cardiac monitored bed, standard cubicle).
2. The system matches the patient’s clinical requirement profile (e.g., cardiac monitoring) with bed capabilities.
3. If the ideal specialized bed is available, the system recommends that specific bed.
4. If the preferred specialized bed is unavailable, the system selects the next most suitable general bed equipped with portable monitoring gear.
5. The nurse confirms the assignment, and the bed status updates to `Occupied`.

### Alternate Flow 3B: Manual Clinical Override by Attending Physician
1. An attending physician or triage supervisor decides to allocate an available bed to a newly arriving trauma patient before system vitals entry completes.
2. The user activates "Emergency Fast-Track Allocation".
3. The system assigns the bed, flags the allocation as "Emergency Override", and prompts for post-intake vitals entry within 15 minutes.

---

## FR-004: Identify Patients Requiring ICU Transfer & Generate Inter-Facility Request

### Main Flow
1. Patient in ER is clinically evaluated as requiring intensive care.
2. System queries internal ICU beds and finds zero available beds (`Occupied` or `Cleaning`).
3. System triggers the ICU inter-facility transfer workflow.
4. Hospital Administrator selects an eligible partner hospital and confirms transfer generation.
5. System creates an inter-facility transfer request with status `Requested`.

### Alternate Flow 4A: Internal ICU Bed Becomes Available Prior to Dispatch
1. While the Administrator is reviewing external partner hospitals, an internal ICU bed completes sanitization and transitions to `Available`.
2. System raises an urgent pop-up notification: *"Internal ICU Bed [ICU-02] is now Available."*
3. The Administrator cancels the external transfer request and assigns the patient to the newly available internal bed.
4. Bed status transitions to `Occupied`, and the transfer workflow is closed.

### Alternate Flow 4B: Multiple Partner Hospitals with Ranked Capabilities
1. The regional partner directory returns multiple facilities with available ICU capacity.
2. The system sorts destination options by composite score based on travel distance, traffic-adjusted ETA, and clinical capability match (e.g., neuro-ICU vs. general ICU).
3. The Administrator approves the top-ranked facility with minimum transit delay.
4. The system proceeds with dispatch generation.

---

## FR-005: Send Ambulance Transfer Requests

### Main Flow
1. An inter-facility ICU transfer request is initiated.
2. System transmits an electronic dispatch request to the primary Ambulance Service.
3. Ambulance Service responds with `Confirmed`, assigning a vehicle ID and ETA.
4. System updates transfer status to `Confirmed` and logs transit details.

### Alternate Flow 5A: Failover to Secondary Ambulance Provider
1. The primary ambulance dispatch partner responds with `Rejected` due to fleet capacity constraints.
2. The system detects rejection and immediately fails over to the contracted secondary ambulance provider.
3. The secondary provider confirms the booking with an updated vehicle ID and ETA.
4. The system logs the dispatch transition and updates the transfer record to `Confirmed`.

### Alternate Flow 5B: En-Route Status and ETA Recalculation
1. The dispatched ambulance transmits dynamic GPS updates while in transit to the hospital.
2. In the event of heavy traffic delays, the ambulance telemetry service sends an updated ETA.
3. The system updates the dashboard countdown timer in real-time for clinical staff preparing the patient.
