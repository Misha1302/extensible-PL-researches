# Research Specification v0.1

## Language Composition Litmus Suite (LCLS)

**Status:** draft for external criticism  
**Repository role:** canonical research specification  
**Primary objective:** obtain high-information feedback on the problem formulation before building expensive infrastructure.

---

## 1. Problem statement

Programming-language extensibility is often demonstrated by showing that a host language or language workbench can add syntax, semantics, transformations, tooling, or domain-specific constructs. That is necessary but insufficient for a stronger claim: that **independently developed extensions can be composed reliably**.

The hard failures frequently appear only after composition:

- individually valid providers become ambiguous;
- passes require an order that no local author owns;
- registration order changes the result;
- a new extension changes unrelated behavior;
- an interpreter supports a feature while another backend silently does not;
- runtime fallback changes a plan after validation;
- version/provenance drift invalidates assumptions;
- syntactic compatibility hides semantic interference;
- a conflict is detected, but too late or with unusable diagnostics.

LCLS will provide minimal cases that make these failures explicit and comparable.

The project deliberately does **not** begin with a new language workbench, a universal architecture DSL, or a claim that all semantic compatibility can be decided automatically. It begins with falsifiable cases.

---

## 2. Research objective

Create a vendor-neutral suite of small composition litmus tests and an evaluation protocol that answer:

1. **Detection:** does a system detect an invalid/ambiguous composition, and at what phase?
2. **Decision ownership:** which component is responsible for resolving the global choice?
3. **Determinism:** is the result invariant under irrelevant registration/configuration permutations?
4. **Non-interference:** can an unrelated extension change an existing composition or result?
5. **Explicitness:** are conflict, precedence, provider choice, fallback, and compatibility policies represented explicitly or hidden in control flow/convention?
6. **Reproducibility:** can the selected composition and relevant provenance be replayed?
7. **Explainability:** can the system produce actionable evidence for why a composition was accepted, rejected, or resolved a certain way?
8. **Cross-layer coverage:** which failures can occur in syntax, binding/types, transformations/IR, optimization, backend selection, runtime materialization, tooling, and versioning?
9. **Limits:** which classes cannot be decided structurally and require semantic tests, proof, domain oracles, or human policy?

The initial study is exploratory and taxonomy-building. It must not pretend to establish a universal ranking from a tiny sample.

---

## 3. Core research questions

### RQ1 — What recurring failure modes arise when independently authored language extensions are composed?

Deliverable: a reviewed taxonomy with minimal counterexamples and explicit boundaries between structural and semantic failures.

### RQ2 — How do different language-engineering approaches expose, reject, resolve, or hide those failures?

Deliverable: per-system behavior records using the same case specification and evidence requirements.

### RQ3 — Which observable properties are useful as architecture-level "composition laws"?

Candidate laws include registration permutation invariance, irrelevant-extension non-interference, explicit ambiguity, no runtime replanning, provenance binding, and backend/route parity where semantics are shared.

Deliverable: laws expressed as predicates over observations, not slogans.

### RQ4 — What is the smallest evaluation artifact that experts consider fair enough to implement on another system?

Deliverable: validated MVP case set and contribution protocol. This RQ is intentionally practical because adoption determines whether later comparative research is possible.

---

## 4. Non-goals

The MVP must **not** attempt to:

- prove semantic equivalence of arbitrary extensions;
- rank all language workbenches with a single score;
- claim that extensibility requires a centralized planner;
- prove UniversalToolchain superior;
- reproduce the full Language Workbench Challenge;
- design a general-purpose architecture specification language;
- build a package manager or universal dependency solver;
- benchmark end-user productivity in the first iteration;
- treat deterministic behavior as synonymous with correct behavior;
- infer industry demand from GitHub stars, Habr comments, or conference interest.

These exclusions are part of the scientific validity of the project.

---

## 5. Unit of evaluation: a litmus case

Every case MUST be small enough that an external implementer can understand the conflict without learning an entire example language.

Each case is assigned a stable ID: `LC###`.

### Required case fields

```yaml
id: LC001
title: Provider ambiguity
status: proposed
layer:
  - composition
  - runtime
intent: reject-or-require-policy
preconditions:
  base_language: minimal expression language
  extensions:
    - extension-a
    - extension-b
independence_assumption: >
  A and B are authored without knowledge of each other and are individually valid.
stimulus: >
  Compose A and B under the same host configuration.
acceptable_outcomes:
  - reject-before-execution
  - require-explicit-policy
unacceptable_outcomes:
  - silently-pick-registration-order
observations:
  - phase-of-decision
  - selected-provider-if-any
  - diagnostic
  - reproducibility-token-if-any
threats_to_validity:
  - system may intentionally define deterministic implicit precedence
notes_for_implementers: >
  If the system's semantics intentionally define a unique result, record that policy
  and demonstrate that it is independent of incidental registration order.
```

### Case design rules

A case MUST:

- isolate one primary phenomenon;
- state assumptions that make the two extensions independently reasonable;
- define acceptable **classes** of outcomes rather than encode one framework's API;
- distinguish `REJECT`, `REQUIRE_EXPLICIT_POLICY`, and `ACCEPT_WITH_DEFINED_SEMANTICS`;
- state what would falsify the intended property;
- record known system-specific escape hatches without automatically treating them as failure;
- be runnable or manually auditable from a clean checkout;
- avoid relying on UniversalToolchain vocabulary unless the term is genuinely domain-general.

A case SHOULD fit on one screen conceptually, even if an implementation requires boilerplate.

---

## 6. Initial taxonomy

The taxonomy is provisional and is expected to change after expert review.

### A. Selection and ambiguity

- multiple providers for one capability;
- multiple valid artifact/transformation routes;
- implicit fallback selection;
- hidden default-provider preference.

### B. Ordering and interaction

- non-commuting transformations;
- contradictory before/after constraints;
- locally reasonable passes whose global order is undefined;
- phase-local ordering that leaks into another phase.

### C. Determinism and reproducibility

- registration-order dependence;
- filesystem/discovery-order dependence;
- unstable equal-cost/equal-priority choice;
- non-canonical serialization or plan identity drift.

### D. Non-interference

- unrelated extension changes selected route/provider;
- extension changes semantics outside its declared surface;
- extension alters diagnostics/tooling of unrelated constructs;
- adding an unused feature changes code generation.

### E. Cross-layer completeness

- syntax exists without semantic support;
- semantic feature exists without one backend;
- optimizer assumes pre-extension semantics;
- tooling accepts syntax the runtime cannot execute;
- interpreter/compiler behavior diverges on a shared subset.

### F. Provenance, lifecycle, and version drift

- planned component differs from executed component;
- package/version changes after planning;
- runtime re-resolves choices;
- stale cache/lock representation is reused across incompatible versions.

### G. Structural vs semantic compatibility

- structurally compatible transformations are semantically non-commutative;
- extension invalidates optimizer assumptions;
- two type-system extensions create a new ambiguity;
- name-binding changes capture behavior.

The suite MUST explicitly label cases that cannot be decided without a semantic oracle.

---

## 7. MVP case set

The MVP target is **8–12 cases**, not 30+.

The first candidate set should cover different layers and failure mechanisms:

1. `LC001 ProviderAmbiguity`
2. `LC002 RegistrationPermutation`
3. `LC003 HiddenTransformationOrdering`
4. `LC004 IrrelevantExtensionNonInterference`
5. `LC005 BackendHole`
6. `LC006 RuntimeReplanningOrFallback`
7. `LC007 FeatureResurrectionAfterExclusion`
8. `LC008 VersionOrProvenanceDrift`
9. `LC009 StructuralRouteSemanticMismatch`
10. `LC010 OptimizerAssumptionInvalidation`
11. `LC011 NameBindingInterference`
12. `LC012 DiagnosticActionability`

Only cases that survive adversarial review enter `mvp-1`. A smaller, sharper suite is preferred over broad but ambiguous coverage.

---

## 8. Observable result model

Do not reduce a system to pass/fail. Each implementation produces a structured observation.

```yaml
case_id: LC001
system:
  name: example
  version: 1.2.3
  revision: abcdef0
environment:
  os: linux
  runtime: jvm-25
outcome:
  classification: require-explicit-policy
  phase: planning
  deterministic: true
  execution_started: false
diagnostic:
  available: true
  machine_readable: false
  summary: "ambiguous provider"
composition_evidence:
  replayable: true
  artifact: path/to/evidence
notes: "..."
```

### Required raw dimensions

- outcome classification;
- phase of detection/resolution;
- execution started or not;
- determinism under defined permutations;
- explicit policy required or not;
- replay/provenance artifact available or not;
- diagnostic presence and actionability evidence;
- manual intervention required;
- source changes required to host/base language;
- evidence revision.

No composite score is permitted in MVP unless a later study justifies weights independently.

---

## 9. Composition laws as testable properties

The project may derive reusable laws from repeated cases. A law is accepted only if it has:

- a precise precondition;
- observable inputs and outputs;
- an explicit oracle;
- at least one positive case;
- at least one negative/control case;
- a documented class of systems for which the law is intentionally inapplicable.

Candidate laws:

### L1 — Registration permutation invariance

If registration order is not declared to have semantic meaning, permutations of independent registrations must not change the observable resolved composition.

### L2 — Irrelevant-extension non-interference

Adding an extension whose declared surface is unreachable from a configuration must not change the prior observable behavior for that configuration.

### L3 — Explicit ambiguity

When more than one semantically eligible implementation exists and no language rule uniquely selects one, the system must reject or require an explicit policy rather than depend on incidental discovery order.

### L4 — No hidden runtime replanning

If a system claims a pre-execution composition artifact, execution must not silently replace its selected components/routes without producing new explicit evidence.

### L5 — Provenance binding

A replay claim must identify enough version/revision/configuration information to reconstruct the evaluated composition.

### L6 — Declared parity

Where a system claims multiple backends/routes implement the same semantic subset, their observations for that subset must satisfy an explicit parity oracle.

These are hypotheses, not axioms of all extensible languages.

---

## 10. Reference implementation policy

UniversalToolchain is useful for bootstrapping because it exposes explicit feature/contribution metadata, conflicts, capabilities, deterministic planning, routes, runtime materialization, and relational testing infrastructure.

However:

- UniversalToolchain MUST NOT define the only acceptable outcome;
- cases MUST be reviewed for vocabulary leakage;
- at least one case SHOULD expose a current UniversalToolchain limitation before public comparative claims;
- UT-specific conveniences are not benchmark requirements;
- a case that cannot be represented fairly in a materially different workbench must be revised, scoped, or removed from the comparative subset.

The benchmark is stronger when the reference implementation fails some cases honestly.

---

## 11. Comparison targets

Targets are selected to maximize architectural diversity, not quantity.

Candidate families include:

- projectional language workbenches (e.g. JetBrains MPS);
- parser/declarative workbenches (e.g. Spoofax, MontiCore, Langium);
- modular/extensible language frameworks (e.g. Neverlang, ableC/Silver);
- language-oriented/macro ecosystems (e.g. Racket) for carefully scoped applicable cases;
- UniversalToolchain as the initial explicit-planning reference implementation.

The Language Workbench Challenge is treated as prior art and a potential community bridge, not as a competitor to duplicate.

---

## 12. Evaluation methodology

### 12.1 Case qualification

Before implementation, each candidate case receives:

1. literature/prior-art check;
2. strongest-counterexample review;
3. system-neutrality review;
4. oracle review;
5. minimality review.

A case is rejected if the expected outcome depends mainly on an unstated architectural preference.

### 12.2 Implementation evidence

For each system/case pair record:

- exact repository/release revision;
- setup instructions;
- source/configuration used;
- commands or manual steps;
- raw output/logs;
- observation record;
- deviations from the canonical case;
- reviewer/author confirmation when available.

### 12.3 Permutation tests

Where applicable, run controlled permutations of:

- extension registration order;
- package/discovery order;
- irrelevant extensions;
- equivalent configuration serialization.

Do not use random fuzzing as the only evidence; every reported failure must reduce to a replayable minimal case.

### 12.4 Semantic cases

For semantic compatibility cases, structural acceptance/rejection is not enough. The case must define a domain-specific oracle such as:

- expected observable value;
- backend parity;
- metamorphic relation;
- preservation property;
- intentionally failing mutant.

### 12.5 Human evaluation

The MVP may ask experts whether cases are fair and representative. It must not call this an industry survey unless sampling and methodology actually support that claim.

---

## 13. Anti-bias and credibility rules

The project SHALL:

- publish failures of the reference implementation;
- preserve negative results;
- distinguish `unsupported`, `not applicable`, `invalid case`, and `system failure`;
- invite maintainers to correct interpretations before comparative publication;
- avoid a leaderboard during the exploratory phase;
- avoid weights invented after seeing results;
- maintain a decision log for case inclusion/removal;
- pin public claims to exact suite and implementation revisions;
- disclose when an implementation was authored by the benchmark team rather than the target system's maintainers.

The goal is a useful test suite, not a marketing instrument.

---

## 14. Repository structure target

The MVP should converge toward:

```text
/cases/
  LC001-provider-ambiguity/
    README.md
    case.yaml
    controls/
  ...
/schema/
  case.schema.json
  observation.schema.json
/implementations/
  universaltoolchain/
  <external-system>/
/results/
  observations/
/docs/
  RESEARCH_SPEC.md
  TAXONOMY.md
  METHODOLOGY.md
  DECISIONS.md
/tools/
  validate.py
ROADMAP.md
README.md
LICENSE
```

Do not create empty architecture for its own sake. Directories should appear when the first real artifact needs them.

---

## 15. MVP definition of done

`mvp-1` is done only when all conditions hold:

- 8–12 cases have stable IDs and complete specifications;
- every case has an explicit oracle and threat-to-validity section;
- at least 2 negative/control variants exist across the suite;
- all cases are implemented against UniversalToolchain or marked with a specific blocker;
- at least 2 cases reveal a limitation, unsupported class, or non-trivial trade-off in the reference implementation;
- at least 3 external experts/maintainers have reviewed the suite or a targeted subset;
- at least 1 materially different external language-engineering system has one case implemented or reviewed in enough detail to validate portability of the case format;
- feedback causes at least one documented revision, merge, split, or rejection of a case;
- all published observations are revision-pinned and replayable;
- no claim of general superiority is made.

This definition intentionally makes **external correction** part of MVP completion.

---

## 16. Success criteria for the research program

### Near-term success

- experts engage with concrete cases rather than only debating the high-level thesis;
- at least one external maintainer says a case is useful, unfair, missing a category, or worth implementing;
- the suite changes because of feedback;
- the project identifies a failure taxonomy that is not merely a rephrasing of one framework's architecture.

### Medium-term success

- multiple architectural families have comparable observation records;
- the suite is small enough to be adopted, forked, or used in a workshop/challenge setting;
- results support a defensible paper, experience report, or artifact submission;
- cases/laws are cited or reused independently of UniversalToolchain.

### Failure signals

The project should reconsider direction if:

- experts consistently see the cases as trivial or already fully subsumed by a better-established benchmark;
- cases require extensive framework-specific reinterpretation;
- no external implementer can understand the task without learning UniversalToolchain concepts;
- the taxonomy keeps growing but does not change decisions or reveal failures;
- automation effort materially exceeds insight gained from new cases.

---

## 17. Prior-art anchors

These are starting points, not an exhaustive literature review:

- Language Workbench Challenge 2025: common-task comparison across diverse language workbenches.  
  https://github.com/judithmichael/lwb25
- ableC / Minnesota Extensible Language Tools: reliable and automatic composition of C language extensions.  
  https://melt.cs.umn.edu/ableC/
- ACM SIGPLAN Software Language Engineering (SLE): research community focused on design, implementation, and evolution of software languages.  
  https://conf.researchr.org/home/sle-2026
- UniversalToolchain: initial explicit-planning reference implementation and source of candidate counterexamples.  
  https://github.com/Misha1302/UniversalToolchain

A publication-grade version requires a systematic related-work pass covering language workbenches, modular language composition, macro/language-oriented systems, feature-oriented language development, non-interference, extensible compilers, and software product-line testing.

---

## 18. Governance of the specification

Material changes to the suite should be reviewable in pull requests.

For every added/changed case, the PR should answer:

1. What failure mode does this isolate?
2. What existing case fails to cover it?
3. What is the oracle?
4. What is the strongest argument that the case is unfair or architecture-specific?
5. Which systems are expected to treat it as not applicable?
6. What observation would change our interpretation?

Case deletion is allowed and encouraged when external evidence shows the case is redundant or badly framed.
