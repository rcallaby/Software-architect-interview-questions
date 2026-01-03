# MA - Question 09 - Imagine we are building a new social media platform. How would you design the data model and architecture to support a highly-performant news feed for millions of users?

## Why Interviewers Ask This Question

This question is a classic **systems + data modeling** problem disguised as a product scenario.

Interviewers are evaluating whether you can:

1. **Reason at massive scale**
   Millions of users implies fan-out, hot keys, read/write asymmetry, and data locality issues.

2. **Model data around access patterns**
   News feeds are not about “perfect normalization”—they’re about **fast reads under heavy load**.

3. **Make trade-offs explicitly**
   Push vs pull models, consistency vs freshness, storage vs compute, simplicity vs flexibility.

4. **Connect architecture to user experience**
   Feed latency, ranking freshness, and pagination behavior directly affect engagement.

5. **Communicate clearly under ambiguity**
   There is no single “correct” feed design—only contextually correct ones.

Your job is not to invent Facebook in 45 minutes, but to demonstrate **structured thinking, prioritization, and judgment**.

---

## How to Structure a Strong Answer

A strong answer almost always follows this arc:

1. Clarify assumptions and requirements
2. Identify core access patterns
3. Design the data model around those patterns
4. Choose a feed generation strategy
5. Address scale, caching, and performance
6. Discuss trade-offs and evolution paths

This shows you think like an architect—not a feature implementer.

---

## Sample Interview Answer (Professional, Casual, Architect-Level)

> *“I’d start by grounding the design in how a news feed is actually consumed—because the data model should follow the read path, not the other way around.”*

### Step 1: Define the Core Requirements

For a highly-performant feed at scale, I’d assume:

* **Read-heavy workload** (far more feed reads than content writes)
* **Low-latency expectations** (sub-200ms for initial feed load)
* **Eventual consistency is acceptable**
* **Personalized ordering** (not just chronological)
* **Infinite scroll / pagination**

These assumptions strongly influence the architecture.

---

### Step 2: Identify the Core Entities

At a minimum, I’d model:

* **User**
* **Post / Content**
* **Follow / Graph relationship**
* **Feed entry**

Importantly, I would *not* try to dynamically join these at read time for feed generation at scale.

---

### Step 3: Feed Generation Strategy (Push vs Pull)

This is the key architectural decision.

#### Option 1: Pull Model (Fan-out on Read)

* On feed request, query posts from followed users
* Rank and paginate dynamically

**Pros**

* Simple write path
* Always fresh data

**Cons**

* Very expensive reads
* Poor performance at scale
* Doesn’t work well for users with many follows

This approach breaks down quickly at millions of users.

---

#### Option 2: Push Model (Fan-out on Write) — Preferred

* When a user creates a post, it is **pre-computed and written** into the feeds of their followers
* Each user has a **materialized feed table or feed cache**

**Pros**

* Extremely fast reads
* Predictable latency
* Scales well for read-heavy systems

**Cons**

* Higher write amplification
* Complex for users with millions of followers

Most large-scale social platforms use a **hybrid push model**.

---

### Step 4: Data Model (Optimized for Reads)

A simplified model might look like:

#### Feed Table (Denormalized)

```
FeedEntry {
  user_id
  post_id
  author_id
  timestamp
  ranking_score
}
```

* Stored in a **distributed NoSQL store** (e.g., DynamoDB, Cassandra)
* Partitioned by `user_id`
* Sorted by timestamp or ranking score

This allows:

* O(1) feed fetch
* Simple pagination
* Cache-friendly access patterns

---

### Step 5: Handling Scale & Hot Users

To handle celebrities or viral posts:

* **Hybrid fan-out**

  * Push to normal users
  * Pull on read for “high-fanout” accounts
* **Async feed workers**

  * Kafka / PubSub to process feed updates
* **Rate-limited or capped feed fan-out**

  * Only push top N posts per author

This prevents write storms while preserving read performance.

---

### Step 6: Caching & Performance

* **In-memory caching** (Redis / Memcached) for hot feeds
* **Feed pre-warming** for active users
* **CDN caching** for media assets
* **Pagination via cursors**, not offsets

The goal is predictable, low-latency reads under load.

---

### Step 7: Ranking & Evolution

Ranking logic is intentionally **decoupled** from storage:

* Ranking scores computed asynchronously
* ML models can evolve without changing the feed schema
* Feed entries can be re-ranked or re-written as models improve

This keeps the system flexible over time.

---

### Trade-Off Summary

| Decision             | Trade-Off                               |
| -------------------- | --------------------------------------- |
| Push-based feed      | Fast reads, expensive writes            |
| Denormalized feed    | Performance over storage efficiency     |
| Eventual consistency | User experience over strict correctness |
| Hybrid fan-out       | Complexity for scalability              |

---

### Decision Principle

> **For social feeds, I optimize relentlessly for read latency and predictability, even if it means higher write complexity and denormalized data.**

That’s the trade most large-scale consumer platforms make.

---

## Final Interview Coaching Tip

If you want to stand out, explicitly say something like:

> *“This design is intentionally not ‘perfectly normalized’ because feeds are fundamentally an access-pattern problem, not a relational modeling problem.”*


