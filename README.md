# SE_LAB1_hospital-bed-icu-allocation

## 1. Project Overview

The **Hospital Bed & ICU Allocation Dashboard** is a healthcare management system designed to support emergency-room bed allocation, ICU transfer decisions, real-time bed-status monitoring, and inter-facility ambulance transfer workflows.

This project is based on **Problem Statement #20 – Healthcare & Telemedicine** from the PES University CSE Software Engineering Lab.

### Problem Statement

**Hospital Bed & ICU Allocation Dashboard**

The system provides an emergency-room bed management dashboard that displays real-time ward occupancy, prioritizes ICU transfers based on triage score, and supports inter-facility ambulance transfer workflows.

### Primary Actors

- Triage Nurse
- Hospital Administrator
- Ambulance Service

---

# 2. Problem Statement

The Hospital Bed & ICU Allocation Dashboard is designed to:

1. Compute triage severity scores from patient intake vitals.
2. Dynamically prioritize the ER bed allocation waiting list.
3. Display real-time ER, ward, and ICU bed availability.
4. Allocate available ER beds according to triage priority and bed availability.
5. Identify patients requiring ICU transfer.
6. Generate inter-facility transfer requests when no suitable ICU bed is available.
7. Send ambulance transfer requests to the ambulance service.
8. Track ambulance transfer status.

---

# 3. Functional Requirements

| Req ID | Type | Description | Priority |
|---|---|---|---|
| FR-001 | Functional | The system shall compute triage severity scores from intake vitals and dynamically order the ER bed allocation waitlist. | High |
| FR-002 | Functional | The system shall display the real-time status of ER, ward, and ICU beds as Available, Occupied, or Cleaning. | High |
| FR-003 | Functional | The system shall allocate an available ER bed to a patient according to triage priority and bed availability. | High |
| FR-004 | Functional | The system shall identify patients requiring ICU transfer and generate an inter-facility transfer request when no suitable ICU bed is available. | High |
| FR-005 | Functional | The system shall send ambulance transfer requests to the ambulance service and record the transfer status. | Medium |

---

# 4. Non-Functional Requirements

| Req ID | Type | Description | Priority |
|---|---|---|---|
| NFR-001 | Performance & Security | The system shall propagate bed status transitions to all connected ER dashboard clients in under 1 second via WebSockets. | High |
| NFR-002 | Security | The system shall restrict patient and bed-management functions to authenticated and authorized users based on their assigned role. | High |

---

# 5. System Architecture

The system follows a layered architecture consisting of:

### 1. Actors / External Systems

- Triage Nurse
- Hospital Administrator
- Ambulance Service

### 2. Web Dashboard / User Interface

- Patient Intake & Triage
- ER Waitlist
- Bed Status Dashboard
- Transfer Management
- Reports & Administration

### 3. Authentication & Authorization Layer

- User Authentication
- Role-Based Access Control
- Session Management

### 4. Application / API Layer

- Triage Severity & Priority Engine
- Bed Allocation Manager
- Bed Availability / Status Monitor
- ICU Transfer Manager
- Ambulance Transfer Coordinator

### 5. Real-Time Event Layer

- WebSocket-based real-time event service
- Bed status updates
- ER waitlist updates
- Transfer status updates

### 6. Data Layer

- Patient and triage records
- Bed inventory and status
- Transfer requests and statuses
- Audit and access records

---

# 6. Architecture Flow

```text
                    ┌──────────────────────────────┐
                    │      ACTORS / SYSTEMS        │
                    │                              │
                    │  Triage Nurse                │
                    │  Hospital Administrator      │
                    │  Ambulance Service           │
                    └──────────────┬───────────────┘
                                   │
                                   ▼
                    ┌──────────────────────────────┐
                    │       WEB DASHBOARD          │
                    │                              │
                    │ Patient Intake & Triage      │
                    │ ER Waitlist                   │
                    │ Bed Status Dashboard          │
                    │ Transfer Management            │
                    └──────────────┬───────────────┘
                                   │
                                   ▼
                    ┌──────────────────────────────┐
                    │ AUTHENTICATION &             │
                    │ AUTHORIZATION                │
                    │                              │
                    │ User Authentication          │
                    │ Role-Based Access Control    │
                    │ Session Management            │
                    └──────────────┬───────────────┘
                                   │
                                   ▼
              ┌────────────────────────────────────────────┐
              │       HOSPITAL APPLICATION / API           │
              │                                            │
              │ Triage Severity & Priority Engine           │
              │                                            │
              │ Bed Allocation Manager                      │
              │                                            │
              │ Bed Availability / Status Monitor           │
              │                                            │
              │ ICU Transfer Manager                        │
              │                                            │
              │ Ambulance Transfer Coordinator              │
              └───────────────┬────────────────────────────┘
                              │
              ┌───────────────┴─────────────────┐
              │                                 │
              ▼                                 ▼
┌──────────────────────────────┐   ┌───────────────────────────┐
│ REAL-TIME EVENT SERVICE      │   │ EXTERNAL AMBULANCE        │
│                              │   │ SERVICE                   │
│ WebSocket                   │   │                           │
│                              │   │ Transfer Requests         │
│ Bed Status Updates           │   │ Confirmed / Rejected      │
│ Waitlist Updates             │   │ Status Updates            │
│ Transfer Status Updates      │   │                           │
└──────────────┬───────────────┘   └───────────────────────────┘
               │
               ▼
┌──────────────────────────────────────────────────────────────┐
│                    DATABASE / STORAGE                        │
│                                                              │
│ Patient & Triage Records                                     │
│ Bed Inventory & Status                                       │
│ Transfer Requests & Status                                   │
│ Audit & Access Records                                       │
└──────────────────────────────────────────────────────────────┘
