# LCLS smoke review

I'm trying to figure out whether these are actually useful cross-system cases, or whether I've accidentally turned assumptions from my own compiler architecture into “general” problems.

There are only three cases. You don't need to read the rest of the project, understand UniversalToolchain, or implement anything. If one of them resembles something your system already deals with, I'd mostly like to know where the formulation breaks.

> **Which case would you push back on first, and why?**

---

## LC002 — Order Stability

Suppose two independently developed contributions are both used in the same language. Their registration or discovery order is **not** supposed to be part of the language semantics.

Now reverse only that order. Should the resulting composition or program behavior be allowed to change?

A system might reject an ambiguous composition, require an explicit ordering rule, or have its own deterministic rule. All of those seem reasonable to me. And if registration order is deliberately meaningful in your system, this case may simply not apply.

The thing I'm trying to separate is **intentional ordering from accidental ordering**.

[Full case](../cases/LC002-order-stability/README.md)

---

## LC004 — Irrelevant Extension Non-Interference

Start with a working configuration. Then make another extension available, but don't select, import, or otherwise activate it for the configuration being tested.

Can that extension still change the old composition or the behavior of an old program?

The hard part here is deciding what “not participating” means in different systems. If your architecture has no meaningful equivalent of an inactive or irrelevant extension, that may mean the premise of this case is wrong rather than that your system fails it.

This overlaps with existing work on non-interference, especially ableC/Silver; I'm not claiming the property itself is new. What I'm unsure about is whether this can be turned into a small case that is still meaningful across different architectures.

[Full case](../cases/LC004-irrelevant-extension-noninterference/README.md)

---

## LC005 — Cross-Layer Realization Completeness

The concrete compiler version of this one is simple: a construct is accepted by the frontend, but the selected backend cannot actually implement its semantics.

Does your architecture have an analogous situation — something is accepted at one point, but cannot be realized by the target that was selected later? Or does your architecture make that mismatch impossible by construction?

Explicit partial support is fine. If a target openly supports only a subset, that is not a failure. The interesting case is a mismatch between what the chosen composition claims to support and what it can actually realize.

[Full case](../cases/LC005-cross-layer-realization-completeness/README.md)

---

If one of these has a bad premise, I'd rather change or delete the case than force another architecture into the wording. A few sentences are enough.