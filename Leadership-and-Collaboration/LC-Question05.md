# LC - Question 05 - Managing Conflict in Design Reviews

## Question: Describe a situation where two senior leads had a fundamental disagreement over a critical architectural choice. How did you facilitate a resolution without damaging the relationship or stalling the project

### AnswerL 

## Framing the Situation Realistically

I start by normalizing the conflict, because at senior levels it’s not a failure—it’s expected:

> *When experienced leaders disagree, it’s usually because they’re optimizing for different, legitimate concerns.*

In this situation, two senior leads had a fundamental disagreement over a **critical architectural choice** that would shape the system for years.

One lead was optimizing for **long-term scalability and correctness**.
The other was optimizing for **near-term delivery and operational simplicity**.

Both were right—just in different time horizons.

---

## Step 1: Depersonalize the Disagreement

The first thing I did was **remove people from the problem**.

Instead of framing it as:

> “Lead A wants X, Lead B wants Y”

I reframed it as:

> “We have competing architectural forces that need to be reconciled.”

I explicitly named the forces:

* Scalability and future growth
* Delivery timelines and operational cost
* Team skill set and cognitive load
* Failure modes and blast radius

This reframing lowered defensiveness and allowed both leads to engage as peers rather than adversaries.

---

## Step 2: Make Trade-offs Explicit and Shared

I facilitated a session where we jointly documented:

* Assumptions each approach relied on
* Risks we were accepting with each choice
* What failure would look like under each option

We captured this in a lightweight **RFC-style document**, not to decide immediately, but to **externalize the reasoning**.

Once the trade-offs were written down, it became clear that:

* The disagreement was not about facts
* It was about which risks we were willing to carry *now* versus *later*

That insight shifted the conversation from argument to decision-making.

---

## Step 3: Introduce Time as a First-Class Dimension

The breakthrough came when I reframed the question as:

> “What decision do we need to make *now*, and which decisions can be deferred safely?”

Instead of choosing between two mutually exclusive architectures, we explored:

* Incremental paths
* Reversible decisions
* Transitional architectures

For example:

* Start with a simpler model that met current needs
* Design explicit extension points
* Commit to revisiting the decision once usage crossed a defined threshold

This allowed both leads to “win” on the dimensions they cared about.

---

## Step 4: Anchor the Decision to Business Outcomes

To avoid stalemate, I tied the decision to:

* A delivery milestone
* A customer impact metric
* A cost or risk threshold

For example:

> “If usage grows beyond X or MTTR exceeds Y, we trigger the next architectural phase.”

This shifted the debate from preference to **measurable outcomes**.

---

## Step 5: Preserve the Relationship Explicitly

Behind the scenes, I had individual conversations with both leads to:

* Acknowledge their concerns
* Reinforce that their perspective was valid
* Make it clear that the goal was alignment, not compromise for its own sake

Publicly, I was careful to:

* Credit both viewpoints in the final decision record
* Document dissent where it existed
* Emphasize shared ownership of the outcome

This preserved trust and avoided a “winner/loser” dynamic.

---

## Step 6: Close with a Clear Decision and Commitment

Once alignment was reached, I:

* Documented the decision in an ADR
* Explicitly listed the trade-offs we accepted
* Defined review conditions and exit criteria

Then I asked both leads to commit publicly.

That final step was critical:

> Healthy disagreement is fine. Indecision is not.

---

## Outcome and Validation

The project moved forward without delay.
More importantly:

* The architecture evolved as expected
* The revisit conditions were triggered later—and handled smoothly
* Both leads continued to collaborate effectively on future decisions

The relationship was not just preserved—it was strengthened.

---

## Key Principles I Applied

1. **Assume good intent**
2. **Externalize reasoning to reduce emotion**
3. **Make time and reversibility explicit**
4. **Anchor decisions to outcomes, not opinions**
5. **Protect relationships as carefully as the architecture**

---

## Closing Summary

I resolved a fundamental architectural disagreement by depersonalizing the conflict, making trade-offs explicit, introducing time and reversibility as design dimensions, and anchoring the final decision to business outcomes. By facilitating structured disagreement and preserving mutual respect, I enabled progress without sacrificing either the relationship or the quality of the architecture.

