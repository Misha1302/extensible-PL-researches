# Roadmap

This roadmap optimizes for **information gained per unit of implementation effort**. The first objective is not feature completeness; it is to discover whether the proposed research object is useful and fair before expensive benchmark infrastructure is built.

## Guiding rule

> Build the smallest artifact that can attract a high-quality correction from somebody who understands a materially different language-engineering system.

---

## Calendar to LangDev 2026

LangDev takes place **8–9 October 2026 in Málaga**. The pre-conference plan is deliberately compressed around feedback rather than infrastructure:

- **8–13 September:** Phase 0 + first 4 litmus cases.
- **14–20 September:** complete the 8–12-case paper MVP and run adversarial/prior-art review.
- **21–27 September:** implement the sharpest cases in UniversalToolchain; preserve failures/limitations.
- **28 September–4 October:** targeted external review with 5–8 researchers/maintainers; revise the suite.
- **5–7 October:** freeze three conference-ready litmus cards and a short reviewer packet.
- **8–9 October:** LangDev field test and structured expert feedback.
- **10–18 October:** incorporate feedback and choose the first non-UT implementation.

Missing a date is less damaging than building infrastructure before the cases survive criticism.

---

## Phase 0 — Research boundary and repository bootstrap

**Target:** now  
**Goal:** make the project legible and prevent premature implementation drift.

Deliverables:

- [x] Apache-2.0 license
- [x] research specification
- [x] staged roadmap
- [ ] short decision log template
- [ ] issue template for proposed litmus cases

Exit criteria:

- scope and non-goals are explicit;
- UniversalToolchain is defined as reference implementation, not oracle;
- MVP requires external correction.

Do **not** build a generic DSL or runner here.

---

## Phase 1 — Paper MVP: 8–12 litmus specifications

**Goal:** validate the taxonomy and case format before writing harness infrastructure.

### 1.1 Draft the first cases

Create 8–12 `LC###` case specifications across at least five taxonomy groups.

Priority order:

1. provider ambiguity;
2. registration permutation;
3. hidden transformation ordering;
4. irrelevant-extension non-interference;
5. backend hole/parity;
6. runtime replanning/fallback;
7. provenance/version drift;
8. structural-vs-semantic mismatch.

Add deeper cases only if the first set does not cover the phenomenon cleanly.

### 1.2 Add controls

At minimum:

- one case where composition should clearly succeed;
- one mutant that should clearly be rejected;
- one intentionally `NOT_APPLICABLE` example demonstrating that the suite is not a universal checklist.

### 1.3 Run adversarial case review

For every case ask:

- is this actually a composition failure or merely a design preference?
- does the expected outcome secretly encode UniversalToolchain?
- can a system legitimately define semantics that make this non-ambiguous?
- is the case minimal?
- does prior art already provide a better formulation?

**Exit criteria:** 8–12 cases survive internal adversarial review with explicit oracles.

---

## Phase 2 — Reference implementation, not reference truth

**Goal:** test whether cases are executable and discover weaknesses in the specification.

Implement the surviving cases in UniversalToolchain using the smallest possible fixtures.

Required outputs per case:

- exact revision;
- setup/command;
- raw evidence;
- structured observation;
- expected-vs-actual note;
- case-specific limitation.

### Critical credibility requirement

Find and preserve at least **two** cases that UT does not cleanly solve, does not support, or solves only with an explicit trade-off.

If every case is a clean UT pass, stop and redesign the suite before comparing with other systems.

### Harness policy

Start with scripts and structured files. Build a generalized runner only after repeated duplication proves the interface.

**Exit criteria:** all MVP cases are runnable or have precise blockers; observation schema has survived real use.

---

## Phase 3 — First external feedback loop

**Goal:** maximize correction quality before breadth.

Do not begin with mass outreach. Select **5–8 people** whose systems/experience represent different approaches.

Suggested target categories:

- language workbench researchers;
- maintainers/authors from Neverlang, MontiCore, Spoofax, MPS, ableC/Silver, Langium, or related systems;
- LangDev speakers/attendees working on DSL composition or compiler architecture.

### Outreach artifact

Send a reviewer a **small packet**, not the whole research agenda:

- one-paragraph motivation;
- 3–5 litmus cases most relevant to their system;
- a one-page explanation of the observation model;
- three concrete questions:
  1. Which case is unfair or underspecified?
  2. Which important failure mode is missing?
  3. How would your architecture represent the owner of this decision?

Do not ask "what do you think about my framework?"

### Feedback accounting

Record each substantive objection as:

- accepted and repaired;
- rejected with evidence;
- unresolved;
- case split/merged/deleted;
- new case candidate.

**Exit criteria:** at least 3 substantive external reviews and at least one material change to the suite caused by them.

---

## Phase 4 — LangDev field test

**Goal:** use the conference as a research instrument, not only a presentation venue.

Prepare 3 highly legible litmus cards:

- ambiguity/ownership;
- non-interference/determinism;
- structural-vs-semantic composition.

For conversations after the talk, ask experts how their architecture would classify the case:

- `ACCEPT_WITH_DEFINED_SEMANTICS`
- `REJECT`
- `REQUIRE_EXPLICIT_POLICY`
- `NOT_APPLICABLE`
- `OTHER`

Capture explanations, not just labels.

### Conference success criterion

At least 5 high-information conversations where the answer changes the taxonomy, oracle, comparison target, or research claim.

A compliment that changes nothing is not counted as research feedback.

---

## Phase 5 — First non-UT implementation

**Goal:** test portability of the suite before building scale.

Choose **one** external system based on:

- maintainer/researcher interest;
- architectural contrast with UT;
- setup cost;
- applicability of at least 3 MVP cases;
- likelihood of getting interpretation reviewed by somebody who knows the system.

Implement only 2–4 cases initially.

Strong candidates will likely come from different families rather than from the closest UT analogue.

### Required comparison discipline

- do not translate every system into UT terminology;
- preserve system-native mechanisms;
- document semantic differences;
- let maintainers mark a case `NOT_APPLICABLE` with rationale;
- get interpretation reviewed before publishing comparative claims.

**Exit criteria:** one materially different system has a reviewed implementation/analysis for at least one case, and the shared schema still makes sense.

---

## Phase 6 — MVP-1 release

Tag `mvp-1` only when the specification's MVP DoD is met.

Release contents:

- 8–12 reviewed cases;
- taxonomy;
- case/observation schema;
- UT reference results;
- at least one external-system implementation or reviewed mapping;
- raw evidence and replay instructions;
- decision log;
- known limitations;
- citation metadata (`CITATION.cff`) if the artifact is stable enough.

### Public narrative

The release message should lead with failures/questions, not with UT.

Preferred framing:

> "Here are small cases that expose where independently authored language extensions need a global decision. We want maintainers to tell us which cases are wrong."

Avoid:

> "Our framework solves extensibility better than existing language workbenches."

---

## Phase 7 — Public feedback amplifier

Only after the MVP cases are concrete, publish a high-signal article/post.

Possible Habr framing:

- "Два расширения работают отдельно. Почему вместе они ломают язык?"
- "10 litmus-тестов для расширяемых языков: где заканчивается модульность?"

The article should contain real cases and invite counterexamples/PRs.

Success metric:

- useful corrections;
- implementations;
- new counterexamples;
- introductions to relevant researchers/maintainers.

Views and likes are secondary metrics.

---

## Phase 8 — Decide whether automation is justified

Only now decide whether repeated implementation work justifies:

- generalized case runner;
- JSON/YAML schema validation tooling;
- environment adapters;
- automated permutation generation;
- property/metamorphic testing;
- reducer/minimizer;
- result site/dashboard;
- composition-law DSL.

### DSL gate

Do not design a DSL until at least 10 cases reveal repeated primitives and at least two different systems demonstrate that those primitives are not UT-specific.

If a simple schema + code remains clearer, do not build a DSL.

---

## Phase 9 — Comparative study expansion

**Goal:** turn the artifact into research evidence rather than a demo.

Expand to 3–5 architecturally diverse systems only if early external work is tractable.

Possible result dimensions:

- detection phase;
- explicitness of policy;
- determinism;
- replayability;
- diagnostic actionability;
- host/base-language modifications required;
- semantic oracle support;
- implementation effort for the case.

Do not collapse dimensions into a leaderboard without an independently justified model.

Add a systematic related-work review and explicit threat-to-validity analysis.

---

## Phase 10 — Publication decision

Choose publication form based on evidence, not prestige first.

### If the strongest result is a new taxonomy + benchmark artifact

Target software-language-engineering venues/workshops/artifact tracks.

### If the strongest result is an empirical cross-system comparison

Target a full empirical/software-language-engineering paper.

### If the strongest result is a conceptual architecture argument with counterexamples

Consider an essay/position/workshop format before claiming a general theorem.

### If external feedback shows the niche is already covered better

Pivot the repository toward reproducing/extending the stronger prior work rather than defending the original framing.

That is a successful research outcome, not a failure.

---

# Immediate backlog

## P0 — next actions

- [ ] Create `LC001 ProviderAmbiguity` specification.
- [ ] Create `LC002 RegistrationPermutation` specification.
- [ ] Create `LC003 HiddenTransformationOrdering` specification.
- [ ] Create `LC004 IrrelevantExtensionNonInterference` specification.
- [ ] Draft `case.schema.json` only after at least 3 case Markdown drafts expose stable fields.
- [ ] Map existing UniversalToolchain/PlanFuzz evidence to those cases without changing the cases to fit UT.
- [ ] Identify 10 candidate external reviewers, then rank by information value and probability of response.

## P1 — after first four cases

- [ ] Prior-art check for each case.
- [ ] Add controls/mutants.
- [ ] Decide which 4–6 cases are best suited for pre-LangDev outreach.
- [ ] Prepare a one-page reviewer packet.

## Explicitly deferred

- generalized benchmark framework;
- web dashboard;
- scoring/leaderboard;
- architecture DSL;
- 30+ case catalog;
- mass outreach;
- claims of market/product fit;
- monetization engineering.

---

# Decision checkpoints

At the end of every phase, answer:

1. What did we learn that changes the next step?
2. Which hypothesis became weaker?
3. What is the cheapest experiment that could falsify the current framing?
4. Are we building infrastructure because evidence requires it, or because it is technically attractive?
5. Has an external expert changed the artifact yet?

If the answer to #5 remains "no" after Phase 3, stop expanding the benchmark and fix the feedback strategy first.
