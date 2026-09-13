# LCLS smoke review

I'm trying to answer a fairly simple question: do these cases describe real problems that show up when language extensions are composed, or have I accidentally baked assumptions from my own architecture into them?

There are only three cases here. You do **not** need to review the whole project, understand UniversalToolchain, or implement anything. If one of the cases is close to something your system already handles, I'd mainly like to know where my formulation is wrong.

A short answer is enough. “This does not apply to us because…” is just as useful as “yes, we have this problem.”

> **Which one of these three cases looks wrong or unfair for your architecture, and what assumption would you change?**

---

## LC002 — Order Stability

Suppose two independently developed contributions are both part of the same composition. Their registration or discovery order is **not** meant to be part of the language semantics.

If I reverse only that incidental order, should the resulting composition or behavior be allowed to change?

I can imagine several perfectly reasonable designs here: reject an ambiguous composition, require an explicit ordering rule, or have a system-level rule that gives the same answer regardless of discovery order. If your system deliberately treats registration order as meaningful, then this case may simply not apply.

What I care about is not whether the system “passes”, but **where that decision is made and whether the order is intentional or accidental**.

[Full case](../cases/LC002-order-stability/README.md)

---

## LC004 — Irrelevant Extension Non-Interference

Start with some working configuration `C`. Then add an extension `E` which, according to the system's own selection/composition rules, should not participate in the part of the language we are observing.

Should adding `E` be able to change the old result anyway?

The difficult part is obviously the word *irrelevant*. Different systems have very different notions of visibility, activation, scope, feature selection, imports, generator participation, and so on. If there is no sensible system-independent way to state that precondition, that is exactly the kind of criticism I'm looking for.

There is already strong prior work here, especially ableC/Silver's work on reliable composition and non-interference. I'm **not** claiming non-interference as a new idea. The question is whether a small cross-system case can still be formulated in a useful and fair way.

[Full case](../cases/LC004-irrelevant-extension-noninterference/README.md)

---

## LC005 — Cross-Layer Realization Completeness

Suppose a feature or composition is accepted, but later the selected target cannot actually realize some part of the semantics that was admitted earlier.

The obvious compiler example is “the frontend accepts it, but this backend cannot implement it”, but I don't want the case to depend on compiler-specific layering.

Does your architecture have a meaningful analogue of **“accepted here, but not realizable there”**? If not, is that because the architecture prevents this situation by construction, or because the distinction itself is the wrong abstraction?

Explicitly partial targets are fine. If a system openly says that a target supports only a subset, that is not a failure by itself. The interesting case is when the composition claims to support something that the selected realization cannot actually provide.

[Full case](../cases/LC005-cross-layer-realization-completeness/README.md)

---

## One UniversalToolchain example

UniversalToolchain is only the first place where I'm exercising these ideas; it is **not** the reference answer for the cases.

For LC004, PlanFuzz already has an `extension-noninterference` oracle with replay metadata for a deliberately seeded fault. That is useful as a concrete example of what evidence for a case might look like, but it is not a discovered compiler bug and it does not show that LC004 transfers cleanly to other systems.

[UT evidence mapping](../results/universaltoolchain/LC004/README.md)

---

If one of these cases has a bad premise, I'd rather change or delete it than force another system into the current wording. The most useful response for me is therefore the uncomfortable one: **which case would you push back on first, and why?**

A few sentences are enough. No formal review is expected.
