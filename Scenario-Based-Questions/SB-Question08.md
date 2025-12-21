# SB Question 08: You are designing the architecture for a new internal service that manages all employee PII. This service will be accessed by other internal microservices and a select group of HR personnel. You operate in a Zero Trust environment. Detail the specific security mechanisms you will implement at each layer: Network, Service-to-Service Communication, and User Access. How do you ensure that a compromised microservice cannot use its token to pivot and access other restricted data within the environment?

## Answer:

### How to Approach This Question (Architect Mindset)

This question is **not about listing security tools**.

What the interviewer is really evaluating:

1. **Do you understand Zero Trust as a system, not a product?**
2. **Can you reason in layers and failure modes?**
3. **Can you design for compromise, not prevention?**
4. **Do you understand blast radius reduction?**
5. **Can you explain security in a structured, defensible way?**

The most important sentence in the question is:

> *“How do you ensure that a compromised microservice cannot use its token to pivot and access other restricted data?”*

This explicitly tests:

* Identity scoping
* Token design
* Authorization boundaries
* Defense-in-depth

---

# Step 1: Reframe the Problem Correctly

This is not “how do we protect PII?”

This is:

> *“Assuming something will be compromised, how do we limit damage and prove trust continuously?”*

Zero Trust means:

* No implicit trust based on network location
* Every request is authenticated, authorized, and evaluated
* Identity > network
* Least privilege everywhere

---

# Step 2: Define the Security Model Before Layers

Before jumping into layers, a senior architect **defines the governing principles**:

### Governing Security Principles

* **Explicit Identity Everywhere** (users *and* services)
* **Least Privilege by Default**
* **Short-lived Credentials**
* **Strong Isolation of PII**
* **Auditability and Non-Repudiation**
* **Assume Breach**

These principles guide every technical choice.

---

# Step 3: Layered Security Design

We now walk **top-down**, explicitly calling out **mechanisms and reasoning**.

---

## 1. Network Layer (Transport & Infrastructure Security)

### Goal

Reduce exposure surface and prevent lateral movement.

### Mechanisms

#### a. Private Network Segmentation

* PII service deployed in a **dedicated, isolated subnet**
* No public ingress
* No direct access from non-approved workloads

#### b. Default-Deny Network Policies

* Kubernetes NetworkPolicies or cloud-native equivalents
* Explicit allow-lists:

  * Only approved service identities can reach the PII service
  * No wildcard or namespace-wide access

#### c. East-West Traffic Control

* All internal traffic routed through a **service mesh** or controlled gateway
* No “flat” internal network

### Why This Matters

Even if a service is compromised:

* It cannot *see* or *reach* unrelated services
* Network isolation becomes the first blast-radius boundary

---

## 2. Service-to-Service Communication (Machine Identity & Authorization)

This is the **most critical layer**.

### Goal

Ensure services are authenticated, authorized, and narrowly scoped — and cannot reuse credentials to pivot.

---

### a. Strong Workload Identity (No Shared Secrets)

* Each microservice has a **unique cryptographic identity**
* Identity derived from:

  * SPIFFE/SPIRE
  * Cloud IAM workload identity
  * mTLS certificates

No API keys. No shared secrets. No long-lived tokens.

---

### b. Mutual TLS (mTLS) Everywhere

* Every service-to-service request:

  * Authenticates both client and server
  * Encrypts traffic
* Certificates:

  * Short-lived
  * Automatically rotated
  * Bound to service identity

This prevents:

* Impersonation
* Token replay from outside approved contexts

---

### c. Fine-Grained Authorization (Not Just Authentication)

Authentication proves *who you are*.
Authorization proves *what you can do*.

Examples:

* Service A can read **only specific PII fields**
* Service B can write **only non-sensitive metadata**
* Service C has **no access at all**

Implemented via:

* Policy engines (OPA / Cedar / Zanzibar-style ACLs)
* Attribute-based access control (ABAC)
* Explicit service-to-endpoint mapping

---

### d. Token Scope, Audience & Context Binding

This directly answers the pivoting question.

Each service token is:

* **Audience-restricted** (can only call the PII service)
* **Scope-limited** (specific endpoints, actions, and data types)
* **Context-bound** (service name, environment, namespace)

Even if stolen:

* Token is useless outside its narrow context
* Cannot be replayed against other services

---

### e. Short-Lived Tokens

* Lifetimes measured in minutes
* Automatic rotation
* Revocation via identity provider

This limits the usefulness of compromised credentials.

---

## 3. User Access Layer (Human Access to PII)

### Goal

Protect against insider threats and credential compromise.

---

### a. Centralized Identity Provider (SSO)

* All HR users authenticate via:

  * Enterprise IdP (Okta, Azure AD, etc.)
* No local user accounts

---

### b. Strong Authentication

* Mandatory MFA
* Conditional access policies:

  * Device posture
  * Location
  * Time-based access

---

### c. Role-Based & Attribute-Based Access Control

* HR roles defined explicitly:

  * Payroll
  * Benefits
  * Compliance
* Access limited by:

  * Job function
  * Data sensitivity
  * Jurisdiction (GDPR, etc.)

---

### d. Just-In-Time (JIT) Access

* No standing admin privileges
* Temporary elevation:

  * Requires approval
  * Automatically expires

---

### e. Full Auditability

* Every PII access is logged:

  * Who
  * What
  * When
  * Why
* Logs are:

  * Immutable
  * Monitored
  * Retained per compliance requirements

---

# Step 4: Explicitly Answer the Pivoting Question

> **“How do you ensure that a compromised microservice cannot use its token to pivot?”**

### The Architectural Answer

We assume compromise and design for containment:

1. **Identity-bound tokens**

   * Tokens are tied to a specific service identity

2. **Audience-restricted tokens**

   * Tokens only valid for a single target service

3. **Fine-grained authorization**

   * Even valid tokens can only access explicitly permitted data

4. **Short-lived credentials**

   * Compromise window is small

5. **Network isolation**

   * Token cannot be used to reach unauthorized services

6. **Continuous verification**

   * Every request is re-validated, not trusted implicitly

Result:

> A compromised service can only do exactly what it was already allowed to do — nothing more.

That is Zero Trust done correctly.

---

# Step 5: How to Communicate This in an Interview

### What This Demonstrates

✔ Zero Trust maturity
✔ Defense-in-depth thinking
✔ Blast-radius reduction
✔ Identity-first security design
✔ Real-world compromise awareness

This is **architectural security**, not checkbox security.

---

# Final Executive-Level Summary

> The system is designed under the assumption of breach. By combining strong workload identity, mTLS, fine-grained authorization, short-lived scoped tokens, network isolation, and strict user access controls, we ensure that both human and machine access to PII is continuously verified and narrowly constrained. Compromise of a single service does not result in lateral movement or data exfiltration beyond its explicitly authorized scope.
