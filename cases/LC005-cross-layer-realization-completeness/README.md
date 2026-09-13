# LC005 — Cross-Layer Realization Completeness

**Status:** Smoke MVP / open to invalidation  
**Case version:** 0.1

## One-sentence question

Can a feature/composition be admitted at one layer while a selected realization target silently lacks a required contribution for the semantics that were admitted?

## Why this is not called `BackendHole`

`BackendHole` presupposes a compiler-style architecture. The research question is broader: a workbench may realize a language through generators, interpreters, runtime services, validators, tooling, code generators, deployment targets, or another system-native mechanism.

## Scenario

A composition admits feature `F`. The user/system selects realization target `T` that is claimed to realize the relevant semantic subset. `T` lacks one contribution required to realize `F`.

## Applicability

Applicable when:

- the architecture separates language/feature admission from at least one realization target or layer; and
- the system makes a meaningful claim that the selected target realizes the admitted semantic subset.

`NOT_APPLICABLE` is valid when there is no comparable split between admission and realization, or when the system never claims the relevant target supports the feature/subset.

## Independence assumption

`F` is valid in the abstract composition and `T` is otherwise a valid realization target. The missing contribution is the only intended difference.

## Stimulus

Select/use `F`, then select/use `T` under a configuration where `T` lacks the required realization contribution.

## Acceptable outcome classes

- `REJECT` — reject the feature/target composition before the unsupported execution path is used;
- `REQUIRE_EXPLICIT_POLICY` — require an explicit restriction, adapter, fallback, or target choice;
- `ACCEPT_WITH_DEFINED_SEMANTICS` — use a documented alternative realization whose selection is explicit and produces appropriate provenance/evidence;
- `NOT_APPLICABLE` — with a principled architecture-specific rationale;
- `OTHER` — with actual system-native behavior described.

## Failure of interest

The system silently omits required semantics or proceeds as if `T` realized the admitted feature when it does not.

## Observe

- what admitted the feature/composition;
- what selected the realization target;
- decision phase and owner;
- explicit support/capability declaration if any;
- whether execution/use began before incompatibility was detected;
- whether fallback changed the selected realization;
- diagnostics;
- replay/provenance evidence.

## Strongest unfairness argument

Some language systems deliberately define partial tooling or partial target support and do not promise parity across layers. LCLS must not convert “feature exists in parser/editor but intentionally not in code generation” into a failure unless the system or experiment actually claims that target should realize the feature.

## Prior-art anchors

- language-workbench comparison/challenge work already studies heterogeneous capabilities; LCLS does not claim that missing feature support is new.  
  https://github.com/judithmichael/lwb25
- modular language composition work already studies composition across language assets; this case tests a narrow **admission-versus-realization claim boundary**, not generic composition novelty.  
  https://doi.org/10.1145/2427048.2427055

## Falsifier / decision-changing evidence

Revise or delete this case if target-system experts show that the admission/realization distinction cannot be defined without importing UT's compiler pipeline model, or if it collapses into an existing LWC-style feature-presence check without producing a distinct composition observation.

## Reviewer question

**What is the closest native analogue of “admitted here, unrealizable there” in your architecture — or is the premise itself wrong?**
