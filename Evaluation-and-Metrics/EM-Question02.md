# EM - Question 02 - Define software architecture evaluation and explain its role in the software development lifecycle.

### What is Software Architecture Evaluation

Software architecture evaluation is a systematic process of assessing a software system’s architecture to determine how well it meets required quality attributes (non-functional requirements), to identify risks, trade-offs, and potential design issues, and to validate that architectural decisions will support current and future business or mission goals.

Key points in the definition:

* It looks beyond just whether the system “works” (i.e. functional requirements) to **qualities** such as performance, security, modifiability, maintainability, scalability, reliability, etc.
* It is concerned with **trade-offs**: improving one quality (e.g. performance) often negatively impacts others (e.g. modifiability or cost).
* It seeks **risks**: design choices that may turn out poorly under future change, load, security threats, or other stressors.
* It often involves scenarios or use cases to test how the architecture behaves under different conditions.

The Software Engineering Institute (SEI) describes architecture evaluation as a way to uncover whether an architecture supports the quality attributes needed, and to manage risk by catching problems early. ([sei.cmu.edu][1])

---

#### Common Methods & Tools

Some of the well-known architecture evaluation methods include:

* **ATAM (Architecture Tradeoff Analysis Method)** — widely used; helps stakeholders understand trade-offs between quality attributes, clarifies risks and sensitivity points. ([sei.cmu.edu][2])
* **SAAM (Software Architecture Analysis Method)** — earlier scenario-based method, particularly focusing on modifiability. ([Wikipedia][3])
* **ARID (Active Reviews for Intermediate Designs)** — evaluates not fully fixed architectures, combining design review and scenario-based methods. ([Wikipedia][4])
* Others exist: various scenario-based evaluation methods; models, metrics, simulations; sometimes quantitative as well as qualitative. ([scholarscentral.com][5])

---

#### Role of Architecture Evaluation within the SDLC

Evaluating software architecture plays several roles at different stages of the software development lifecycle. Here’s how it typically fits and why it matters.

| SDLC Phase                                | What architecture evaluation contributes / Why it is done here                                                                                                                                                                                                              |
| ----------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Requirements and early design**         | Helps clarify quality attribute requirements; surfaces trade-offs early when changes are less expensive; ensures that the chosen architecture is capable of meeting non-functional constraints. For instance, ATAM is intended for early in the lifecycle. ([Wikipedia][6]) |
| **Architectural design**                  | Guides design decisions, helps select among alternatives, ensures that structural decisions reflect the needs for performance, modifiability, security, etc. One can test candidate architectures via scenarios, simulation, or risk analysis.                              |
| **Implementation planning / prototyping** | Use evaluation feedback to adjust design before too much coding; identify risky parts to prototype; adjust the modularization, dependencies, interfaces as needed.                                                                                                          |
| **During development / iterative cycles** | Re-evaluate as the system evolves; ensure that changes have not introduced architectural drift or degraded critical qualities; maintain alignment with the original goals.                                                                                                  |
| **Before major deployment / integration** | Validate that the architecture will scale, behave reliably in production, meet performance/security requirements; reduce risk of cost overruns or failures under real usage.                                                                                                |
| **Maintenance / evolution**               | As business needs, load, requirements change, re-evaluation ensures that the architecture can evolve without excessive cost; identifies parts that are becoming bottlenecks or accumulating technical debt.                                                                 |

---

#### Benefits of Doing Architecture Evaluation

* **Risk reduction**: Catch problems early (in design or early in implementation) before they become costly to fix. ([sei.cmu.edu][1])
* **Better decision making**: Helps stakeholders understand trade-offs; helps prioritize which quality attributes matter most; supports selecting among architectural alternatives. ([sei.cmu.edu][7])
* **Improved architecture documentation and communication**: The evaluation process often forces architects and stakeholders to make explicit what might otherwise remain implicit assumptions. ([sei.cmu.edu][7])
* **Cost and time savings downstream**: Fixing architectural problems later (after implementation/integration) tends to be far more expensive than detecting them early. ([sei.cmu.edu][1])
* **Improved software quality / maintainability / adaptability** over the lifespan of the system. ([ijccn.com][8])

---

#### Challenges / Caveats

While architecture evaluation has many benefits, it also has costs and limitations:

* It requires time, effort, and expertise (architects or evaluators need to have experience in formal evaluation).
* There may be resistance, especially when teams feel pressure to move quickly rather than spend time on architectural review.
* It may be difficult to quantify some quality attributes or trade-offs (e.g., security, usability) ahead of implementation or under the uncertainty of future requirements.
* Some evaluation methods are heavyweight; using them improperly may slow development without delivering proportional benefit.

---

### Summary

Software architecture evaluation is an essential discipline in the SDLC focused on assessing whether architecture decisions will enable a software system to satisfy both its functional and non-functional requirements, particularly the quality attributes, and manage associated risks and trade-offs, as early and as continuously as feasible.

Its role spans from earliest phases (requirements, conceptual design) through design, implementation, deployment, maintenance and evolution. Well-done architecture evaluation improves confidence, reduces risk and rework, and supports maintainability, evolution, and alignment with business or mission goals.

[1]: https://www.sei.cmu.edu/library/reduce-risk-with-architecture-evaluation/ "Reduce Risk with Architecture Evaluation"
[2]: https://www.sei.cmu.edu/library/architecture-tradeoff-analysis-method-collection/ "Architecture Tradeoff Analysis Method Collection"
[3]: https://en.wikipedia.org/wiki/Software_architecture_analysis_method "Software architecture analysis method"
[4]: https://en.wikipedia.org/wiki/Active_reviews_for_intermediate_designs "Active reviews for intermediate designs"
[5]: https://www.scholarscentral.com/abstract/comparison-of-software-architecture-evaluation-methodsrnfor-software-quality-attributes-88005.html "COMPARISON OF SOFTWARE ARCHITECTURE EVALUATION METHODS FOR SOFTWARE QUALITY ATTRIBUTES"
[6]: https://en.wikipedia.org/wiki/Architecture_tradeoff_analysis_method "Architecture tradeoff analysis method"
[7]: https://www.sei.cmu.edu/library/evaluating-software-architectures-methods-and-case-studies/ "Evaluating Software Architectures: Methods and Case Studies"
[8]: https://www.ijccn.com/index.php/IJCCN/article/view/20 "Software Architecture Evaluation Methods: A Comparative Study | International Journal of Computing and Communication Networks"
