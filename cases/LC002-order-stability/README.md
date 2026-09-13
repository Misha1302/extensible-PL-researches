# LC002 — Order Stability

**Status:** Smoke MVP / open to invalidation  
**Case version:** 0.1

## One-sentence question

If registration or discovery order is **not** declared to have semantic meaning, may changing only that incidental order silently change the resolved composition or observable behavior?

## Scenario

A host selects two or more independently owned contributions that can participate in the same composition. The language/system does not declare their registration/discovery order to be semantic input. Build two otherwise equivalent configurations that differ only in that incidental order.

## Applicability

Applicable when:

- multiple independently owned contributions can participate in one composition; and
- the system can vary or observe an order that is not itself declared language semantics.

`NOT_APPLICABLE` is valid when, for example:

- the system explicitly defines registration/declaration order as part of the language semantics; or
- there is no analogous multi-contribution composition mechanism.

## Independence assumption

The contributions are individually valid, are authored without coordinating an incidental discovery order, and no declared semantic rule gives that incidental order meaning.

## Stimulus

Evaluate the same selected composition under at least two permutations of incidental registration/discovery order.

## Acceptable outcome classes

- `REJECT` — the composition is underdetermined and is rejected;
- `REQUIRE_EXPLICIT_POLICY` — the user/language must provide an explicit ordering or priority;
- `ACCEPT_WITH_DEFINED_SEMANTICS` — a system-native rule selects a stable result independent of incidental order;
- `NOT_APPLICABLE` — with a principled architecture-specific rationale;
- `OTHER` — with the actual system-native rule described.

## Failure of interest

The observable resolved composition or behavior changes only because incidental registration/discovery order changed, while the system provides no declared semantic rule making that order meaningful.

## Observe

- applicability rationale;
- resolved composition/result for each permutation;
- decision phase;
- decision owner in system-native terms;
- explicit policy source, if any;
- diagnostics/evidence;
- replay/provenance artifact, if the system exposes one.

## Strongest unfairness argument

Some systems intentionally make declaration, import, generator, rule, or registration order semantically meaningful. Treating every order-sensitive result as a defect would encode an architectural preference. This case therefore applies only to **order that the system itself does not declare semantic**.

## Prior-art anchors

- JetBrains MPS explicitly models generator ordering using mapping priorities and generation plans; this is an example of **explicit order semantics/policy**, not automatically a failure.  
  https://www.jetbrains.com/help/mps/generation-plan.html  
  https://www.jetbrains.com/help/mps/mapping-priorities.html
- Language-composition taxonomies and workbench comparisons predate LCLS; this case is an operational probe, not a novelty claim about ordering itself.  
  https://doi.org/10.1145/2427048.2427055

## Falsifier / decision-changing evidence

Revise or delete this case if external reviewers show that the distinction between incidental order and declared semantic order cannot be stated portably enough to compare systems without forcing a common architecture.

## Reviewer question

**For your architecture, which assumption above is wrong or underspecified?**
