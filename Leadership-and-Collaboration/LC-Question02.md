# LC Question 02: Influencing Without Authority

## Question: As an architect, you often propose changes that require other teams to do the work. How do you gain 'buy-in' from a skeptical engineering team that doesn't report to you?

### Answer: 

#### Framing the Reality of the Role

I start by acknowledging an uncomfortable truth, because credibility matters:

> *As an architect, I rarely have direct authority. If my ideas only work when I have control, then they’re not robust.*

Influencing without authority is therefore not a soft skill layered on top of architecture—it **is** the job. Any architectural change that requires other teams to act must compete successfully with their priorities, constraints, and incentives.

So my approach is not “convince people I’m right,” but **align incentives so the change is in their interest**.

---

##### Step 1: Start With Their Pain, Not My Design

When facing a skeptical team, I intentionally **do not begin with the architecture**.

Instead, I invest time understanding:

* What slows them down today
* What incidents or toil they’re blamed for
* What metrics they’re held accountable to
* What trade-offs they’re already making

I’ll often ask questions like:

* “What’s the most frustrating part of owning this service?”
* “What’s the change that makes you nervous to deploy?”
* “What would you fix if you had a quarter with no roadmap pressure?”

This does two things:

1. It surfaces real constraints I may not see from the outside
2. It signals respect for their expertise and ownership

Only after I understand their pain do I frame the architectural change.

---

##### Step 2: Reframe the Proposal as *Their* Problem, Not Mine

Skeptical teams resist changes when they feel like:

* Extra work
* Someone else’s problem
* A theoretical improvement with unclear payoff

So I explicitly reframe the proposal in terms of **what they gain**.

For example:

* Reduced on-call load
* Fewer emergency rollbacks
* Clearer ownership boundaries
* Less cross-team coordination

I avoid abstract goals like “better architecture” and instead say things like:

> “This change reduces the number of teams you depend on from five to one—including yours.”

At that point, the conversation shifts from skepticism to evaluation.

---

##### Step 3: Use Evidence, Not Authority

Architectural authority that relies on seniority fails quickly. Instead, I rely on **shared data**.

Depending on the situation, I bring:

* Deployment frequency and failure rates
* Incident timelines showing coordination delays
* Dependency graphs highlighting change coupling
* Concrete examples of past outages or slow deliveries

The key is that the data is **about the system**, not the team.

This avoids blame and creates a neutral ground:

> “The system is producing these outcomes. Let’s change the system.”

That framing consistently lowers defensiveness.

---

##### Step 4: Reduce the Perceived Risk of Saying “Yes”

Even when teams agree intellectually, they resist because the cost of failure lands on them.

I address this head-on by:

* Proposing **small, reversible steps**
* Offering a pilot instead of a full rollout
* Defining explicit success and rollback criteria
* Volunteering architectural support during implementation

For example:

> “Let’s try this for one API boundary. If it increases your deploy time or incident rate, we stop.”

This changes the decision from:

> “Do we believe in this architecture?”
> to
> “Is this experiment worth running?”

That’s a much easier yes.

---

##### Step 5: Share Ownership, Not Just Work

One common failure mode is asking teams to do the work while architects keep the credit.

I avoid that by:

* Making them co-authors of the design
* Incorporating their constraints into the solution
* Publicly attributing success to the team, not the architecture

In practice, this means:

* Design reviews where they challenge assumptions
* ADRs written collaboratively
* Leadership updates that highlight their impact

People support what they help create.

---

##### Step 6: Align With Leadership Without Weaponizing It

If leadership support is required, I use it **carefully**.

I do *not* say:

> “Leadership wants this.”

Instead, I ensure leadership:

* Understands the trade-offs
* Is aligned on priorities
* Removes organizational blockers

When escalation is needed, it sounds like:

> “We agree on the direction, but the team lacks capacity. Can we adjust priorities?”

This preserves trust and avoids turning leadership into a blunt instrument.

---

##### Results and Indicators of Success

When this approach works, the signals are clear:

* Teams begin bringing architectural problems proactively
* Pushback becomes about trade-offs, not resistance
* Teams adopt the change even when I’m not present
* The architecture continues evolving without enforcement

At that point, influence has scaled beyond the individual.

---

##### Key Principles I Rely On

1. **Influence comes from alignment, not persuasion**
2. **Data beats opinion; empathy beats both**
3. **Risk reduction matters more than elegance**
4. **Trust is built by sharing credit and accountability**
5. **Authority borrowed from leadership must be repaid with restraint**

---

### Closing Summary

To gain buy-in without authority, I focus on understanding team pain, reframing architectural change as a solution to *their* problems, grounding proposals in data, and reducing the risk of adoption. By collaborating rather than prescribing—and by aligning with leadership without undermining trust—I turn architectural direction into a shared goal rather than an imposed mandate.
