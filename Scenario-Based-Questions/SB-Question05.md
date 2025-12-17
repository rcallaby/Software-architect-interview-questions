# SB Question 05: During pre-launch load testing, your system met the 10,000 requests per second (RPS) target. However, when the load was increased to 50,000 RPS (a 5x spike), the system didn't just slow down—it completely cascaded into failure across multiple services, despite significant auto-scaling being configured. Outline your systematic investigation plan (the 3 steps you take first) to find the bottleneck. What are the three most common architectural suspects for a non-linear collapse under extreme load, and what is the typical remedy for each?

## Answer:

## 1. Reframing the Problem (Architect’s Perspective)

This is **not a scaling problem in isolation**—it is a **non-linear systemic failure**.

The key signals in the question are:

* The system passed **10,000 RPS**
* A **5× increase** caused a **full cascade**
* **Auto-scaling was already enabled**
* Multiple services failed simultaneously

This immediately tells me:

* The bottleneck is **not CPU alone**
* Auto-scaling **increased pressure somewhere else**
* A shared dependency or control plane collapsed

The architect’s job is to **identify the first domino**, not the last failure.

---

## 2. Immediate Goal of the Investigation

The first objective is **not optimization**.

The objective is to answer three questions, in order:

1. *What failed first?*
2. *Why did scaling amplify the failure instead of absorbing it?*
3. *Which shared constraint turned linear load into exponential damage?*

---

## 3. Systematic Investigation Plan — The First 3 Steps

### Step 1: Identify the First Failing Component (Timeline Reconstruction)

I begin by reconstructing a **precise failure timeline** using:

* Distributed traces
* Per-service error rates
* Queue depths
* Connection pool metrics
* Retry counts

I’m looking for:

* The **first service** whose latency spiked
* The **first dependency** to hit a hard limit
* The point where retries began amplifying load

> Cascades never start everywhere at once. They always start somewhere.

---

### Step 2: Compare 10k vs 50k Load Characteristics (Non-Linear Triggers)

Next, I compare **behavioral differences**, not just throughput:

* Did connection counts explode?
* Did retry rates increase?
* Did timeouts align across services?
* Did queues switch from draining to growing?

Key insight:

> A 5× RPS increase often produces a **25× increase** in contention.

This is where non-linear constraints reveal themselves.

---

### Step 3: Verify Control Planes and Shared Resources

Finally, I inspect **everything auto-scaling does *not* scale**:

* Databases
* Caches
* Message brokers
* Rate-limited APIs
* Thread pools
* Network bandwidth
* Authentication systems

This step usually reveals:

* A shared choke point
* A resource with a **hard ceiling**
* Or a feedback loop created by retries

---

## 4. The Three Most Common Architectural Suspects

### Database Connection Exhaustion (Most Common)

#### Why It Causes Collapse

* Each auto-scaled instance opens connections
* Databases have **hard max connection limits**
* Once exhausted:

  * Requests block
  * Timeouts occur
  * Retries multiply
  * Latency explodes system-wide

This creates a **death spiral**, not a slowdown.

#### Typical Remedy

* Introduce **connection pooling at the application layer**
* Use **database proxying** (e.g., PgBouncer, RDS Proxy)
* Enforce:

  * Per-service connection caps
  * Backpressure instead of retries
* Scale **read replicas**, not writers

> Databases scale *vertically* and *carefully*—never like stateless services.

---

### Retry Storms & Missing Backpressure

#### Why It Causes Collapse

At high load:

* Timeouts increase
* Clients retry aggressively
* Services retry downstream calls
* Load multiplies internally

Auto-scaling adds **more retrying clients**, not relief.

This is the classic **thundering herd** problem.

#### Typical Remedy

* Enforce:

  * Bounded retries
  * Exponential backoff with jitter
* Implement:

  * Circuit breakers
  * Bulkheads per dependency
* Prefer **fast failure** over slow saturation

> A system that retries without limits will self-DoS under pressure.

---

### Queue Saturation or Thread Pool Starvation

#### Why It Causes Collapse

Many systems rely on:

* Message queues
* Async task executors
* Thread pools

Under extreme load:

* Queues fill faster than they drain
* Threads block on I/O
* Latency increases
* Upstream services time out and retry

Once queues stop draining, **everything upstream collapses**.

#### Typical Remedy

* Size queues based on **worst-case backlog**, not average
* Apply:

  * Backpressure
  * Drop or degrade low-priority traffic
* Separate:

  * CPU-bound and I/O-bound thread pools
* Add **load shedding**, not infinite buffering

> A queue without backpressure is a memory leak with better marketing.

---

## 5. Why Auto-Scaling Didn’t Save You

Auto-scaling:

* Scales **stateless compute**
* Does **not** scale:

  * Databases
  * Queues
  * Network limits
  * External APIs
  * Control planes

In this incident:

* Scaling **amplified contention**
* Increased parallelism overwhelmed shared resources
* Turned latency into cascading failure

---

## 6. Long-Term Architectural Guardrails

To prevent recurrence, I would mandate:

* **Load testing beyond target** (10×, not 1×)
* Failure injection (timeouts, slow DB, partial outages)
* Capacity modeling for shared dependencies
* Explicit **backpressure policies**
* Per-service concurrency limits
* Retry budgets instead of unlimited retries

---

## 7. Executive-Level Summary (Interview Closer)

> *“When a system collapses under load instead of slowing down, I assume a shared constraint or feedback loop. I first identify the initial failure via traces and metrics, compare non-linear behaviors between load levels, and inspect shared dependencies that auto-scaling can’t fix. In practice, the usual culprits are database connection exhaustion, retry storms, or queue saturation—each solved through pooling, backpressure, and bounded concurrency. Auto-scaling without guardrails amplifies failure; resilience comes from architectural limits, not elasticity alone.”*

