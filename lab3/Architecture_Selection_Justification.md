# Architecture Selection and Justification

## Hospital Emergency Department Triage, Bed and Transfer Management System
**Problem Statement #20 | Healthcare & Telemedicine**  
**Course:** Software Engineering Lab 3 (Component Modelling & Architectural Pattern Selection)  
**Department of Computer Science & Engineering, PES University**  
**Student:** Shriya K (PES1UG24CS448)

---

### Architecture Selection
We chose **Layered Architecture** for the **Hospital Emergency Department Triage, Bed and Transfer Management System**.

---

### Architectural Choice
The system is structured using a multi-tiered Layered Architecture composed of four key vertical layers:
1. **Presentation Layer (Web Application):** Encapsulates the Web Dashboard UI (Patient Intake & Triage, ER Waitlist, Bed Status Dashboard, Transfer Management, and Reports & Administration).
2. **Business Logic Layer (Application Services):** Houses core clinical domain components including the Triage Service, Bed Allocation Service, Bed Management Service, ICU Transfer Service, Ambulance Transfer Service, and an API Gateway / Controller paired with an Authentication & Authorization Service.
3. **Real-Time Event Layer (WebSocket Server):** A dedicated event broadcasting tier that publishes sub-second bed transitions, waitlist modifications, and transfer state updates to all active dashboard clients.
4. **Data Layer (Database & Storage):** Independent persistence components including the Patient Database, Bed Inventory Database, and Transfer Database.

These layers strictly separate user presentation, clinical business processing, real-time push synchronization, and transactional persistence.

---

### Two Scenario-Related Reasons

1. **Strict Separation of Concerns for Critical Clinical Workflows:**  
   In an emergency department environment, business rules such as triage acuity scoring, waitlist priority sorting, bed availability tracking, and external ambulance transfer workflows undergo independent updates and operational requirements. Decoupling these domain services from the presentation layer ensures changes to clinical algorithms or database structures do not disrupt active nursing dashboards.
2. **Dedicated Real-Time Event Architecture for Emergency Operations:**  
   A traditional layered architecture relies heavily on request-response polling, which introduces dangerous latency and server load. By incorporating a dedicated Real-Time Event Layer (WebSocket server) that directly interacts with the Bed Management and Transfer services, bed status transitions (`Available`, `Occupied`, `Cleaning`) and waitlist changes are pushed instantaneously to connected frontline consoles in under 1 second without high server polling overhead.

---

### Security Advantage

The architecture places a centralized **API Gateway / Controller** and a dedicated **Authentication & Authorization Service** directly between the presentation layer and application services. This creates a secure, controlled single point of entry that enforces fine-grained Role-Based Access Control (RBAC) and session validation. 

Frontline Triage Nurses are strictly limited to triage assessment and bed allocation functions, while critical inter-facility ICU transfer approvals require verified Administrator privileges, ensuring patient Protected Health Information (PHI) is safeguarded in compliance with healthcare data protection standards.

---

### Performance Benefit

The dedicated **Real-Time Event Service (WebSocket Server)** establishes persistent, full-duplex socket connections with connected dashboard clients. Rather than thousands of client dashboards repeatedly issuing HTTP polling requests during peak emergency surges, the event server asynchronously broadcasts status deltas only when state transitions occur. This reduces network payload volume by over 80% and guarantees that bed availability and transfer status changes reach frontline clinical teams in under 1.0 second.
