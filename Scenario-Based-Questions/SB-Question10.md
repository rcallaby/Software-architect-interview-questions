# SB Question 10: 

## Answer:

### How to Approach This Question (Architect Mindset)

This question is **not about microservices vs monoliths**.

It is a test of whether you can:

1. **Diagnose organizational pathologies through architecture**
2. **Recognize Conway’s Law in action**
3. **Use data—not opinion—to justify architectural change**
4. **Balance technical refactoring with human and political realities**
5. **Restore delivery velocity without causing cultural collapse**

The phrase that matters most:

> *“The ‘Distributed Monolith’ has become a reality.”*

This signals:

* Architectural over-fragmentation
* High coordination cost
* Low autonomy
* False sense of scalability

---

# Step 1: Reframe the Problem Correctly

This is **not** a “we have too many services” problem.

This is:

> *“Our architecture no longer matches our team size, cognitive capacity, or rate of change.”*

With:

* 150 services
* 40 engineers
* ~1 engineer per 4 services

The system is mathematically unownable.

Microservices only work when:

* Teams are large enough to own them
* Services change independently
* Deployment is autonomous

None of those are true here.

---

# Step 2: Define the Goal of Service Consolidation

A senior architect does **not** say “merge everything.”

The goal is:

> **Restore independent deployability, reduce coordination cost, and align architecture with team reality.**

This means moving toward **Macro-services**:

* Larger deployment units
* Clear domain boundaries
* Fewer runtime hops
* Fewer handoffs

---

# Step 3: Establish the Decision Framework (Metrics First, Opinions Last)

You **must** lead with metrics to avoid endless debate.

---

## Core Metrics to Drive Consolidation Decisions

### 1. **Change Coupling (Most Important Metric)**

#### What It Measures

* How often two services change together

#### How to Measure

* Git history analysis:

  * % of commits touching multiple services
  * Repeated co-changes over time

#### Interpretation

* If Service A and Service B change together >70% of the time:

  * They are not independent
  * They are one service pretending to be two

---

### 2. **Deployment Frequency & Coordination Cost**

#### What It Measures

* How often services can be deployed independently

#### Signals of Failure

* Coordinated releases
* Slack messages like:

  * “Please don’t deploy until…”
  * “We need to wait for Team X”
* Manual sequencing of deployments

#### Interpretation

* Services that cannot be deployed alone are **organizationally coupled**
* They should be merged

---

### 3. **Team Cognitive Load**

#### What It Measures

* How much mental overhead teams carry

#### Indicators

* Number of services a team “owns”
* Number of dependencies per service
* On-call fatigue
* Bus factor

#### Interpretation

* If a team cannot explain their service boundaries clearly:

  * The architecture is too fragmented
* Cognitive load is a hard scalability limit

---

### 4. **Runtime Chattiness & Failure Cascades**

#### What It Measures

* Synchronous call chains
* Blast radius during incidents

#### Red Flags

* 5–10 service call chains
* Retries amplifying failures
* “One service down = partial outage”

#### Interpretation

* These services form a *runtime monolith*
* They belong together

---

# Step 4: The Consolidation Strategy (How You Actually Do This)

This is where many candidates fail.

---

## Phase 1: Freeze Service Creation

* No new microservices without architectural review
* Forces discipline
* Stops further fragmentation

---

## Phase 2: Identify Consolidation Candidates

Using the metrics above, you identify:

* “Service clusters” that:

  * Change together
  * Deploy together
  * Fail together
  * Are owned by the same teams

These become **Macro-service candidates**.

---

## Phase 3: Define Clear Domain Boundaries

This is **Domain-Driven Design applied pragmatically**.

* Align services to:

  * Business capabilities
  * Not technical layers
* Each Macro-service:

  * Owns its data
  * Exposes a stable API
  * Can be deployed independently

---

## Phase 4: Merge Strategically (Not Big-Bang)

* Start with:

  * The most painful, highest-coupling clusters
* Merge incrementally:

  * Keep APIs stable
  * Internally consolidate logic
* Measure improvements:

  * Deployment speed
  * Incident frequency
  * Team satisfaction

---

# Step 5: Handling the Political Fallout (This Is Where Seniority Shows)

This is **not optional** to address.

---

## Understand the Root Cause of Resistance

Teams resist because:

* Ownership equals status
* Microservices feel like autonomy
* People fear loss of control

You do **not** win by arguing architecture.

---

## Reframe the Narrative

You say:

> “We are not taking services away. We are giving teams back their time.”

Key messages:

* Fewer services = more impact
* Larger scope = clearer ownership
* Less coordination = more autonomy

---

## Change the Incentives

* Measure teams on:

  * Lead time
  * Reliability
  * Customer impact
* Not number of services owned

Ownership of outcomes > ownership of artifacts.

---

## Involve Teams in the Decision

* Use metrics transparently
* Let teams:

  * Validate data
  * Propose consolidation plans
* This turns resistance into authorship

---

# Step 6: Executive Framing (Critical for Buy-In)

To leadership, you say:

> “Our architecture scaled beyond our organization. Service consolidation is not a step backward — it’s a correction that restores speed, reliability, and accountability.”

---

# Final Interview-Ready Summary

> The distributed monolith is a symptom of premature microservice decomposition. By using objective metrics such as change coupling, deployment independence, cognitive load, and runtime coupling, we can identify which services should be consolidated into macro-services. This reduces coordination cost, restores team autonomy, and improves delivery velocity. Success depends as much on managing organizational incentives and ownership culture as on the technical consolidation itself.
