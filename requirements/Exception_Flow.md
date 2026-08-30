# Exception Flow

## System
Hospital Bed & ICU Allocation System

## FR-003: Allocate Available ER Bed

### Exception Flow
1. The system checks for an available ER bed.
2. If no suitable ER bed is available, the patient remains in
   the waiting list.
3. The system maintains the patient's priority based on triage
   severity.
4. The system continues monitoring bed availability.
5. When a suitable bed becomes available, the patient can be
   considered for allocation.

---

## FR-004: Generate ICU Inter-Facility Transfer Request

### Exception Flow
1. The system determines that the patient requires ICU care.
2. The system checks for available suitable ICU beds.
3. If no suitable ICU bed is available, an inter-facility
   transfer request is generated.
4. The system records the patient's transfer requirements.
5. If no suitable destination facility is available, the request
   remains pending until an appropriate facility is identified.

---

## FR-005: Send Ambulance Transfer Request

### Exception Flow
1. The system prepares the ambulance transfer request.
2. If the ambulance service rejects the request, the system
   records the rejection.
3. The system allows another transfer request to be initiated.
4. If the ambulance service does not respond, the request remains
   pending until a response is received.
