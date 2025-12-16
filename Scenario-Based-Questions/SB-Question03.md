# SB Question 03: Your SaaS product is entering the European market. The product currently uses a single US-based PostgreSQL database cluster. New GDPR and data sovereignty laws require customer data to remain within the EU region. You must maintain a unified application codebase and provide near-real-time synchronization of non-sensitive metadata (e.g., user IDs, subscription status) globally, while ensuring sensitive customer data (PII) is strictly geo-fenced. How do you design the database tier, and which specific replication and partitioning strategies (e.g., Sharding, Geo-partitioning, Read Replicas) will you employ to meet these dual requirements?

## Answer:

## 1. Start with the Core Constraint Framing (What the Interviewer Is Testing)

This problem is fundamentally about:

* **Data classification** (PII vs non-PII)
* **Data locality / sovereignty**
* **Consistency boundaries**
* **Avoiding a forked codebase**
* **Operational simplicity under regulation**

The key insight is:

> **You cannot solve this with a single global database or naive replication.
> You must explicitly separate data domains and align them with geography.**

---

## 2. High-Level Database Tier Architecture

### Logical View

```
                        ┌────────────────────┐
                        │ Global Metadata DB │
                        │ (Non-PII Only)     │
                        └─────────┬──────────┘
                                  │
               Near-real-time logical replication
                                  │
          ┌───────────────────────┴───────────────────────┐
          │                                                   │
┌────────────────────┐                        ┌────────────────────┐
│ US Region          │                        │ EU Region          │
│ PostgreSQL Cluster │                        │ PostgreSQL Cluster │
│ (PII + local data) │                        │ (PII + local data) │
└────────────────────┘                        └────────────────────┘
```

### Key Principle

* **PII never leaves its region**
* **Non-sensitive metadata is globally replicated**
* **The application routes writes based on data classification and region**

---

## 3. Data Model Strategy (Critical Interview Signal)

### Explicit Data Segmentation

You **must physically separate**:

| Data Type                  | Examples                         | Storage Strategy                 |
| -------------------------- | -------------------------------- | -------------------------------- |
| **Sensitive (PII)**        | Name, email, address, IPs        | Region-local databases only      |
| **Non-Sensitive Metadata** | User ID, tenant ID, plan, status | Globally replicated              |
| **Derived / Aggregates**   | Usage counts, billing states     | Region-derived, globally visible |

This separation is enforced at:

* Schema level
* Database connection level
* ORM / repository level

---

## 4. PostgreSQL Design: Concrete Mechanisms

### 4.1 Geo-Partitioning (Primary Strategy)

**Each region has its own primary PostgreSQL cluster**.

* US cluster: `users_us`, `customers_us`
* EU cluster: `users_eu`, `customers_eu`

No cross-region writes for PII.

This satisfies:

* GDPR Article 44 (data transfer restrictions)
* Right-to-erasure enforcement
* Regulatory audits

---

### 4.2 Sharding Strategy (Regional Shards)

This is **geo-sharding**, not hash-sharding.

| Shard Key   | Value                  |
| ----------- | ---------------------- |
| `region`    | `US`, `EU`             |
| `tenant_id` | Secondary partitioning |

Example:

```sql
users_eu PARTITION BY HASH (tenant_id)
```

Benefits:

* Horizontal scalability inside region
* Clean regulatory boundary

---

### 4.3 Global Metadata Store (Logical Replication)

For **non-PII metadata only**, use:

#### PostgreSQL Logical Replication

* Publication on regional clusters
* Subscription to a **Global Metadata DB**

Replicated tables:

* `user_id`
* `tenant_id`
* `subscription_status`
* `entitlements`
* `feature_flags`

Example:

```sql
CREATE PUBLICATION global_metadata_pub
FOR TABLE user_metadata, subscriptions;
```

Why logical replication:

* Row-level control
* Schema-filtered
* No PII leakage risk
* Near-real-time (seconds)

---

### 4.4 Read Replicas (Secondary Optimization)

Use **read replicas per region** for:

* Reporting
* Analytics
* Admin dashboards

**Important**:

* Replicas do **not** cross sovereignty boundaries for PII
* Global services query the metadata DB instead

---

## 5. Application Layer Design (Unified Codebase Requirement)

### Single Codebase, Region-Aware Routing

The application uses:

* **Region-aware connection routing**
* **Data-classification-aware repositories**

Example (conceptually):

```plaintext
UserService
 ├── MetadataRepository → Global Metadata DB
 └── PIIRepository
      ├── EU → EU PostgreSQL Cluster
      └── US → US PostgreSQL Cluster
```

Key rules:

* Writes are always region-local
* Reads follow the data owner’s region
* No “join across regions” at runtime

---

## 6. Consistency Model (Explicit Tradeoffs)

| Data Type     | Consistency Model              |
| ------------- | ------------------------------ |
| PII           | Strong consistency (local)     |
| Metadata      | Eventual consistency (global)  |
| Billing state | Causally consistent via events |

This is acceptable because:

* GDPR prioritizes correctness and locality over global freshness
* Subscription state tolerates seconds of lag
* User identity integrity remains local

---

## 7. Synchronization Mechanism Beyond Replication

### Event-Driven Sync (Recommended)

Replication alone is not enough.

Use:

* **Outbox pattern** in regional DB
* Events like `UserCreated`, `SubscriptionUpdated`
* Kafka / SNS / PubSub → Metadata DB

This:

* Avoids tight DB coupling
* Provides auditability
* Enables replay and recovery

---

## 8. Failure & Compliance Scenarios (Interview Gold)

### EU Region Outage

* EU users impacted only
* Global metadata remains readable
* No cross-region fallback for PII (intentional)

### GDPR “Right to be Forgotten”

* Delete PII in EU DB
* Emit event to purge metadata references
* Global metadata contains no PII → compliant

### Audit Readiness

* Clear data lineage
* Region-locked backups
* No cross-border replication of PII WALs

---

## 9. Why This Design Is Correct

### Satisfies All Constraints

✔ GDPR & sovereignty
✔ Unified codebase
✔ Near-real-time global metadata
✔ Operational scalability
✔ Clear failure isolation
✔ Auditable and defensible

---

## 10. Final Summary (What You’d Say to Close)

> “I would design the database tier around explicit geo-partitioning of sensitive data, with independent PostgreSQL clusters per region. PII remains strictly region-local, while non-sensitive metadata is synchronized globally using logical replication and event-driven patterns. This allows a single application codebase to operate globally while enforcing data sovereignty at the storage layer, providing strong local consistency for regulated data and eventual consistency for globally shared metadata.”

