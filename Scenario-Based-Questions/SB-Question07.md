# SB Question 07: Your engineering team can build an in-house document management and search service in six months. Alternatively, you can integrate a leading commercial third-party solution (like Elasticsearch/Box/SharePoint) in four weeks. The third-party option has a recurring monthly cost of $15,000. Detail the three most critical, non-technical factors (e.g., vendor lock-in, data security, TCO) you would use to drive the final decision. Frame your recommendation to the CEO not just on cost, but on long-term competitive advantage and business agility.

## Answer:

# How to Approach This Question (Architect Mindset)

Before answering, understand **what the interviewer is really testing**:

1. **Can you separate technical feasibility from business impact?**
   Both options are technically viable. The decision hinges on *business strategy*.

2. **Can you reason beyond upfront cost?**
   This is explicitly a *non-technical* tradeoff question.

3. **Can you communicate at the executive level?**
   The CEO doesn’t care about APIs or schemas — they care about *risk, speed, leverage, and advantage*.

4. **Can you create a decision framework that scales?**
   They want to see *repeatable thinking*, not a one-off opinion.

---

# Step 1: Reframe the Problem Correctly

This is **not** a “build vs buy” technical debate.

This is a decision about:

* **Focus**
* **Risk allocation**
* **Organizational leverage**
* **Time-to-value**
* **Strategic control**

The correct framing:

> *“Are we a company whose competitive advantage depends on owning document management and search, or are we better served by outsourcing this capability and investing our engineering effort elsewhere?”*

Once framed this way, the answer becomes clearer.

---

# Step 2: Identify the Three Most Critical *Non-Technical* Factors

Many candidates mention *too many* factors. Senior architects **prioritize**.

The three most critical non-technical factors here are:

---

## 1. **Total Cost of Ownership (TCO) Over Time — Not Just Monthly Spend**

### Why This Matters

The $15,000/month ($180,000/year) cost is **deceptively simple**. The real question is:

> *What is the fully loaded cost of building, operating, securing, evolving, and supporting an in-house system over 3–5 years?*

### In-House Reality (Often Underestimated)

* 6 months of engineering (opportunity cost)
* Ongoing:

  * On-call support
  * Incident response
  * Security patching
  * Compliance audits
  * Feature requests from internal teams
* Staff churn risk (knowledge loss)
* Scaling costs (infra + people)

A conservative estimate:

* 2–3 engineers ongoing
* At $180–220k fully loaded per engineer
* **$360k–660k per year**, *before infrastructure*

### Third-Party Reality

* Predictable cost
* Cost scales with usage, not headcount
* Vendor absorbs:

  * Security updates
  * Reliability engineering
  * Compliance certifications

### Architect Conclusion

TCO almost always favors **buy** unless:

* The capability is revenue-generating **or**
* It materially differentiates the product

---

## 2. **Strategic Focus & Opportunity Cost (Competitive Advantage)**

### This Is the Most Important Factor

Every engineering hour spent on internal document management is an hour **not spent** on:

* Core product differentiation
* Customer-facing features
* Speed to market
* Innovation

### Key Question for the CEO

> *“Does owning document management make us more competitive in our market?”*

If the answer is **no**, then:

* Building it is *organizational drag*
* Engineers become infrastructure caretakers
* Innovation slows

### Competitive Reality

Companies rarely win because their:

* Search index is better
* File storage UI is custom-built
* Permission system is homegrown

They win because they:

* Ship faster
* Adapt quicker
* Focus narrowly on what customers pay for

### Architect Insight

**Buying commoditized capabilities is a strategic accelerator**, not a weakness.

---

## 3. **Risk Allocation & Business Agility (Including Vendor Lock-In)**

Vendor lock-in is often raised — but **senior architects treat it realistically**, not emotionally.

### Real Risks of Vendor Lock-In

* Pricing increases
* Feature roadmap dependency
* Migration complexity

### Counterpoint (Often Missed)

**You are also locked into your own code**:

* Legacy decisions
* Poor early abstractions
* Engineers who leave
* Technical debt you must own forever

### Smart Mitigation Strategy

Instead of rejecting vendors:

* Use **contractual safeguards**
* Design **exit paths**
* Abstract integration points
* Retain data portability guarantees

This is *risk management*, not risk avoidance.

### Agility Comparison

| Factor               | In-House          | Third-Party      |
| -------------------- | ----------------- | ---------------- |
| Time to Value        | 6+ months         | 4 weeks          |
| Feature Evolution    | Slow              | Continuous       |
| Compliance Readiness | Internal burden   | Vendor-managed   |
| Scaling              | Engineering-heavy | Mostly automatic |

### Architect Conclusion

Third-party solutions **increase organizational agility** — which is a competitive advantage in itself.

---

# Step 3: CEO-Level Recommendation (How to Present It)

This is how you frame it **to a CEO**, not an engineering manager.

---

## Executive Recommendation Summary

> **Recommendation:** Integrate a leading third-party document management and search solution now, with architectural safeguards for future optionality.

### Rationale (CEO Language)

1. **Speed to Market**

   * 4 weeks vs 6 months is a material advantage
   * Enables faster delivery of customer-facing value

2. **Focus on Core Differentiation**

   * Frees engineering to work on features that drive revenue and retention
   * Avoids long-term maintenance drag

3. **Predictable Economics**

   * Known monthly cost
   * Avoids hidden operational and staffing expenses

4. **Managed Risk**

   * Enterprise-grade security and compliance
   * Contractual and architectural mitigations for vendor dependence

---

## Strategic Safeguards I Would Put in Place

To show maturity, you *must* mention this.

* Data ownership guarantees in contract
* Export and migration clauses
* API-based abstraction layer
* Periodic vendor review (cost, roadmap, risk)
* Internal metrics to reassess build-vs-buy annually

This signals **long-term thinking**, not blind optimism.

---

# Final “Gold Standard” Answer Summary

> We should integrate the third-party solution because document management and search are commoditized capabilities that do not differentiate our business. Buying allows us to move faster, reduce operational risk, and concentrate engineering effort on competitive advantages. The recurring cost is outweighed by opportunity cost, predictable TCO, and increased agility. With proper contractual and architectural safeguards, this choice maximizes long-term business flexibility while preserving future optionality.

