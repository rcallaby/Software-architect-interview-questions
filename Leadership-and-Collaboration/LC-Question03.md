# LC - Question 03 - Collaborative Decision Making (ADRs)

## Question: How do you balance the need for architectural consistency with the autonomy of individual product teams? Specifically, how do you use Architectural Decision Records (ADRs) or RFCs to foster collaboration?

### Answer: 

## Framing the Tension Clearly

I start by naming the core tension directly, because pretending it doesn’t exist undermines credibility:

> *Architectural consistency and team autonomy are both good—and they naturally pull in opposite directions.*

Too much consistency leads to centralized bottlenecks and local disengagement. Too much autonomy leads to fragmentation, duplicated effort, and operational risk. My goal as an architect is not to “pick a side,” but to **decide where consistency is essential and where diversity is acceptable**—and then make those decisions transparent and evolvable.

ADRs and RFCs are the primary tools I use to manage that balance.

---

## Step 1: Define What Must Be Consistent vs. What Must Be Free

Before introducing process, I establish **clear architectural guardrails**.

I explicitly categorize decisions into three buckets:

1. **Non-negotiable constraints**

   * Security standards
   * Regulatory requirements
   * Core interoperability contracts
     These are documented once and referenced everywhere.

2. **Guided standards**

   * Preferred frameworks
   * Observability patterns
   * Deployment conventions
     Teams can diverge, but they must explain why.

3. **Local decisions**

   * Internal service design
   * Language features
   * Internal abstractions
     Teams decide independently with no approval required.

This classification ensures autonomy is preserved where it matters most: local optimization.

---

## Step 2: Use ADRs to Record *Why*, Not Just *What*

I’m very explicit that ADRs are **not approval gates** or design paperwork.

Each ADR answers four questions:

* What problem are we solving?
* What options were considered?
* What decision did we make?
* What trade-offs did we accept?

The value is not the decision—it’s the **reasoning trail**.

This has several benefits:

* Future teams understand context, not just rules
* Reversing a decision becomes safer
* Consistency emerges organically because teams reuse well-reasoned decisions

I often say:

> “If you can explain why your context is different, you’re free to diverge.”

---

## Step 3: Use RFCs for Decisions That Cross Team Boundaries

When a decision impacts multiple teams—shared APIs, platform changes, data contracts—I shift from ADRs to **RFCs**.

The RFC process is intentionally collaborative:

* Early drafts are lightweight and exploratory
* Feedback is requested, not mandated
* Objections must include alternatives, not just vetoes

Critically, RFCs are not consensus-driven. They are **consultative**.

I make it clear that:

* Feedback shapes the decision
* Not every suggestion will be adopted
* Final responsibility is explicit and documented

This prevents decision paralysis while still honoring collaboration.

---

## Step 4: Optimize for “Disagree and Commit”

One of my explicit goals with ADRs and RFCs is enabling healthy disagreement without stalling progress.

I encourage teams to:

* Surface risks early
* Document unresolved concerns
* Commit once a decision is made

If a team disagrees, that disagreement is captured in the ADR or RFC. This has two effects:

1. People feel heard
2. The organization gains institutional memory

Over time, this builds trust in the process—even among skeptics.

---

## Step 5: Make Reversibility Explicit

Architectural consistency becomes dangerous when decisions feel permanent.

To counter that, I often include:

* Review dates
* Success metrics
* Conditions for reversal

For example:

> “If this choice increases deploy time or incident rate, we revisit it in six months.”

This lowers the psychological cost of alignment and encourages experimentation within guardrails.

---

## Step 6: Lead by Writing, Not Enforcing

One subtle but important practice: I write ADRs *with* teams, not *for* them.

Sometimes I’m the author. Sometimes I’m a reviewer. Often I’m a facilitator helping teams articulate trade-offs.

This reinforces a key cultural signal:

> “Architecture is something we do together, not something handed down.”

When teams see ADRs as a tool for clarity rather than control, adoption takes care of itself.

---

## Results and Signals of Success

I know this balance is working when:

* Teams reference past ADRs voluntarily
* RFCs generate thoughtful trade-off discussions
* Fewer decisions are revisited due to “unknown context”
* Architecture evolves without heavy governance

At that point, consistency is maintained not by enforcement, but by shared understanding.

---

## Key Principles I Rely On

1. **Consistency should protect the system, not constrain creativity**
2. **Autonomy without memory leads to repeated mistakes**
3. **ADRs scale reasoning; RFCs scale collaboration**
4. **Transparency reduces the need for authority**
5. **Reversible decisions are easier to align on**

---

## Closing Summary

I balance architectural consistency and team autonomy by clearly defining guardrails, using ADRs to preserve decision context, and leveraging RFCs for cross-team collaboration. By focusing on *why* decisions are made, encouraging healthy disagreement, and making reversibility explicit, I foster alignment without centralization—and enable teams to move quickly without fragmenting the system.

