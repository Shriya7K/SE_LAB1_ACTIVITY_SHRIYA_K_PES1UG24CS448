# Exception Flow Specifications

## System
Hospital Bed & ICU Allocation Dashboard  
**Problem Statement #20 | Healthcare & Telemedicine**  
**Course:** Software Engineering Lab 1 | Department of CSE, PES University  
**Student:** Shriya K (PES1UG24CS448)

---

## FR-001: Compute Triage Severity and Prioritize ER Waiting List

### Exception Flow 1.1: Missing or Out-of-Range Vital Signs Input
1. **Trigger:** Triage nurse enters incomplete vitals or values outside physiologically valid boundaries (e.g., Heart Rate > 300 bpm, SpO2 > 100%, Systolic BP < 20 mmHg).
2. **System Response:** 
   - The system intercepts the submission and highlights invalid input fields with inline validation errors.
   - The system prevents calculation with corrupt data and prompts the nurse to re-measure or verify the reading.
   - If emergency conditions prevent complete vital entry, the nurse may flag "Critical Distress / Unresponsive", triggering default Tier-1 (Resuscitation) allocation priority.

---

## FR-002: Real-Time Multi-Ward Bed Status Monitoring

### Exception Flow 2.1: WebSocket Server Communication Failure
1. **Trigger:** Network partition or WebSocket gateway crash prevents real-time event dissemination.
2. **System Response:**
   - Client dashboards detect heart-beat ping failure within 3 seconds.
   - A warning banner is displayed: *"Live Sync Interrupted – Polling Mode Active"*.
   - System automatically degrades to REST-based polling at 2-second intervals while background reconnection attempts continue.
   - When the connection is restored, the client fetches a state snapshot to reconcile any missed bed transitions.

---

## FR-003: Allocate Available ER Bed

### Exception Flow 3.1: Zero ER Beds Available (ER Saturation)
1. **Trigger:** All ER beds are occupied or undergoing cleaning when a high-acuity patient arrives.
2. **System Response:**
   - The patient remains in the ER waitlist with prioritized triage status displayed in amber/red alert.
   - The system alerts the charge nurse of bed saturation and marks the queue as "Surge Level 1".
   - The system continues automated continuous monitoring of bed sanitation states.
   - As soon as any ER bed transitions from `Cleaning` to `Available`, the system immediately allocates it to the waiting high-acuity patient.

### Exception Flow 3.2: Bed Inactivation During Maintenance / Deep Cleaning
1. **Trigger:** An ER bed is marked `Available`, but clinical staff discover a biohazard spill or equipment malfunction before patient placement.
2. **System Response:**
   - Nurse marks the bed as `Out of Service / Maintenance`.
   - The system immediately revokes assignment of that bed, rolls the patient back to the top of the waitlist without loss of priority, and updates connected dashboards via WebSockets within 1 second.

---

## FR-004: Identify Patients Requiring ICU Transfer & Generate Inter-Facility Request

### Exception Flow 4.1: Zero Available Partner ICU Beds in Regional Network
1. **Trigger:** Patient requires ICU admission, internal ICU beds are full, and queries to all integrated regional partner hospitals return zero available ICU beds.
2. **System Response:**
   - The system logs the transfer request state as `Pending - Regional Capacity Exceeded`.
   - The patient is placed in an ER critical holding bay under continuous telemetry and dedicated nursing supervision.
   - System sounds a high-priority alert on the Administrator console and sends an automated escalation notification to the Chief Medical Officer for manual inter-state mutual-aid coordination.
   - System schedules automated queries every 3 minutes to detect any newly freed ICU bed across the regional hospital network.

### Exception Flow 4.2: Target Receiving Hospital Cancels / Revokes Acceptance
1. **Trigger:** A receiving hospital initially accepts an ICU transfer but abruptly revokes acceptance before ambulance arrival due to an internal mass-casualty surge.
2. **System Response:**
   - Receiving facility sends cancellation notice via API or administrator logs manual revocation.
   - System updates transfer status to `Revoked by Destination`.
   - The patient is returned to active transfer staging, and the system prompts the Administrator to select the next ranked alternative partner hospital.
   - If an ambulance has already departed, the system transmits reroute instructions to the Ambulance Service.

---

## FR-005: Send Ambulance Transfer Requests

### Exception Flow 5.1: Ambulance Service Dispatches Rejection
1. **Trigger:** Ambulance service responds with `Rejected` due to severe weather, road blockages, or complete fleet unavailability.
2. **System Response:**
   - The system logs the rejection reason and timestamp in the audit database.
   - The system executes automatic failover to the secondary contracted ambulance provider.
   - If all integrated providers fail to provide a vehicle, the system triggers an emergency alert on the dashboard with direct phone dispatch contact details.

### Exception Flow 5.2: Ambulance Dispatch Gateway Timeout
1. **Trigger:** External ambulance API does not acknowledge dispatch payload within 90 seconds.
2. **System Response:**
   - System marks the request as `Dispatch Timed Out`.
   - System retries the API request with exponential backoff (attempt 2 of 3).
   - If timeout persists, system alerts the administrator to initiate direct telephony dispatch while preserving patient transfer records.

---

## NFR-001 & NFR-002: System Quality Exception Handling

### Exception Flow 6.1: Real-Time Latency Exceeds Benchmark (NFR-001)
1. **Trigger:** Server load surge causes WebSocket update propagation to exceed 1.0 second threshold.
2. **System Response:**
   - Monitoring service records latency spike and logs alert in telemetry log.
   - Dashboard UI displays last-synced timestamp to inform users of potential millisecond staleness.
   - Server dynamically spins up additional WebSocket worker nodes to restore sub-second latency.

### Exception Flow 6.2: Unauthorized Access Attempt (NFR-002)
1. **Trigger:** An unauthenticated user or an authenticated user with insufficient privileges (e.g., standard triage nurse attempting to authorize external ICU transfer or modify system security settings) invokes a protected endpoint.
2. **System Response:**
   - The API Gateway / Controller blocks the request immediately with HTTP 403 Forbidden.
   - The system generates a security audit event with IP address, user identity, attempted endpoint, and timestamp.
   - Access is denied and user dashboard displays: *"Access Denied: You do not possess the required administrative privileges to perform this operation."*
