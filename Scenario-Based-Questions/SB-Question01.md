# SB-Question 01: Your organization currently has a single 'Platform Team' responsible for the Kubernetes cluster, CI/CD pipelines, and shared libraries. This team is constantly a bottleneck for the three product development teams. How would you redesign the organizational structure—specifically the roles, responsibilities, and communication patterns—to better align with an autonomous microservices architecture? Give a specific example of a shared component that you would de-couple from the Platform Team and assign to a Product Team, and why this change improves velocity.

## Answer:

## 1. Diagnose the Root Problem

The issue is not Kubernetes or CI/CD—it’s **misaligned team boundaries**.

A single Platform Team owning:

* Kubernetes clusters
* CI/CD pipelines
* Shared libraries

creates:

* A **centralized gatekeeper**
* High coordination cost
* Slow feedback loops
* Violation of Conway’s Law (architecture mirrors org structure)

In a microservices architecture, **teams—not a central group—should own the full lifecycle** of their services.

---

## 2. Target Organizational Model

I would redesign the organization using a **“Platform as a Product + Stream-Aligned Teams”** model (inspired by *Team Topologies*).

### Team Types

1. **Platform Enablement Team (formerly “Platform Team”)**
2. **Stream-Aligned Product Teams (3 teams)**
3. *(Optional)* **Enabling Team** for short-term coaching

---

## 3. Redesigned Roles & Responsibilities

### 3.1 Platform Enablement Team (Not a Gatekeeper)

**What they own**

* Kubernetes *platform*, not workloads
* Base infrastructure primitives
* Developer self-service tooling

**Responsibilities**

* Maintain Kubernetes clusters (control plane, networking, security)
* Provide standardized **golden paths**
* Offer opinionated templates and APIs
* Ensure compliance, reliability, and observability

**What they explicitly do NOT own**

* Service pipelines
* Application-level libraries
* Day-to-day deployment changes
* Feature-level decisions

**Key shift**:
They become **internal product builders**, not ticket handlers.

---

### 3.2 Product (Stream-Aligned) Teams

Each product team owns:

* Their microservices
* CI/CD pipelines for those services
* Runtime configuration
* Application-level shared components relevant to their domain
* On-call and production support

This gives them **true end-to-end ownership**:

> “You build it, you run it.”

---

## 4. Communication Patterns

### Before (Current State)

* Product Teams → Platform Team → Infrastructure
* Heavy ticketing
* Async delays
* Context switching

### After (Proposed)

* **Self-service first**
* Platform Team publishes:

  * Documentation
  * APIs
  * Templates
* Product Teams consume without approval loops
* Platform Team acts as:

  * Consultants
  * Escalation support
  * System stewards

**Communication becomes**

* Asynchronous
* Contract-based (APIs, interfaces)
* Rarely synchronous

---

## 5. Concrete De-Coupling Example

### Shared Component to Decouple: **CI/CD Pipelines**

#### Current State

* Platform Team owns all CI/CD pipelines
* Every pipeline change requires:

  * A ticket
  * A review
  * Waiting in queue

This leads to:

* Slow iteration
* One-size-fits-all pipelines
* Inflexible workflows

---

### Redesigned Ownership

**Platform Team provides**

* A standardized CI/CD **framework**, not pipelines

  * Base GitHub Actions / GitLab CI templates
  * Security scanning steps
  * Compliance checks
* Clear extension points

**Product Team owns**

* Their service-specific pipelines
* Build steps
* Deployment logic
* Feature flags
* Rollback strategies

---

### Example in Practice

The **Payments Product Team** owns:

* `payments-service`
* Its CI/CD pipeline definition

They:

* Customize deployment strategies (canary vs blue-green)
* Add domain-specific tests
* Adjust release cadence independently

The Platform Team:

* Maintains the reusable pipeline templates
* Ensures security and compliance steps remain mandatory

---

## 6. Why This Improves Velocity

### 6.1 Reduced Coordination Overhead

* No waiting for another team to unblock changes
* Teams deploy when ready

### 6.2 Faster Feedback Loops

* Developers own failures end-to-end
* Fixes happen immediately

### 6.3 Better Alignment with Microservices

* Organizational boundaries match service boundaries
* Fewer cross-team dependencies

### 6.4 Platform Team Scales Better

* One platform team can support many product teams
* Knowledge encoded in tooling instead of people

---

## 7. Governance Without Bottlenecks

Autonomy does **not** mean chaos.

Guardrails are enforced through:

* Mandatory pipeline steps
* Kubernetes admission controllers
* Policy-as-code (OPA/Gatekeeper)
* Shared observability standards

This keeps:

* Security
* Compliance
* Reliability

centralized **without** centralizing execution.

---

## 8. How I’d Close This in an Interview

> “I wouldn’t try to ‘fix’ the Platform Team—I’d redesign the system so they’re no longer a bottleneck. By turning the platform into a self-service product and pushing CI/CD ownership down to product teams, we align team autonomy with the microservices architecture. The result is faster delivery, clearer ownership, and a platform team that scales with the organization rather than slowing it down.”
