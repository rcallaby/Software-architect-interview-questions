# TD - Question 01 - What is technical debt? Can you enumerate different types (architecture, design, code, test, requirements, etc.) and illustrate with examples?

## **Technical Debt – Types and Examples**

## **1. Definition**

**Technical debt** is a metaphor describing the future cost of suboptimal technical decisions taken to meet immediate needs. Similar to financial debt, it incurs **principal** (the effort to fix it) and **interest** (the ongoing cost of reduced productivity, risk, or complexity).

Equation:

$$
Total\_Cost\_of\_Ownership = Principal + (Interest \times Time)
$$

---

## **2. Categories of Technical Debt**

### **(a) Architectural Debt**

* **Definition:** Long-term structural decisions that hinder scalability or adaptability.
* **Example:** Monolithic system where microservices would better support growth.

### **(b) Design Debt**

* **Definition:** Poorly structured components or modules.
* **Example:** Cyclical dependencies across services, violating separation of concerns.

### **(c) Code Debt**

* **Definition:** Poor implementation practices.
* **Example:** Duplicated code blocks, lack of documentation, excessive complexity.

### **(d) Test Debt**

* **Definition:** Insufficient or fragile testing coverage.
* **Example:** Relying on manual QA instead of automated regression tests.

### **(e) Requirements Debt**

* **Definition:** Incomplete, outdated, or ambiguous requirements.
* **Example:** Features coded against assumptions that later conflict with user needs.

### **(f) Infrastructure / Environment Debt**

* **Definition:** Gaps in deployment, CI/CD, or infrastructure-as-code.
* **Example:** Manual server configuration leading to inconsistent environments.

---

## **3. Visual Illustrations**

1. **Debt Pyramid** – showing debt layered from code → design → architecture.
2. **Technical Debt Growth Curve** – cost increases over time if unmanaged.
3. **Debt Categories Mind Map** – taxonomy of debt types and examples.

---

## **4. Examples in Context**

* **Startup MVP:** Takes on *code and test debt* for speed-to-market.
* **Enterprise Legacy System:** Suffers *architectural and requirements debt*.
* **Agile Project Under Pressure:** May incur *design and infrastructure debt* when delivery timelines override process rigor.

---

## **5. Conclusion**

Technical debt is **not inherently negative**—it can be a strategic decision.
However, unmanaged debt compounds and constrains performance, scalability, and maintainability.
Explicit tracking and prioritization are key.

---

**Sources:**

* Cunningham, Ward. *The WyCash Portfolio Management System*. OOPSLA Report, 1992.
* Kruchten, Philippe, et al. *Managing Technical Debt: A Research Roadmap.* ICSE, 2012.
* Fowler, Martin. *Technical Debt Quadrant.* martinfowler.com.
* Richards, Mark, Neal Ford. *Fundamentals of Software Architecture.* O’Reilly, 2020.


