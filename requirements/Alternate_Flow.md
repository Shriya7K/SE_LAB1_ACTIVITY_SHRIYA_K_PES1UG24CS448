# Alternate Flow

## System
Hospital Bed & ICU Allocation System

## FR-001: Compute Triage Severity and Prioritize ER Waiting List

### Main Flow
1. Patient arrives at the Emergency Room.
2. The system computes the patient's triage severity.
3. The patient is added to the ER waiting list.
4. Patients are prioritized based on severity.

### Alternate Flow
1. If two or more patients have the same triage severity,
   the system compares their arrival times.
2. The patient who arrived earlier is given higher priority.
3. The system updates the ER waiting list accordingly.
4. The updated priority order is displayed to authorized users.

---

## FR-003: Allocate Available ER Bed

### Main Flow
1. The system identifies an available ER bed.
2. The system selects the highest-priority eligible patient.
3. The bed is allocated to the patient.
4. The bed status is updated.

### Alternate Flow
1. If multiple beds are available, the system selects a suitable
   bed based on the patient's requirements.
2. If the preferred bed is unavailable, another suitable available
   bed is selected.
3. The system assigns the selected bed to the patient.
4. The bed status is updated.
