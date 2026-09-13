# LC004 — Irrelevant Extension Non-Interference

**Status:** Smoke MVP / open to invalidation  
**Case version:** 0.1

## One-sentence question

If an added extension is declared irrelevant to the observed configuration/program, can adding it change prior observable behavior?

## Scenario

Start from a valid configuration `C` and observation `O(C)`. Add extension `E` while keeping `E` outside the declared/reachable surface of the observed behavior. Compare `O(C)` with `O(C + E)`.

The difficult part is the word **irrelevant**. A system-specific implementation must state exactly why `E` should be unreachable or non-participating.

## Applicability

Applicable when the architecture can identify a meaningful boundary such as:

- selected vs unselected feature/contribution;
- reachable vs unreachable extension surface;
- imported vs non-imported module;
- configuration feature excluded by the selected variant;
- otherwise declared non-participation in the observed execution.

`NOT_APPLICABLE` is valid if the architecture intentionally gives every installed/loaded extension ambient global influence and does not claim a non-interference boundary comparable to the case.

## Independence assumption

The base configuration does not depend on `E`, and the system-native declarations/selection rules classify `E` as outside the observed composition surface.

## Stimulus

Evaluate the same program/configuration before and after adding `E` without selecting/reaching `E` according to the system-native boundary being tested.

## Acceptable outcome classes

- `ACCEPT_WITH_DEFINED_SEMANTICS` — prior behavior and relevant composition evidence are unchanged;
- `REJECT` — if adding an unselected/unreachable extension is itself forbidden by an explicit system rule;
- `NOT_APPLICABLE` — with a principled architecture-specific rationale;
- `OTHER` — with the system-native semantic rule described.

## Failure of interest

`E` activates, changes selected composition, changes the result, or otherwise affects the observed behavior despite the tested system-native boundary classifying it as irrelevant/unreachable.

## Observe

- exact relevance/non-participation criterion;
- base and mutated observations;
- activation/ownership evidence if available;
- decision phase and owner;
- diagnostics;
- replay/provenance artifacts.

## Strongest unfairness argument

“Unused” or “unselected” is not a universal semantic concept. Macro systems, open-world registries, dynamic update systems, global method extension, or intentionally ambient plugin systems may define influence differently. The case is invalid if it labels an extension irrelevant using the benchmark author's model rather than the target system's own boundary.

## Prior-art anchors

This property is **not claimed as novel** by LCLS.

- ableC / Minnesota Extensible Language Tools: reliable independent composition of language extensions.  
  https://melt.cs.umn.edu/ableC/
- SLE 2017 work on detecting/eliminating non-interference violations in extensible compilers.  
  https://doi.org/10.1145/3136014.3136023
- ableC reliable composition paper.  
  https://doi.org/10.1145/3138224

## Existing UniversalToolchain evidence

A current UT/PlanFuzz evidence record is mapped in:

`results/universaltoolchain/LC004/README.md`

It is deliberately labelled **seeded-fault / harness evidence**, not a newly discovered compiler defect and not proof that this case transfers unchanged to other architectures.

## Falsifier / decision-changing evidence

Narrow, split, or delete this case if experts show that no system-neutral notion of a declared non-participation boundary survives across the target architecture families, or that the proposed observation is fully subsumed by a stronger established formalism without adding useful cross-system evidence.

## Reviewer question

**What would make an extension genuinely irrelevant in your architecture, and is this case using the wrong boundary?**
