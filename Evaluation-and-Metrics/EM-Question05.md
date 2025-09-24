# EM - Question 05 - How does the Scenario-Based Architecture Analysis Method (SAAM) differ from other evaluation approaches?

## How the Scenario-Based Architecture Analysis Method (SAAM) Differs from Other Evaluation Approaches

It employs scenarios—concrete descriptions of system interactions or changes—to assess an architecture's suitability for specific quality attributes, such as modifiability, portability, reliability, and performance. The method is particularly useful for comparing alternative architectures early in the development process, helping stakeholders identify potential issues before implementation. By focusing on scenarios derived from real-world usage, SAAM provides a practical, context-specific evaluation that bridges abstract quality requirements and architectural decisions.

SAAM's emphasis on modifiability as a primary quality attribute sets it apart, as it uses scenarios to simulate changes and estimate their impact on the architecture. This approach compensates for the challenges in directly measuring qualities like modifiability by making evaluations more tangible and stakeholder-driven. Applications of SAAM have included domains such as computer-aided software engineering (CASE) tools, combat systems, and telecommunications, where it aids in risk identification and architecture refinement.

#### Main Steps of SAAM
SAAM follows a systematic process, typically involving six core steps, which can be conducted in workshops with stakeholders including architects, developers, maintainers, and end-users. These steps ensure a collaborative and iterative evaluation:

1. **Describe the Candidate Architecture(s)**: Present the architecture using notations like components, connectors, and data flows to establish a common understanding.

2. **Develop and Prioritize Scenarios**: Elicit scenarios representing anticipated uses, modifications, or extensions; classify them (e.g., direct or indirect) and prioritize based on stakeholder input, often through voting.

3. **Classify Scenarios**: Determine if scenarios are direct (supported without changes) or indirect (requiring architectural modifications).

4. **Evaluate Individual Scenarios**: For indirect scenarios, map them to architectural elements, estimate change costs, and identify impacts.

5. **Reveal Scenario Interactions**: Analyze how scenarios interact with shared components, measuring the degree of coupling to assess separation of concerns.

6. **Perform Overall Evaluation**: Weight scenarios and interactions to rank architectures, incorporating stakeholder priorities for a holistic assessment.

#### Differences from Other Evaluation Approaches
SAAM differs from other software architecture evaluation methods in its focus, structure, techniques, and scope. Broadly, evaluation approaches can be categorized into scenario-based (which use hypothetical situations to probe qualities) and non-scenario-based (which rely on metrics, simulations, or expert reviews without scenarios). SAAM pioneered the scenario-based paradigm, influencing later methods, but it has distinct characteristics.

##### Differences from Other Scenario-Based Approaches
Scenario-based methods share a common foundation in using scenarios for evaluation, but SAAM is often simpler and more focused compared to its successors. Key comparisons include:

- **Compared to Architecture Tradeoff Analysis Method (ATAM)**: ATAM, an evolution of SAAM, extends the approach to multiple quality attributes and explicitly analyzes tradeoffs, sensitivities, and risks using a utility tree for scenario prioritization. SAAM primarily targets modifiability and lacks ATAM's phased structure (e.g., investigation and testing phases) or emphasis on business drivers. ATAM is more comprehensive for complex systems, while SAAM is lighter-weight and better for early-stage comparisons.

- **Compared to Architecture-Level Modifiability Analysis (ALMA)**: ALMA specializes in modifiability, using impact analysis and maintenance cost predictions, often with analytical models like UML diagrams. SAAM is broader in quality attributes but does not delve into cost modeling or legacy system reengineering as deeply as ALMA.

- **Compared to Performance Assessment of Software Architectures (PASA)**: PASA integrates quantitative performance modeling (e.g., using UML sequence diagrams) alongside scenarios, making it suitable for real-time systems. SAAM remains qualitative and scenario-centric, without PASA's focus on metrics like response time or antipattern detection.

- **Compared to Cost Benefit Analysis Method (CBAM)**: CBAM links architecture to economics by evaluating costs and benefits, refining scenarios for utility calculations. SAAM does not incorporate economic analysis, focusing instead on technical feasibility.

- **Compared to SAAM for Complex Scenarios (SAAMCS) and Family Architecture Assessment Method (FAAM)**: These are extensions; SAAMCS adds risk assessment for changes using graphs, while FAAM targets interoperability in product families. SAAM is more general and foundational.

Overall, while scenario-based methods overlap in activities like scenario elicitation and evaluation, SAAM's structure is less elaborate, with differences in techniques (e.g., no prescribed models in SAAM) and applications (e.g., SAAM's post-design focus versus ATAM's final-stage emphasis).

##### Differences from Non-Scenario-Based Approaches
Non-scenario-based methods, such as metric-based (e.g., coupling and cohesion metrics), simulation-based (e.g., performance modeling tools), or experience-based (e.g., expert reviews), rely on quantitative data or subjective judgments without scenarios. SAAM differs by using scenarios to contextualize evaluations, making abstract qualities concrete and stakeholder-inclusive. For instance, metric-based approaches provide universal measures but lack prediction of future changes, whereas SAAM's scenario interactions reveal context-specific trouble spots. Simulation-based methods predict behavior through models but may overlook qualitative aspects like maintainability, which SAAM addresses narratively.

To illustrate these distinctions, the following table compares SAAM with selected approaches:

| Approach | Focus | Key Techniques | Stakeholder Involvement | Scope |
|----------|-------|----------------|--------------------------|-------|
| SAAM | Modifiability and quality attributes | Scenario classification, interaction analysis | High (voting for prioritization) | Early comparison of architectures |
| ATAM | Tradeoffs among multiple attributes | Utility tree, sensitivity analysis | High (business drivers included) | Comprehensive risk assessment |
| ALMA | Modifiability and maintenance costs | Impact analysis, UML models | Moderate | Legacy systems and predictions |
| Metric-Based (Non-Scenario) | Structural complexity | Quantitative metrics (e.g., coupling) | Low | Static analysis without context |
| Simulation-Based (Non-Scenario) | Performance prediction | Modeling tools | Low to moderate | Quantitative forecasts |

This table highlights SAAM's balanced, scenario-driven nature.

In summary, SAAM's pioneering use of scenarios for architecture evaluation distinguishes it by providing a lightweight, collaborative framework that emphasizes modifiability and change impact, differing from more elaborate scenario-based methods like ATAM and from non-scenario approaches that lack contextual depth. Its influence persists in modern practices, though it is often augmented by advanced techniques for complex systems.
