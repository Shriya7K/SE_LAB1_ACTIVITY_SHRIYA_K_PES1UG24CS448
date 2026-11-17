# Architectural Styles Evaluation & Analysis

## Software Engineering Lab 3: Component Modelling & Architectural Pattern Selection
**Course:** Software Engineering Lab | Department of CSE, PES University  
**Student:** Shriya K (PES1UG24CS448)  
**System Scenario:** Hospital Bed & ICU Allocation Dashboard (Problem Statement #20)

---

## 1. Overview of Evaluated Architectural Styles

In software architecture design, selecting an appropriate architectural style requires evaluating system functional requirements, performance constraints, operational complexity, and security requirements. Three major architectural patterns were analyzed:
1. **Layered Architecture**
2. **Microservices Architecture**
3. **Client-Server Architecture**

---

## 2. Comparative Analysis of Patterns

### 1. Layered Architecture
- **Structure:** Organized into horizontal, distinct layers where each layer has a specific role (e.g., Presentation, Business Logic, Real-Time Event, and Data). Communication flows in a controlled direction.
- **Advantages (Pros):**
  - **Clear Separation of Concerns:** Isolates user interface from medical logic and database schemas.
  - **Maintainability & Simplicity:** Easier to design, test, debug, and govern within hospital IT infrastructure.
  - **Controlled Security Perimeter:** Centralized API Gateway and RBAC service enforce strict access controls.
  - **Low Operational Overhead:** Does not require complex distributed service meshes, container orchestrators, or distributed transaction managers.
- **Disadvantages (Cons):**
  - Performance overhead from requests traversing multiple layers.
  - Potential tight coupling between adjacent layers if interfaces are not strictly defined.
  - Individual components cannot be scaled completely independently without scaling the layer.

### 2. Microservices Architecture
- **Structure:** Decomposes the application into a suite of small, independently deployable services organized around business capabilities (e.g., Triage Microservice, Bed Inventory Microservice, Ambulance Dispatch Microservice).
- **Advantages (Pros):**
  - **Independent Scalability:** Services with high traffic can be scaled horizontally.
  - **Fault Isolation:** A failure in the ambulance service does not bring down local triage calculation.
  - **Technology Diversity:** Different services can adopt different technology stacks.
- **Disadvantages (Cons):**
  - **High Operational Complexity:** Requires complex deployment pipelines, container orchestration (Kubernetes), and distributed monitoring.
  - **Network Latency & Distributed Transactions:** Inter-service RPC calls introduce network latency and distributed data consistency challenges (e.g., two-phase commit or saga patterns).
  - Unnecessary overhead for a single emergency department deployment.

### 3. Client-Server Architecture
- **Structure:** Direct division of work between clients (dashboard frontends) and a centralized monolithic server providing services and database access.
- **Advantages (Pros):**
  - **Centralized Management:** Simple deployment and straightforward centralized data consistency.
  - Easy to implement and verify for basic CRUD applications.
- **Disadvantages (Cons):**
  - **Single Point of Failure:** Central server failure completely halts all emergency department functions.
  - **Scalability Bottlenecks:** Difficult to handle high concurrent WebSocket broadcasts alongside heavy transactional database queries.
  - Poor modularity and separation of concerns.

---

## 3. Architectural Style Comparison Matrix

| Evaluation Criteria | Layered Architecture (Selected) | Microservices Architecture | Client-Server Architecture |
|---|:---:|:---:|:---:|
| **Separation of Concerns** | High | High | Low to Medium |
| **Operational Simplicity** | High | Low (High Complexity) | High |
| **Real-Time Latency (&lt; 1s)** | High (via Event Layer) | Medium (Network Latency) | Medium |
| **Security & RBAC Enforcement** | Centralized & Strong | Distributed & Complex | Centralized but Rigid |
| **Data Consistency** | High (ACID Relational) | Eventual Consistency | High |
| **Suitability for Problem #20** | **Optimal (Selected)** | Over-Engineered | Insufficiently Modular |

---

## 4. Scenario Evaluation: Problem Statement #20 (Hospital Bed & ICU Allocation)

For the **Hospital Bed & ICU Allocation Dashboard**, the primary system priorities are:
1. Sub-second real-time status propagation across emergency room client dashboards (**NFR-001**).
2. Strict role-based access control safeguarding sensitive medical records (**NFR-002**).
3. Dependable ACID transaction consistency during high-stakes bed allocations and transfers.

**Why Layered Architecture is the Optimal Choice:**  
The Layered Architecture with a dedicated **Real-Time Event Layer** provides the perfect balance. It isolates the high-throughput WebSocket streaming engine from the persistent database layer, guarantees ACID consistency for critical bed allocation operations, and avoids the distributed transaction failures and networking latencies of microservices.

---

## 5. Reference Scenario Evaluation: Self-Service Coffee Kiosk System

As an architectural exercise from Lab 3 guidelines, the Self-Service Coffee Kiosk System was also evaluated:
- **Requirements:** 3 coffee types (Espresso, Americano, Latte), 2 drink sizes (Small, Large), credit card payment only, receipt printer hardware integration, touchscreen UI.
- **Optimal Choice:** **Layered Architecture**
  - *Presentation Layer:* Touchscreen User Interface Component.
  - *Business Logic Layer:* Order Manager Component, Payment Service Component, Receipt Printer Component.
  - *Data Layer:* Menu & Pricing Database Component.
- **Justification:** A standalone kiosk requires local hardware interface management (touchscreen, card reader, printer) and low operational complexity. Microservices would introduce needless latency and network failure modes on a single embedded kiosk.
