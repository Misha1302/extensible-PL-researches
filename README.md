# extensible-PL-researches

A research workspace for **empirical evaluation of extensible programming-language architectures**, with an initial focus on failures that appear when independently developed language extensions are composed across language-engineering layers.

The first project is the **Language Composition Litmus Suite (LCLS)**: a small, vendor-neutral set of executable or precisely specified cases for composition interactions such as order instability, semantic interference, cross-layer realization gaps, ambiguity, provenance drift, and non-interference violations.

The goal is not to prove that one framework is universally better. The goal is to make composition claims **falsifiable, reproducible, comparable, and easy for external experts to correct**.

## Start here

- **[Smoke review: three cases, one question](review/SMOKE_REVIEW.md)** — the fastest way to review the project. Pick one case and tell us which assumption is wrong.
- [Research specification](docs/RESEARCH_SPEC.md) — scope, research questions, staged MVPs, case format, observation model, prior-art boundaries, anti-bias rules, and acceptance criteria.
- [Roadmap](ROADMAP.md) — feedback-first path from three externally attackable cases to the first non-UT result and only then to a broader comparative artifact.
- [Decision log](docs/DECISIONS.md) — material case/claim changes and the evidence that caused them.

## Current Smoke MVP

The project deliberately starts with only three cases:

1. [LC002 — Order Stability](cases/LC002-order-stability/README.md)
2. [LC004 — Irrelevant Extension Non-Interference](cases/LC004-irrelevant-extension-noninterference/README.md)
3. [LC005 — Cross-Layer Realization Completeness](cases/LC005-cross-layer-realization-completeness/README.md)

The first research question is not “which framework passes?”. It is:

> **Which ONE case is wrong or unfair for your architecture, and what assumption should change?**

A principled `NOT_APPLICABLE` or `INVALID_CASE` result is useful evidence. Cases may be narrowed, split, merged, or deleted when reviewers expose a bad abstraction.

## Research thesis

> Extensibility is easy to demonstrate in isolation; reliable composition is harder. A useful evaluation must therefore test what happens when independently authored extensions interact, who owns the resulting global decision, whether the interaction is applicable to the target architecture, and whether the result is reproducible and explainable.

UniversalToolchain is used as an early reference implementation because it exposes explicit composition decisions and PlanFuzz evidence, but it is **not** the benchmark oracle and does not define the only acceptable outcome.

## Status

**Smoke MVP / external-criticism stage.** The immediate milestone is one decision-changing external correction and one non-UT mapping/reproduction — not a large runner, DSL, dashboard, leaderboard, or 8–12-case catalog.

## License

Apache License 2.0. See [LICENSE](LICENSE).
