# Research Specification v0.2

## Language Composition Litmus Suite (LCLS)

**Status:** draft for external criticism  
**Repository role:** canonical research specification  
**Primary objective:** obtain decision-changing external criticism on a small set of composition cases before building benchmark infrastructure.

---

## 1. Problem statement

Programming-language extensibility is often demonstrated by showing that a host language or language workbench can add syntax, semantics, transformations, tooling, or domain-specific constructs. That is necessary but insufficient for a stronger claim: that **independently developed extensions can be composed reliably**.

The hard failures frequently appear only after composition:

- individually valid providers become ambiguous;
- transformations require an order that no local author owns;
- incidental registration/discovery order changes the result;
- an extension that should be irrelevant changes existing behavior;
- a feature is admitted at one layer while a selected realization path lacks support;
- runtime fallback changes a plan after validation;
- version/provenance drift invalidates assumptions;
- syntactic compatibility hides semantic interference;
- a conflict is detected, but too late or with unusable diagnostics.

LCLS provides small cases that make those interactions explicit and comparable **without assuming that all systems should make the same architectural choice**.

The project deliberately does **not** begin with a new language workbench, a universal architecture DSL, a leaderboard, or a claim that all semantic compatibility can be decided automatically. It begins with falsifiable cases and gives external reviewers explicit power to narrow, reject, split, or delete them.

---

## 2. Research objective

Create a vendor-neutral suite of small composition litmus tests and an evaluation protocol that answer:

1. **Applicability:** is the case meaningful for this architecture, and why?
2. **Detection:** does the system detect or resolve the interaction, and at what phase?
3. **Decision ownership:** which system-native component, rule, language author, application author, runtime component, or human policy owns the decision?
4. **Determinism:** is the result invariant under permutations that the system does not define as semantically meaningful?
5. **Non-interference:** can an extension declared irrelevant to the observed configuration change prior behavior?
6. **Explicitness:** are conflict, precedence, provider choice, fallback, and compatibility policies represented explicitly or hidden in control flow/convention?
7. **Reproducibility:** can the selected composition and relevant provenance be replayed?
8. **Explainability:** can the system produce actionable evidence for why a composition was accepted, rejected, or resolved a certain way?
9. **Cross-layer coverage:** can a composition accepted in one layer fail to be realized in another layer or target?
10. **Limits:** which classes cannot be decided structurally and require semantic tests, proof, domain oracles, or human policy?

The initial study is exploratory and taxonomy-building. It must not pretend to establish a universal ranking from a tiny sample.

---

## 3. Core research questions

### RQ1 — Which proposed composition cases survive external criticism as fair, portable research objects?

Deliverable: a versioned set of cases whose applicability boundaries and expected observation classes have been corrected by people familiar with materially different language-engineering systems.

### RQ2 — What recurring failure modes arise when independently authored language extensions are composed?

Deliverable: an evidence-shaped taxonomy with minimal counterexamples and explicit boundaries between structural and semantic failures.

### RQ3 — How do different language-engineering approaches expose, reject, resolve, or declare those failures inapplicable?

Deliverable: per-system behavior records using the same observation vocabulary without translating every architecture into UniversalToolchain terminology.

### RQ4 — Which observable properties are useful as architecture-level composition hypotheses?

Candidate hypotheses include order stability under semantically irrelevant permutations, irrelevant-extension non-interference, explicit ambiguity, no hidden runtime replanning, provenance binding, and declared realization parity.

Deliverable: hypotheses expressed as predicates over observations, with prior-art relationships and explicit classes of systems for which they are inapplicable or differently formulated.

### RQ5 — What is the smallest evaluation artifact that experts consider fair enough to review or instantiate on another system?

Deliverable: validated Smoke/Public/Research MVP gates and a low-friction contribution protocol.

---

## 4. Non-goals

The early project must **not** attempt to:

- prove semantic equivalence of arbitrary extensions;
- rank all language workbenches with a single score;
- claim that extensibility requires a centralized planner;
- prove UniversalToolchain superior;
- reproduce the full Language Workbench Challenge;
- claim novelty for common-task benchmarking, non-interference, configuration exploration, metamorphic/differential testing, or failure reduction without a specific prior-art gap;
- design a general-purpose architecture specification language;
- build a package manager or universal dependency solver;
- benchmark end-user productivity in the first iteration;
- treat deterministic behavior as synonymous with correct behavior;
- infer scientific value from GitHub stars, article views, or conference compliments.

These exclusions are part of the scientific validity of the project.

---

## 5. Unit of evaluation: a litmus case

Every case MUST be small enough that an external implementer can understand the interaction without learning an entire example language.

Each case is assigned a stable ID: `LC###`. Stable IDs identify the research question; material semantic changes require a case-version change and may require a new case ID rather than silently redefining an old one.

### Required case fields

```yaml
id: LC002
case_version: 0.1
status: smoke
layer:
  - composition
  - transformation
intent: observe-order-stability
applicability:
  required_capabilities:
    - multiple independently owned contributions can participate in one composition
  not_applicable_when:
    - the system explicitly defines discovery/registration order as language semantics
independence_assumption: >
  The contributions are authored independently and no semantic rule gives their
  discovery order meaning.
stimulus: >
  Evaluate equivalent compositions while permuting only incidental registration
  or discovery order.
acceptable_outcomes:
  - reject-underdetermined-composition
  - require-explicit-policy
  - accept-with-defined-order-semantics-independent-of-incidental-order
unacceptable_outcomes:
  - silently-change-result-because-incidental-order-changed
decision_observations:
  phase: ""
  owner_kind: ""
  owner: ""
  policy_explicit: ""
  policy_source: ""
prior_art:
  closest_work: []
  expected_overlap: ""
strongest_unfairness_argument: ""
falsifier: ""
threats_to_validity: []
```

### Case design rules

A case MUST:

- isolate one primary phenomenon;
- make applicability and principled `NOT_APPLICABLE` cheap to state;
- state assumptions that make the independently authored parts reasonable;
- define acceptable **classes** of outcomes rather than encode one framework's API;
- distinguish `REJECT`, `REQUIRE_EXPLICIT_POLICY`, `ACCEPT_WITH_DEFINED_SEMANTICS`, `NOT_APPLICABLE`, and `OTHER`;
- state what would falsify or materially change the intended property;
- record the strongest known prior-art/counterexample relationship;
- record the strongest argument that the case is architecture-specific or unfair;
- preserve system-native mechanisms in implementations and observations;
- be runnable or manually auditable from a clean checkout when it reaches comparative status;
- avoid UniversalToolchain vocabulary unless the term is genuinely domain-general.

A case SHOULD fit on one screen conceptually, even if an implementation requires boilerplate.

`NOT_APPLICABLE` is boundary evidence, not a failure.

---

## 6. Provisional taxonomy

The taxonomy is intentionally provisional and MUST change when external evidence justifies it.

### A. Selection and ambiguity

- multiple eligible providers for one role/capability;
- multiple valid artifact/transformation routes;
- implicit fallback selection;
- hidden default-provider preference.

### B. Ordering and interaction

- non-commuting transformations;
- contradictory before/after constraints;
- locally reasonable passes whose global order is undefined;
- incidental registration/discovery order affecting behavior;
- phase-local ordering that leaks into another phase.

### C. Determinism and reproducibility

- registration/discovery permutation instability when order is not semantic;
- filesystem/discovery-order dependence;
- unstable equal-cost/equal-priority choice;
- non-canonical serialization or composition identity drift.

### D. Non-interference

- an extension declared unreachable/irrelevant changes selected behavior;
- an extension changes semantics outside its declared surface;
- an extension alters diagnostics/tooling of unrelated constructs;
- adding an unused feature changes code generation or execution.

### E. Cross-layer realization completeness

- syntax exists without semantic support;
- semantic feature exists without one selected realization target;
- optimizer assumes pre-extension semantics;
- tooling accepts syntax the runtime cannot execute;
- multiple declared realization routes diverge on a shared semantic subset.

### F. Provenance, lifecycle, and version drift

- planned component differs from executed component;
- package/version changes after planning;
- runtime re-resolves choices without new evidence;
- stale cache/lock representation is reused across incompatible versions.

### G. Structural vs semantic compatibility

- structurally compatible transformations are semantically non-commutative;
- extension invalidates optimizer assumptions;
- two type-system extensions create a new ambiguity;
- name-binding changes capture behavior.

The suite MUST explicitly label cases that cannot be decided without a semantic oracle.

---

## 7. Staged MVP case sets

LCLS uses three evidence gates rather than one fixed case-count target.

### 7.1 Smoke MVP — exactly 3 externally attackable cases

The Smoke MVP exists to test whether independent experts consider the research object fair, useful, and portable before the project spends heavily on implementations.

Initial Smoke set:

1. `LC002 OrderStability`
2. `LC004 IrrelevantExtensionNonInterference`
3. `LC005 CrossLayerRealizationCompleteness`

Smoke MVP requires:

- 3 one-screen case specifications;
- an explicit applicability / `NOT_APPLICABLE` boundary for every case;
- a strongest prior-art/counterexample note for every case;
- a strongest-unfairness argument for every case;
- one replayable or revision-pinned UniversalToolchain worked evidence record, which MUST NOT be treated as the oracle;
- targeted review requests suitable for 3–5 external experts/maintainers.

A Smoke case does **not** need to be implemented in every reference system before outreach.

### 7.2 Public MVP — normally about 5 externally corrected cases

Public MVP requires:

- about 5 cases that survived external criticism;
- at least 3 substantive external reviews;
- at least one case materially revised, split, merged, narrowed, or deleted because of external feedback;
- at least two non-UT system mappings reviewed by people familiar with those systems where feasible;
- at least one independently executable non-UT result;
- at least one public UT limitation, unsupported class, or non-trivial trade-off;
- a public decision log and versioned case identities;
- citation metadata.

Case count may increase only when evidence reveals a distinct phenomenon that existing cases cannot represent.

### 7.3 Research MVP — minimum for comparative claims

A comparative Research MVP should normally contain 6–8 externally reviewed cases and evidence from at least two materially different non-UT systems in addition to UT.

Before comparative claims, the study MUST include:

- target-system-native descriptions;
- explicit `NOT_APPLICABLE` reasons;
- controls including an obvious success, obvious rejection, and intentional N/A;
- revision/environment-pinned evidence;
- semantic oracles for semantic cases;
- interpretation review by target-system experts when feasible;
- explicit benchmark-author-bias threats;
- at least one result unfavorable to or limiting UT.

The former 8–12 range is a possible later corpus size, not a prerequisite for first external review.

---

## 8. Observable result model

Do not reduce a system to pass/fail. Each implementation or reviewed mapping produces a structured observation.

```yaml
case_id: LC004
case_version: 0.1
system:
  name: example
  version: 1.2.3
  revision: abcdef0
environment:
  os: linux
  runtime: jvm-25
applicability:
  classification: applicable
  rationale: ""
outcome:
  classification: accept-with-defined-semantics
  execution_started: false
decision:
  phase: planning
  owner_kind: framework
  owner: "system-native rule/component"
  policy_explicit: true
  policy_source: "path/rule/config/API if available"
diagnostic:
  available: true
  machine_readable: false
  summary: ""
composition_evidence:
  replayable: true
  artifact: path/to/evidence
  evidence_revision: abcdef0
interpretation:
  authored_by_benchmark_team: true
  checked_by_target_expert: false
notes: ""
```

### Required raw dimensions

- applicability classification and rationale;
- outcome classification;
- phase of detection/resolution;
- system-native decision owner;
- execution started or not;
- determinism under defined permutations where applicable;
- explicit policy required or not;
- replay/provenance artifact available or not;
- diagnostic presence and actionability evidence;
- manual intervention required;
- source changes required to host/base language;
- evidence revision;
- who authored and who reviewed the interpretation.

No composite score is permitted in MVP unless a later study justifies weights independently.

---

## 9. Composition hypotheses as testable properties

The project may derive reusable hypotheses from repeated cases. A hypothesis is retained only if it has:

- a precise precondition;
- observable inputs and outputs;
- an explicit oracle;
- at least one positive case;
- at least one negative/control case;
- a documented class of systems for which it is intentionally inapplicable or differently formulated;
- a strongest known prior-art formulation and an explanation of whether LCLS is equivalent, weaker, stronger, or merely an operational probe.

### H1 — Order stability under non-semantic permutations

If registration/discovery order is not declared to have semantic meaning, permuting only that incidental order must not silently change the observable composition.

Engineering anchor: systems such as MPS explicitly model transformation ordering through mechanisms such as mapping priorities and generation plans. LCLS must distinguish incidental order from declared language semantics.

### H2 — Irrelevant-extension non-interference

Adding an extension whose declared surface is unreachable from an observed configuration must not change the prior observable behavior for that configuration.

Prior-art anchor: ableC/Silver research on reliable independent composition and non-interference. LCLS does **not** claim the non-interference idea itself as novel; it tests whether a cross-system operational case can be stated fairly.

### H3 — Explicit ambiguity when no language rule selects a unique result

When more than one semantically eligible implementation exists and no language rule uniquely selects one, the system should reject, require explicit policy, or expose another system-native resolution rule rather than depend on incidental discovery order.

### H4 — No hidden runtime replanning after a claimed frozen composition artifact

If a system claims a pre-execution composition artifact, execution should not silently replace selected components/routes without producing new explicit evidence.

### H5 — Provenance binding

A replay claim identifies enough version/revision/configuration information to reconstruct the evaluated composition.

### H6 — Declared realization parity

Where a system claims multiple realization routes implement the same semantic subset, their observations for that subset satisfy an explicit parity oracle.

These are hypotheses, not axioms of all extensible languages.

---

## 10. Reference implementation policy

UniversalToolchain is useful for bootstrapping because it exposes explicit feature/contribution metadata, conflicts, capabilities, deterministic planning, typed routes, runtime materialization, lock/provenance data, and PlanFuzz relational-testing infrastructure.

However:

- UniversalToolchain MUST NOT define the only acceptable outcome;
- UT terminology MUST NOT leak into system-neutral case statements without justification;
- Smoke outreach begins after one UT evidence record, not after complete UT coverage;
- before comparative publication, at least one case SHOULD expose a current UT limitation, unsupported class, or explicit trade-off;
- UT-specific conveniences are not benchmark requirements;
- a case that cannot be represented fairly in a materially different workbench must be revised, scoped, or removed from the comparative subset;
- seeded-fault/harness evidence is labelled as such and is not reported as a discovered compiler defect.

The benchmark is stronger when the reference implementation fails or marks cases unsupported honestly.

---

## 11. Comparison targets

Targets are selected to maximize architectural diversity and information value, not quantity.

Candidate families include:

- projectional language workbenches (e.g. JetBrains MPS);
- parser/declarative workbenches (e.g. Spoofax, MontiCore, Langium);
- modular/extensible language frameworks (e.g. Neverlang, ableC/Silver);
- language-oriented/macro ecosystems (e.g. Racket) for carefully scoped applicable cases;
- UniversalToolchain as an initial explicit-planning reference implementation.

The Language Workbench Challenge is prior art and a potential future community bridge, not a competitor to duplicate. A challenge/subchallenge proposal is deferred until Public MVP evidence exists.

---

## 12. Evaluation methodology

### 12.1 Case qualification

Before a case enters Public MVP, it receives:

1. literature/prior-art check;
2. strongest-counterexample review;
3. system-neutrality review;
4. applicability/N/A review;
5. oracle review;
6. minimality review;
7. strongest-unfairness argument.

A case is rejected if the expected outcome depends mainly on an unstated architectural preference.

### 12.2 Implementation evidence

For each system/case pair record:

- exact repository/release revision;
- exact case version;
- setup instructions;
- source/configuration used;
- commands or manual steps;
- raw output/logs;
- observation record;
- deviations from the canonical case;
- implementation author;
- target-system reviewer/maintainer confirmation when available.

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
- realization-route parity;
- metamorphic relation;
- preservation property;
- intentionally failing mutant.

### 12.5 Human evaluation

The early project asks experts whether cases are fair, useful, redundant, missing assumptions, or inapplicable. It must not call this an industry survey unless sampling and methodology actually support that claim.

### 12.6 Review-response quality

For outreach and conference feedback, use a simple quality scale:

- `0` — social/compliment only;
- `1` — applicability or outcome label without mechanism;
- `2` — concrete system mechanism, phase, owner, or prior-art correction;
- `3` — decision-changing case/taxonomy/claim correction;
- `4` — durable issue/PR/implementation/reuse.

Score `>=2` counts as substantive review; score `>=3` counts as decision-changing evidence.

---

## 13. Anti-bias and credibility rules

The project SHALL:

- publish failures and limitations of the reference implementation;
- preserve negative results;
- distinguish `UNSUPPORTED`, `NOT_APPLICABLE`, `INVALID_CASE`, and `SYSTEM_FAILURE`;
- treat reviewer-supported `INVALID_CASE` as a successful research outcome when a case encoded an architectural preference or incorrect assumption;
- preserve principled `NOT_APPLICABLE` rationales as first-class boundary evidence;
- invite maintainers to correct interpretations before comparative publication;
- avoid a leaderboard during the exploratory phase;
- avoid weights invented after seeing results;
- maintain a public decision log for case inclusion/removal/split/merge/narrowing;
- pin public claims to exact suite and implementation revisions;
- disclose when an implementation was authored by the benchmark team rather than the target system's maintainers;
- publish case deletions/splits and the evidence that caused them.

The goal is a useful research instrument, not a marketing instrument.

---

## 14. Repository structure target

The project should grow only when real artifacts need a place:

```text
/cases/
  LC002-order-stability/
    README.md
  LC004-irrelevant-extension-noninterference/
    README.md
  LC005-cross-layer-realization-completeness/
    README.md
/review/
  SMOKE_REVIEW.md
  RESPONSE_TEMPLATE.md
/results/
  universaltoolchain/
/docs/
  RESEARCH_SPEC.md
  DECISIONS.md
ROADMAP.md
README.md
LICENSE
```

Schemas, runners, dashboards and generalized adapters appear only after repeated evidence demonstrates a stable abstraction.

---

## 15. MVP definitions of done

### Smoke MVP DoD

- exactly 3 cases have stable IDs and complete one-screen specifications;
- every case has an applicability/N/A boundary, prior-art anchors, falsifier, and strongest-unfairness argument;
- at least one real UT evidence record is revision-pinned and clearly labelled by evidence type;
- the reviewer packet exposes one CTA and does not require reading UT;
- targeted review can begin immediately.

### Public MVP DoD

- about 5 externally corrected cases;
- every case has an explicit oracle and threat-to-validity section;
- at least 3 external experts/maintainers have provided substantive review;
- at least one case materially changed because of external feedback;
- at least 2 non-UT system mappings have been reviewed where feasible;
- at least 1 materially different external system has one executable result;
- at least one UT limitation/unsupported class/trade-off is public;
- published observations are revision-pinned and replayable;
- decision log and citation metadata exist;
- no claim of general superiority is made.

### Research MVP DoD

- 6–8 reviewed cases unless evidence justifies another size;
- UT plus at least two materially different non-UT architecture families;
- controls and semantic oracles appropriate to claims;
- target-system-native descriptions and interpretation review where feasible;
- explicit threat-to-validity analysis;
- evidence is sufficient for the specific comparative claims being made.

---

## 16. Success criteria for the research program

### Near-term success

- experts engage with concrete cases rather than only debating the high-level thesis;
- at least one external expert changes an assumption, applicability boundary, outcome class, or prior-art interpretation;
- that change is preserved in the decision log;
- at least one non-UT system can represent one case without being translated into UT concepts.

### Medium-term success

- multiple architectural families have comparable observation records for a meaningful subset;
- maintainers validate some interpretations;
- the suite is small enough to be adopted, forked, or reused in a workshop/challenge setting;
- results support a defensible paper, experience report, or artifact submission;
- cases/hypotheses are cited or reused independently of UniversalToolchain.

### Failure / pivot signals

The project MUST run a pivot review if:

- two or more independent experts identify the same stronger prior work that substantially subsumes the central claim;
- three or more independent experts classify at least two of the three Smoke cases as `NOT_APPLICABLE` for principled architectural reasons;
- a case still requires UT-specific vocabulary after two external revisions;
- external implementers cannot understand a case without learning UT concepts;
- implementation effort consistently exceeds the information gained;
- the taxonomy grows but does not change decisions or reveal meaningful boundaries.

Candidate pivot: narrow the comparative domain or make PlanFuzz/configuration-aware composition testing the primary artifact and LCLS the minimized failure corpus.

---

## 17. Prior-art anchors

These are mandatory starting points for case qualification, not an exhaustive literature review:

### Workbench comparison / challenge design

- Language Workbench Challenge 2025.  
  https://github.com/judithmichael/lwb25
- *A Language Workbench in Action: Comparative Study / Language Workbench Challenge benchmark lineage* (Computer Languages, Systems & Structures, 2015 benchmark work).  
  https://doi.org/10.1016/j.cl.2015.08.007

### Language composition / reuse

- Erdweg, Giarrusso, Rendel — *Language Composition Untangled* (2012).  
  https://doi.org/10.1145/2427048.2427055
- Degueule et al. — *Melange: a meta-language for modular and reusable development of DSLs* (SLE 2015).  
  https://doi.org/10.1145/2814251.2814252
- Leduc, Degueule, Combemale — *Modular language composition for the masses* / ALEX (SLE 2018).  
  https://doi.org/10.1145/3276604.3276622
- Bertolotti, Cazzola, Favalli — *On the granularity of linguistic reuse* (JSS 2023).  
  https://doi.org/10.1016/j.jss.2023.111704
- de Lara, Guerra, Bottoni — *Modular language product lines: concept, tool and analysis*.  
  https://doi.org/10.1007/s10270-024-01179-9
- Jansen, Lüpges, Rumpe — *Lessons Learned from Developing the MontiCore Language Workbench: Challenges of Modular Language Design* (SLE 2025).  
  https://doi.org/10.1145/3732771.3742717

### Reliable independent composition / non-interference

- ableC / Minnesota Extensible Language Tools.  
  https://melt.cs.umn.edu/ableC/
- *Reliable and Automatic Composition of Language Extensions to C: The ableC Extensible Language Framework* (OOPSLA 2017).  
  https://doi.org/10.1145/3138224
- *Detecting and eliminating non-interference violations in extensible compilers* (SLE 2017).  
  https://doi.org/10.1145/3136014.3136023

### Testing-method baselines

Where a contribution depends on configuration exploration, metamorphic/differential testing, or reduction, compare against relevant testing baselines rather than treating those ideas as new:

- Csmith (PLDI 2011).  
  https://doi.org/10.1145/1993498.1993532
- Equivalence Modulo Inputs (PLDI 2014).  
  https://doi.org/10.1145/2594291.2594334
- SPLat (ESEC/FSE 2013).  
  https://doi.org/10.1145/2491411.2491459

### Reference implementation

- UniversalToolchain.  
  https://github.com/Misha1302/UniversalToolchain

A publication-grade version still requires a systematic related-work pass covering language workbenches, modular language composition, macro/language-oriented systems, feature-oriented language development, non-interference, extensible compilers, software product-line testing, benchmark/challenge methodology, and the exact claims ultimately made.

---

## 18. Governance of the specification

Material changes to the suite are reviewable in pull requests and recorded in `docs/DECISIONS.md`.

For every added/changed case, the PR should answer:

1. What phenomenon does this isolate?
2. What existing case fails to cover it?
3. What is the applicability boundary?
4. What is the oracle?
5. What is the strongest argument that the case is unfair or architecture-specific?
6. What prior art is closest?
7. Which systems are expected to treat it as `NOT_APPLICABLE`?
8. What observation would change our interpretation?

Case deletion is allowed and encouraged when external evidence shows the case is redundant or badly framed.

### Attribution / provenance

Before Public MVP amplification, add:

- `CITATION.cff`;
- a canonical case registry with case versions and supersession history;
- contribution/credit policy distinguishing case author, implementation contributor, interpretation reviewer, and paper authorship;
- versioned releases;
- DOI-backed archive when a release is stable enough to cite.

Do not add artificial licensing restrictions to preserve credit; preserve credit through canonical identity, versioning, provenance, citation metadata, and public decision history.
