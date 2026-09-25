# The order of the waves: what each stage needs to teach

This essay discusses how to order a sequence of risky changes — not by ease of execution, but by what each stage teaches the next.

## Objective

Discuss an ordering criterion for migrations and changes executed in multiple stages ("waves"), covering:

* what each wave needs to teach to reduce the risk of the next one;
* why the "easiest first, riskiest last" order tends to fail;
* where to place the most critical stage within the sequence;
* the real cost of ordering by learning instead of by ease.

## The obvious criterion, and why it fails

The most intuitive order for a sequence of changes is to start with what has the fewest dependencies and end with what has the most. It makes sense on paper: it optimizes for "finish the easy ones quickly." In practice, this order leaves the team least prepared exactly when the risk is highest — at the end of the sequence, with the most critical pieces, after weeks of execution and with the team's attention already worn down.

## The criterion used: each wave teaches something the next one will need

The alternative is to order by learning, not by ease: each stage is positioned to reveal, as early as possible, the kind of failure the following stages could suffer.

* **First wave — low risk, high error visibility.** A component with no real user traffic, but with all the same integrations (networking, authentication, observability) the others also have. If something breaks here, it breaks with no customer in the middle, and the cause tends to be generic enough to apply to the rest of the scope.
* **Second wave — first case with real traffic, of low business criticality.** Tests the rollback criterion with real usage, without it being a usage that hurts to lose. It's the first time the criterion defined before any wave began is exercised under pressure, rather than just rehearsed in a lab.
* **Middle waves — grouped by behavior pattern, not by owner.** Grouping by how a component behaves (read-heavy, write-heavy, queue-dependent), rather than by which team owns it, prevents "the way one specific team operates" from becoming an exception never tested in the following waves.
* **The most critical stage — in the middle of the sequence, not at the end.** The natural temptation is to leave the most complicated thing for last, when the team is already tired. The opposite tends to work better: place the most delicate stage as soon as there's already enough confidence in the rollback criterion, but still with energy to spare for handling the unexpected.
* **Last wave — the set with the highest business criticality.** It only reaches the last position after every possible class of failure has already shown up, once, in something less critical.

## The real cost

Ordering by "what teaches the most" has a price: the learning curve hurts in the middle of the path, not just at the start. A stage deliberately positioned in the middle of the sequence can be delayed more than expected, precisely because an edge case only shows up there. The alternative — leaving everything risky for the end — looks safer on paper, but only pushes the difficulty to when the team is more tired and has less margin for error.

## The question that remains

When planning a sequence of risky changes, the question worth asking is: is the order optimized to finish the easy part quickly, or to quickly learn what the next stages will need to know?

---

*This essay uses a generic situation, without identifying any real system, company or incident.*
