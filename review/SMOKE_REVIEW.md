# LCLS Smoke Review — three cases, one question

**Review time target:** 5 minutes  
**Status:** pre-benchmark validation

LCLS is testing whether a few language-extension composition cases are fair enough to survive contact with materially different language architectures.

This is **not** a request to review UniversalToolchain, rank frameworks, or implement a benchmark. UT is only an initial reference implementation. A response that says a case is wrong, redundant, or `NOT_APPLICABLE` is useful evidence.

## The one question

> **Which ONE of these three cases is wrong or unfair for your architecture, and what assumption should change?**

Please pick whichever case is closest to your system. One paragraph or even a few sentences is enough.

---

## LC002 — Order Stability

**Scenario:** independently owned contributions participate in one composition. Their registration/discovery order is *not* declared language semantics. We permute only that incidental order.

**Question:** may the resolved composition or behavior silently change?

Reasonable answers can include rejection, requiring explicit order/policy, or a system-native rule that yields defined semantics independent of incidental order. `NOT_APPLICABLE` is valid if order is explicitly semantic in your system.

[Full case](../cases/LC002-order-stability/README.md)

---

## LC004 — Irrelevant Extension Non-Interference

**Scenario:** start from configuration `C`, then add extension `E` while the target system's own declarations/selection rules classify `E` as outside the observed composition surface.

**Question:** what makes `E` genuinely irrelevant in your architecture, and may adding it change the prior result/composition?

The word *irrelevant* is intentionally attackable. ableC/Silver already has strong prior work on reliable independent composition/non-interference; LCLS is testing whether a useful cross-system operational case can be stated, not claiming that property as new.

[Full case](../cases/LC004-irrelevant-extension-noninterference/README.md)

---

## LC005 — Cross-Layer Realization Completeness

**Scenario:** a feature/composition is admitted, then a selected realization target lacks a contribution required to realize the admitted semantics.

**Question:** what is the closest native analogue of “admitted here, unrealizable there” in your architecture — or is that distinction itself a compiler-centric mistake?

A system that explicitly supports partial targets is not failing unless the experiment/system actually claims that the selected target realizes that semantic subset.

[Full case](../cases/LC005-cross-layer-realization-completeness/README.md)

---

## One concrete UT reference point — not the oracle

Current UniversalToolchain/PlanFuzz has an `extension-noninterference` oracle and preserved replay metadata for a **test-owned seeded fault**. The record is useful because it exercises the observation shape, but it does **not** count as a discovered compiler defect and does **not** prove LC004 is portable.

[UT evidence mapping](../results/universaltoolchain/LC004/README.md)

---

## What happens to your answer

If you identify a bad assumption, the project will record one of:

- case narrowed;
- case split/merged;
- acceptable outcome added;
- `NOT_APPLICABLE` boundary corrected;
- prior-art relationship corrected;
- case deleted.

If you are willing, the benchmark author will draft the GitHub issue/result so you only need to confirm or correct the interpretation.

## CTA

**Which ONE case is wrong or unfair for your architecture, and what assumption should change?**
