# extensible-PL-researches

A research workspace for **empirical evaluation of extensible programming-language architectures**, with an initial focus on failures that appear when independently developed language extensions are composed across the compiler/runtime stack.

The first project is the **Language Composition Litmus Suite (LCLS)**: a small, vendor-neutral set of executable or precisely specified litmus tests for composition failures such as provider ambiguity, hidden ordering, registration-order dependence, semantic interference, backend holes, runtime replanning, version drift, and non-interference violations.

The goal is not to prove that one framework is universally better. The goal is to make claims about extensibility **falsifiable, reproducible, and comparable**.

## Start here

- [Research specification](docs/RESEARCH_SPEC.md) — scope, research questions, taxonomy, case format, evaluation model, anti-bias rules, and acceptance criteria.
- [Roadmap](ROADMAP.md) — staged path from a minimal 8–12-case MVP to external validation, multi-system implementations, and publication-grade evidence.

## Initial research thesis

> Extensibility is easy to demonstrate in isolation; reliable composition is harder. A useful evaluation must therefore test what happens when independently authored extensions interact, who owns the resulting global decision, whether failures are detected before execution, and whether the result is reproducible and explainable.

UniversalToolchain may be used as an early reference implementation because it already exposes explicit composition decisions, but it is **not** the benchmark oracle and must not define the suite around what it happens to support.

## Status

Pre-MVP research design. The immediate milestone is not a large framework or DSL. It is a compact litmus suite that can be reviewed quickly by language-engineering researchers and implementers and revised before costly automation is built.

## License

Apache License 2.0. See [LICENSE](LICENSE).
