# SB Question 06: You've inherited a system where developers rely heavily on logs to diagnose production issues, and the alerting is based purely on resource consumption (CPU > 80%). This leads to slow diagnosis and alerts that don't reflect user experience. Design a complete, modern observability strategy for a new critical service. Detail the three 'Golden Signals' you will prioritize for alerting, and provide a concrete example of a Service Level Objective (SLO) that directly measures user experience and triggers a PagerDuty alert

## Answer:

## 1. Reframing the Problem (Architect’s Perspective)

The current system exhibits **classic “log-centric observability” failure modes**:

* Logs are used as the **primary diagnostic tool**
* Alerts are driven by **resource metrics**, not user impact
* Engineers learn about incidents **after users are already affected**
* High CPU does not correlate with poor experience, and poor experience can occur with normal CPU

From an architectural standpoint, this is not an observability problem—it is a **feedback-loop problem**.

The goal of a modern observability strategy is simple:

> **Detect user-visible degradation early, explain it quickly, and act only when it matters.**

---

## 2. Observability Design Goals for a Critical Service

Before selecting tools or metrics, I define **explicit objectives**:

1. **User experience drives alerts**
2. **Symptoms trigger investigation, not noise**
3. **Logs explain *why*, metrics detect *when*, traces show *where***
4. **Alert fatigue is treated as a production risk**

This frames observability as **a system, not a dashboard**.

---

## 3. Complete Observability Strategy (Modern Stack)

### Metrics: Detecting User-Visible Problems

Metrics are the **primary alerting signal**.

For a new critical service, I prioritize **SLI-driven metrics**, not infrastructure metrics.

Collected at:

* Service boundary (ingress)
* Per endpoint
* Per customer tier (if applicable)

---

### Distributed Tracing: Explaining Latency & Failure Paths

Tracing is mandatory from day one:

* End-to-end request tracing
* Service-to-service hops
* Downstream dependency timing
* Error propagation visibility

Tracing answers:

> *Which dependency caused this user-visible failure?*

---

### Structured Logging: Context, Not Volume

Logs are:

* Structured (JSON)
* Correlated via trace and request IDs
* Sampled aggressively under load

Logs are **diagnostic artifacts**, not alert triggers.

---

## 4. The Three Golden Signals (What I Alert On)

For alerting, I focus on **service-level Golden Signals**, not host metrics.

---

### 1. Latency (User-Perceived Performance)

**What I measure**

* p95 and p99 response times
* Measured at the API boundary
* Broken down by critical endpoint

**Why it matters**

* Averages hide pain
* Tail latency reflects real user experience

---

### 2. Errors (Correctness)

**What I measure**

* Error rate as a percentage of total requests
* 5xx responses
* Failed business operations (not just HTTP errors)

**Why it matters**

* Users care whether the action worked
* Silent failures are worse than crashes

---

### 3. Traffic / Saturation (Demand vs Capacity)

**What I measure**

* Request throughput
* Concurrency
* Queue depth
* Dependency saturation (DB, cache, external APIs)

**Why it matters**

* Explains *why* latency or errors are increasing
* Predicts imminent failure before users feel it

> CPU and memory are *supporting signals*, not alert triggers.

---

## 5. What I Explicitly Do *Not* Alert On

* CPU spikes
* Memory pressure
* Disk I/O
* GC pauses (unless they cause latency or errors)

These are:

* **Debugging signals**
* **Not paging signals**

---

## 6. Concrete Example: User-Centric SLO

### Service Context

A critical **Checkout API** for a mobile and web application.

---

### Service Level Indicator (SLI)

> **99% of checkout requests must complete successfully within 300ms**

Measured as:

* HTTP 2xx responses
* End-to-end latency
* From user request to final response

---

### Service Level Objective (SLO)

```text
99.0% of /checkout requests complete in <300ms over a rolling 30-day window
```

---

### Error Budget

```text
1% failure budget (latency or errors) over 30 days
```

This budget:

* Quantifies acceptable risk
* Enables trade-offs between velocity and reliability

---

## 7. PagerDuty Alerting Policy (Concrete)

### Page Only When:

* Error budget burn rate exceeds **2×** over **10 minutes**
* OR p99 latency exceeds **300ms** for **5 consecutive minutes**

Example alert condition:

```text
IF
  p99 latency > 300ms
  AND
  request success rate < 99%
  FOR 5 minutes
THEN
  Trigger PagerDuty
```

---

### Why This Works

* Pages only when users are impacted
* Ignores transient spikes
* Prevents alert fatigue
* Aligns on-call stress with business impact

---

## 8. Incident Workflow (How Engineers Use This)

1. **PagerDuty fires** → user experience is degraded
2. **Dashboard shows** which Golden Signal is violated
3. **Traces identify** the slow or failing dependency
4. **Logs explain** the root cause with context
5. **Fix is applied** with confidence

Mean Time To Resolution (MTTR) drops dramatically.

---

## 9. Long-Term Observability Governance

I enforce the following policies:

* Every service must declare:

  * SLIs
  * SLOs
  * Error budgets
* Alerts require:

  * A runbook
  * A clear user impact statement
* Logs must be:

  * Structured
  * Correlated
  * Sampled

Observability becomes **part of the architecture**, not an afterthought.

---

## 10. Executive-Level Summary (Interview Closer)

> *“I design observability around user experience, not infrastructure metrics. I alert on the Golden Signals—latency, errors, and saturation—measured at the service boundary. Logs and traces explain failures, but SLO-based alerts determine when humans are paged. By tying PagerDuty directly to error-budget burn, we eliminate noise, reduce MTTR, and ensure engineers respond only when users are actually impacted.”*
