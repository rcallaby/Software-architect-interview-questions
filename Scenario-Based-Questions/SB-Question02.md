# SB-Question 02: Your high-traffic e-commerce platform, built on an event-driven, microservices architecture using AWS Lambda and DynamoDB, is facing a 35% budget cut next quarter. The CTO has given you a non-negotiable mandate to hit the new cost target without compromising 99.9% uptime or adding significant operational overhead. Currently, your daily cost is dominated by Lambda execution time and DynamoDB RCU/WCU provisioning. How do you approach this, what specific architectural changes do you investigate, and what is your proposed phased rollback plan if your changes introduce instability?

## Answer: Below is a **senior/principal-level interview answer** that mirrors the structure, depth, and clarity of the previous response. It is pragmatic, AWS-specific, and explicitly balances **cost reduction, reliability, and rollback safety**.

## 1. Reframe the Problem (Architect’s First Move)

This is not a generic “cut costs” exercise. The constraints are explicit:

* **35% cost reduction** (hard target)
* **99.9% uptime is non-negotiable**
* **No significant operational overhead**
* Cost drivers are **Lambda execution time** and **DynamoDB RCU/WCU**

So the correct mindset is:

> *Reduce waste and over-provisioning first, then optimize architecture only where ROI is provable.*

I would **not** start by rewriting services or changing databases.

---

## 2. Establish a Cost & Reliability Baseline (Week 0)

Before changing architecture, I’d establish **measurable baselines**:

### Metrics I need immediately

* Lambda:

  * Average execution time
  * Memory size vs actual memory used
  * Invocation count per event type
  * Cold start frequency
* DynamoDB:

  * Table-level RCU/WCU utilization
  * Hot partition metrics
  * Access patterns (point reads vs scans)
* Reliability:

  * p95/p99 latency
  * Error rates
  * DLQ growth

This gives:

* A **targeted optimization plan**
* A **rollback comparison point**

---

## 3. Primary Cost-Reduction Levers (Ordered by ROI)

### 3.1 Lambda: Execution Time & Memory Tuning (Low Risk, High Return)

#### Observation

Lambda cost = **memory × duration × invocations**

Most systems:

* Over-allocate memory
* Run CPU-bound code inefficiently
* Perform unnecessary synchronous work

#### Actions

* **Right-size memory** using AWS Lambda Power Tuning
* Identify CPU-bound Lambdas and:

  * Increase memory slightly to reduce execution time
* Remove:

  * Excessive logging
  * Blocking SDK retries
  * Synchronous fan-out calls

Typical savings: **15–25% on Lambda spend alone**

No architecture change, no reliability risk.

---

### 3.2 DynamoDB: Shift from Provisioned to On-Demand (or Hybrid)

#### Observation

Provisioned RCU/WCU is often sized for peak but used at 30–40%.

#### Actions

* Convert **spiky or unpredictable tables** to **On-Demand**
* Keep **steady, high-throughput tables** provisioned but:

  * Reduce baseline
  * Enable auto-scaling with tighter thresholds

Why this works:

* E-commerce traffic is bursty
* On-Demand trades predictability for cost efficiency

Typical savings: **10–20% DynamoDB cost**

---

## 4. Architectural Optimization (Targeted, Not Rewrites)

### 4.1 Event Fan-Out: Introduce SQS Buffering

#### Current (Common Anti-Pattern)

* One event → multiple Lambdas triggered directly (SNS/EventBridge)
* All execute synchronously

#### Change

* Insert **SQS between event producer and consumers**

Benefits:

* Natural throttling
* Batch processing
* Reduced invocation count
* Graceful backpressure

This reduces:

* Lambda invocations
* DynamoDB hot partition spikes

Cost reduction without uptime risk.

---

### 4.2 Batch Writes to DynamoDB

#### Observation

Many Lambdas:

* Write one item per invocation
* Pay WCU overhead repeatedly

#### Change

* Aggregate events via SQS
* Use `BatchWriteItem`

Benefits:

* Lower WCU usage
* Fewer write calls
* Better partition distribution

This is invisible to upstream systems.

---

## 5. Strategic Optimization (Medium Risk, High Impact)

### 5.1 Collapse “Chatty” Lambdas

#### Problem

Microservices taken too literally:

* Multiple Lambdas calling each other
* Paying invocation + network cost each time

#### Solution

* Merge tightly-coupled Lambdas into a **single execution unit**
* Preserve logical separation in code, not runtime

This:

* Reduces invocations
* Cuts execution latency
* Maintains service boundaries at API level

Often responsible for **10–15% additional savings**

---

## 6. What I Would Explicitly Avoid

To respect the constraints:

* ❌ Migrating away from DynamoDB
* ❌ Introducing EC2/ECS unless absolutely necessary
* ❌ Rewriting business logic
* ❌ Custom caching layers that add ops burden

---

## 7. Rollback Strategy (Non-Negotiable)

Every change is deployed behind **reversibility**.

---

### Phase 1 Rollback: Configuration-Level

* Lambda memory settings → revert via IaC
* DynamoDB billing mode → instant rollback
* SQS batch size → reduce to 1

Rollback time: **minutes**

---

### Phase 2 Rollback: Traffic Shaping

* Feature flags to:

  * Bypass batching
  * Re-enable direct event triggers
* Weighted routing (EventBridge rules)

⏱ Rollback time: **minutes**

---

### Phase 3 Rollback: Architectural Safety Nets

* DLQs monitored continuously
* Canary deployments for merged Lambdas
* CloudWatch alarms tied to:

  * Error rate
  * Throttles
  * Latency

⏱ Rollback time: **under 15 minutes**

---

## 8. Final Outcome

By combining:

* Execution efficiency
* DynamoDB billing optimization
* Event buffering
* Reduced invocation count

I would confidently hit:

* **35–45% cost reduction**
* **No degradation to 99.9% uptime**
* **No increase in operational complexity**

---

## 9. How I’d Close This in an Interview

> “I wouldn’t chase cost savings by destabilizing the architecture. I’d start by eliminating waste, then introduce buffering and batching where Lambda and DynamoDB costs compound. Every change would be reversible, measured, and rolled out incrementally. The goal isn’t cheaper infrastructure—it’s a cheaper system that behaves exactly the same under load.”


