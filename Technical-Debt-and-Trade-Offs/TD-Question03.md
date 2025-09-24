# TD - Question 03- Describe a scenario in which taking on technical debt was a valid trade-off. What was the decision, and how was the debt subsequently managed or paid off?

### **Answer 1 – Time-to-Market Pressure**

> In a previous role, our team was building a customer-facing portal with a fixed deadline tied to a product launch. To meet the date, we deliberately chose not to fully modularize certain backend services and reused existing APIs with additional transformation layers. We documented the architectural shortcuts in our design repository and added clear backlog items for remediation. After launch, once customer adoption was validated, we allocated a dedicated sprint to refactor those services into independent modules. This approach allowed us to meet the business deadline without losing sight of long-term maintainability.

---

### **Answer 2 – Proof of Concept to Production**

> During a proof of concept for a recommendation engine, we implemented a monolithic design rather than a fully decoupled microservices approach. The goal was to validate the business value quickly, not to perfect the architecture. Once we proved the ROI, we planned a phased migration to microservices—starting with the highest-load components such as the ranking service. By treating the monolith as a temporary step, we consciously took on technical debt but managed it by creating a refactoring roadmap tied to scaling milestones.

---

### **Answer 3 – Legacy System Integration**

> When integrating with a legacy ERP system, we opted to build a translation layer that duplicated some business logic rather than re-engineer the ERP integration fully. This decision introduced technical debt because it created redundant logic, but it allowed the business to roll out the new functionality within the fiscal year. To manage the debt, we tracked the duplication in our architecture decision records (ADRs), and once the ERP vendor exposed a modern API, we retired the translation layer and consolidated the logic. This staged approach balanced immediate needs with long-term maintainability.

---

### **Answer 4 – Resource Constraints**

> On one project, we had a small engineering team and limited infrastructure budget. We initially built on a single-region cloud deployment with less automation than ideal. This introduced operational risk and technical debt around scalability and failover. However, it enabled us to deliver core business features faster. We explicitly logged these decisions in our technical debt register and made "multi-region deployment" a key objective in the following quarter’s OKRs. When headcount grew, we prioritized infrastructure as code, automated scaling policies, and regional redundancy, systematically paying down the debt.

---

### **Answer 5 – User Experience vs. Architecture**

> For a mobile app launch, the business prioritized user experience polish over backend optimization. We knowingly left parts of the caching strategy and API pagination incomplete, which increased server load but allowed us to deliver a seamless user experience on day one. We monitored performance closely, and after launch, when real usage data was available, we tuned caching, implemented pagination, and reduced query costs. This trade-off helped us focus on customer satisfaction initially, while technical improvements followed on a defined timeline.

---

**Key traits across all answers**:

* Acknowledge the **business driver** (deadline, ROI, legacy system, constraints, user experience).
* Explicitly note the **conscious decision** (not an accident, but a trade-off).
* Show **structured management of debt** (documentation, backlog, ADRs, technical debt register, OKRs).
* Demonstrate **eventual resolution** (refactor, migrate, retire, optimize).


