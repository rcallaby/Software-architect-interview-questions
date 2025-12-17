# SB - Question 04: You've just deployed a critical v2 API for the mobile team, which has deprecated three endpoints from v1. Two hours later, you discover a high-priority external partner has not updated their systems and is still calling the old v1 endpoints, causing critical service degradation for them. Describe your immediate mitigation strategy (the 'fire drill') and your long-term API versioning policy moving forward. What specific architectural pattern (e.g., API Gateway policies, Mediator service, Sunset headers) will you implement to prevent this from ever happening again?

## Answer:

## 1. Reframing the Problem (Architect’s Perspective)

This incident has **two distinct dimensions**:

1. **Operational Failure**
   A critical external dependency is broken, causing immediate partner impact and potential contractual or reputational risk.

2. **Architectural & Governance Failure**
   The system allowed deprecated endpoints to be removed (or to degrade) **without enforcing safe deprecation contracts**, observability, or partner readiness verification.

A strong architect addresses **both simultaneously**:

* **Stabilize first** (minutes to hours)
* **Fix the system design** (weeks to months)

---

## 2. Immediate Mitigation Strategy (“Fire Drill”)

### Primary Objective

**Restore partner functionality immediately without rolling back v2 or redeploying mobile clients.**

### Step 1: Stop the Bleeding (Minutes)

**At the API Gateway layer**, I would immediately:

* **Re-enable v1 endpoints** if they were removed or degraded
* OR **route v1 traffic to a compatibility layer** if the backend logic has changed

This avoids:

* Rolling back v2
* Redeploying mobile apps
* Breaking other consumers

> The key principle: *Never force clients to migrate during an incident.*

---

### Step 2: Introduce a Temporary Compatibility Adapter

If the underlying service logic has changed:

* Deploy a **thin adapter / mediator service**:

  * Accepts **v1 request contracts**
  * Transforms them into **v2 request models**
  * Translates v2 responses back to v1 format

This can be:

* A lightweight service
* Or logic inside the API Gateway (if supported)

**Result**:
Partners keep working, mobile stays on v2, no duplication of business logic.

---

### Step 3: Throttle, Protect, and Observe

Since the partner traffic is already causing degradation:

* Apply **rate limits** at the gateway for v1 traffic
* Add **circuit breakers** to isolate failures
* Enable **fine-grained metrics**:

  * Per-endpoint usage
  * Partner ID or API key attribution
  * Error rates by version

This ensures the mitigation doesn’t create **secondary outages**.

---

### Step 4: Immediate Communication

While engineering stabilizes:

* Notify the partner:

  * v1 is temporarily restored
  * v1 is deprecated and has a firm sunset date
* Notify internal stakeholders:

  * Incident root cause
  * No rollback of v2 required
  * Clear remediation timeline

This protects trust and prevents escalation.

---

## 3. Root Cause Analysis (Post-Incident)

This failure happened because:

* Deprecated endpoints were **removed or degraded too aggressively**
* There was **no enforcement mechanism** to verify external consumers had migrated
* API versioning relied on **documentation alone**, not policy or runtime controls

This is a **process + architecture gap**, not just a communication failure.

---

## 4. Long-Term API Versioning Policy (Preventing Recurrence)

### Core Principles

1. **Backward compatibility is the default**
2. **Breaking changes require enforced transition periods**
3. **Deprecation is observable, measurable, and contractual**

---

## 5. Specific Architectural Patterns to Implement

### API Gateway–Enforced Versioning (Primary Control Plane)

At the API Gateway (e.g., Kong, Apigee, AWS API Gateway):

* Explicit version routing:

  * `/v1/*`
  * `/v2/*`
* **Per-version policies**:

  * Rate limits
  * Auth scopes
  * SLAs
* Mandatory **consumer identification** (API keys / OAuth client IDs)

**Why this matters**
You can *prove* who is using which version at any time.

---

### Mediator / Anti-Corruption Layer (Compatibility Pattern)

Implement a **Mediator (Anti-Corruption Layer)** between versions:

* v1 → mediator → v2
* No v1 logic lives in core domain services
* Mediator is:

  * Temporary
  * Clearly marked as deprecated
  * Cheap to remove later

This prevents:

* Domain pollution
* Parallel business logic forks
* Long-term tech debt

---

### Formal Deprecation Lifecycle (Governance)

Define and enforce a **non-negotiable lifecycle**:

| Phase          | Behavior                  |
| -------------- | ------------------------- |
| **Announced**  | Deprecation headers added |
| **Deprecated** | Still fully functional    |
| **Warned**     | Warning responses + logs  |
| **Limited**    | Reduced rate limits       |
| **Sunset**     | Endpoint disabled         |

---

### Sunset & Deprecation Headers (RFC-Based)

All deprecated endpoints must include:

```http
Deprecation: true
Sunset: Wed, 31 Jul 2025 23:59:59 GMT
Link: <https://api.example.com/docs/migration>; rel="deprecation"
```

This ensures:

* Machine-readable warnings
* Visibility in client logs
* Legal and contractual clarity

---

### Contract-First APIs + Consumer Tracking

* APIs defined via **OpenAPI (contract-first)**
* Version compatibility tested automatically
* Track:

  * Which consumer uses which version
  * Last seen timestamp
  * Migration status

Before sunsetting:

* **100% confirmation of external readiness**
* OR executive sign-off on breaking change

---

### Versioning Strategy: Major Versions Only

Avoid “v1.1, v1.2” externally.

Use:

* **v1, v2, v3**
* Minor changes must be backward compatible
* Breaking changes = new major version

This keeps client logic simple and expectations clear.

---

## 6. How This Prevents the Incident Entirely

With this architecture:

* The partner’s usage would have been **visible weeks earlier**
* Sunset warnings would appear in their logs
* Rate limits or warnings would surface before breakage
* v1 traffic could never silently degrade production systems
* Removal would be a **controlled, auditable decision**

In other words:
**This incident becomes impossible to repeat accidentally.**

---

## 7. Executive-Level Summary (Interview Closer)

> *“In the short term, I stabilize the partner using gateway-level routing and a mediator to restore compatibility without rolling back v2. Long-term, I enforce API versioning through gateway policies, observable deprecation lifecycles, Sunset headers, and consumer-tracked contracts. This turns API deprecation from a documentation exercise into an enforceable architectural guarantee.”*

