# Roadmap

This roadmap optimizes for **decision-changing external evidence per unit of implementation effort**. The first objective is not feature completeness; it is to discover whether the proposed research object is useful and fair before expensive benchmark infrastructure is built.

## Guiding rule

> Build the smallest artifact that can attract a high-quality correction from somebody who understands a materially different language-engineering system.

A compliment is not a research result. A corrected assumption, principled `NOT_APPLICABLE`, system-native mechanism, prior-art falsification, validated mapping, or external implementation is.

---

## Calendar to LangDev 2026

LangDev takes place **8–9 October 2026 in Málaga**. The pre-conference plan is deliberately compressed around external validity:

- **8–10 September:** 3-case Smoke MVP + one UT evidence record + first 5 expert sends.
- **11–14 September:** incorporate objections; freeze revised Smoke cases.
- **15–20 September:** obtain the first non-UT mapping/reproduction.
- **21–27 September:** grow to a ~5-case Public MVP only if feedback justifies it; prepare decision log and citation metadata.
- **28 September–2 October:** public/conference artifact freeze; publish broader content only if the external-review gate is met.
- **3–7 October:** conference protocol rehearsal; no new benchmark architecture.
- **8–9 October:** LangDev field experiment.
- **10–18 October:** convert observations to validated issues/results and choose the research-paper/tool direction.

Missing a date is less damaging than building infrastructure before the cases survive criticism.

---

## Phase 0 — Research boundary and repository bootstrap

**Goal:** make the project legible and prevent premature implementation drift.

Deliverables:

- [x] Apache-2.0 license
- [x] research specification
- [x] staged roadmap
- [x] short decision log
- [x] Smoke reviewer packet
- [x] first three Smoke case specifications

Exit criteria:

- scope and non-goals are explicit;
- UniversalToolchain is a reference implementation, not oracle;
- external correction is required for Public MVP;
- `NOT_APPLICABLE` and `INVALID_CASE` are first-class evidence.

Do **not** build a generic DSL or runner here.

---

## Phase 1 — Smoke MVP: 3 externally attackable cases

**Goal:** test the research object before expanding the corpus.

Initial cases:

1. `LC002 OrderStability`
2. `LC004 IrrelevantExtensionNonInterference`
3. `LC005 CrossLayerRealizationCompleteness`

Each case must include:

- applicability / principled N/A boundary;
- independence assumption;
- stimulus;
- acceptable outcome classes;
- unacceptable outcome class;
- decision phase/owner fields;
- prior-art anchors;
- strongest unfairness argument;
- falsifier;
- threats to validity.

### Smoke evidence

One revision-pinned UT worked record is sufficient before outreach. Additional UT implementations are added only after external criticism selects which cases are worth preserving.

The initial UT record may reuse existing replayable PlanFuzz evidence, but its evidence type and claim boundary must be explicit. Seeded-fault evidence is **not** a discovered compiler defect.

### Smoke review CTA

Use one first question only:

> **Which ONE of these three cases is wrong or unfair for your architecture, and what assumption should change?**

Follow-up questions about missing cases, decision owner, implementation, or collaboration come only after engagement.

**Exit criteria:**

- 3 complete Smoke cases;
- one revision-pinned UT evidence record;
- reviewer packet under roughly 900 words;
- 5 personalized review requests prepared/sent by the human operator;
- at least one external response converted into a pending or accepted specification decision before the corpus expands materially.

---

## Phase 2 — External correction before broad implementation

**Goal:** maximize correction quality before code volume.

Select **5 people** for the first wave based on expected information value rather than fame. Strong categories include:

- LWB/benchmark researchers;
- maintainers/authors from MontiCore, Neverlang, ableC/Silver, MPS, Spoofax, Langium or related systems;
- LangDev speakers/organizers with direct composition/testing experience.

### Feedback accounting

Use response quality:

- `0` — social/compliment;
- `1` — applicability/outcome label;
- `2` — mechanism, phase, owner, or concrete prior art;
- `3` — decision-changing case/taxonomy/claim correction;
- `4` — durable issue/PR/implementation/reuse.

A response is substantive at `>=2` and decision-changing at `>=3`.

Record each objection as:

- accepted and repaired;
- rejected with evidence;
- unresolved;
- case split/merged/deleted/narrowed;
- new case candidate.

### Durable-conversion rule

When an expert makes a useful verbal/email observation, the benchmark author drafts the GitHub issue/result and asks the expert only to confirm/correct the interpretation. Do not transfer documentation burden to the reviewer.

**Exit criteria:** at least one decision-changing correction and a revised Smoke set.

---

## Phase 3 — First non-UT mapping/reproduction

**Goal:** prove that at least one case survives contact with a materially different architecture.

Choose the external system based on:

- active reviewer interest;
- architectural contrast with UT;
- setup cost;
- applicability of a Smoke case;
- likelihood of getting the interpretation checked by somebody who knows the system.

Start with **one case**, not a framework port.

### Required comparison discipline

- preserve system-native mechanisms;
- do not translate everything into UT terminology;
- document semantic differences;
- allow `NOT_APPLICABLE` with rationale;
- get interpretation reviewed before publishing comparative claims when feasible;
- record implementation effort as a descriptive observation, not a score.

**Exit criteria:** one materially different system has a reviewed mapping or executable result for at least one Smoke case, and the case format still makes sense.

---

## Phase 4 — Public MVP candidate (~5 cases)

**Goal:** expand only after evidence demonstrates missing dimensions.

Do not choose cases 4–5 from the original catalog merely to fill slots. Promote a new case only when feedback or evidence reveals a distinct phenomenon.

Public MVP requires:

- ~5 externally corrected cases;
- at least 3 substantive external reviews;
- at least one externally caused case revision/split/merge/delete/narrowing;
- two non-UT mappings where feasible;
- at least one executable non-UT result;
- at least one public UT limitation/trade-off;
- decision log;
- case versioning/registry;
- citation metadata.

**Do not build:** generalized runner, dashboard, leaderboard, architecture DSL.

---

## Phase 5 — Public feedback amplifier

Publish broader content only after the external-review gate is met.

Preferred framing leads with a concrete interaction, not UT superiority:

> "Two extensions work independently. What should happen when they meet?"

Article/post CTA:

> "Choose one case and tell us what your framework does — or why the case itself is wrong."

Success metrics:

- useful corrections;
- implementations;
- counterexamples;
- introductions to relevant researchers/maintainers.

Views, likes, stars and compliments are distribution metrics only.

---

## Phase 6 — LangDev field experiment

**Goal:** use the conference as a research instrument, not only a presentation venue.

Carry the same three Smoke cases unless external review has invalidated one.

### Conversation protocol

1. Present the scenario before showing outcome categories.
2. Ask what the system would do.
3. Ask where the decision is owned.
4. Ask which assumption in the case is wrong.
5. Record applicability, mechanism, phase, owner, objection and follow-up permission.

Do not ask "Would your framework pass this test?"

### Conference CTA

> **Classify one case — or tell me which assumption is wrong.**

### Conference success criterion

At least 5 conversations produce one or more of:

- concrete counterexample;
- system-native mechanism;
- principled N/A boundary;
- new acceptable outcome class;
- prior-art correction;
- case split/merge/delete;
- reproduction pointer;
- agreement to validate a written result.

A compliment that changes nothing does not count.

---

## Phase 7 — Research MVP

Expand to 6–8 reviewed cases and at least two materially different non-UT architecture families only if the earlier gates are healthy.

Possible result dimensions:

- applicability;
- detection/resolution phase;
- decision owner;
- policy explicitness;
- determinism under defined permutations;
- replayability/provenance;
- diagnostic actionability;
- host/base-language modifications required;
- semantic-oracle support;
- implementation effort.

Do not collapse dimensions into a leaderboard without an independently justified model.

---

## Phase 8 — Decide whether automation is justified

Only after repeated cross-system work decide whether duplication justifies:

- generalized case runner;
- JSON/YAML schema validation tooling;
- environment adapters;
- automated permutation generation;
- property/metamorphic testing;
- reducer/minimizer;
- result site/dashboard;
- composition-law DSL.

### DSL gate

Do not design a DSL until repeated cases across at least two materially different systems reveal stable primitives that are not UT-specific. If Markdown + code remains clearer, keep Markdown + code.

---

## Phase 9 — Publication decision

Choose the publication form based on evidence, not prestige first.

### If the strongest result is an externally corrected taxonomy + benchmark artifact

Target software-language-engineering venues/workshops/artifact tracks.

### If the strongest result is an empirical cross-system comparison

Target a full empirical/software-language-engineering paper.

### If the strongest result is a configuration-aware testing tool with real minimized defects

Make PlanFuzz primary and LCLS the minimized/canonical failure corpus; target testing/tool venues as appropriate.

### If external feedback shows the niche is already covered better

Pivot toward reproducing/extending the stronger prior work rather than defending the original framing.

That is a successful research outcome, not a failure.

---

# Immediate backlog

## P0 — now

- [x] Create `LC002 OrderStability`.
- [x] Create `LC004 IrrelevantExtensionNonInterference`.
- [x] Create `LC005 CrossLayerRealizationCompleteness`.
- [x] Add one current, revision-pinned UT evidence mapping for LC004.
- [x] Prepare a one-page Smoke reviewer packet.
- [x] Prepare a response template and decision log.
- [ ] Send five personalized review requests.
- [ ] Convert the first substantive objection into a documented decision.

## P1 — after first external correction

- [ ] Revise/drop/split cases based on feedback.
- [ ] Select the lowest-friction non-UT mapping from engaged reviewers.
- [ ] Implement or validate exactly one external case.
- [ ] Decide whether cases 4–5 are evidence-justified.

## Explicitly deferred

- generalized benchmark framework;
- web dashboard;
- scoring/leaderboard;
- architecture DSL;
- 8–12-case completion as a prerequisite to outreach;
- 30+ case catalog;
- mass outreach;
- challenge/competition proposal before Public MVP;
- monetization engineering.

---

# Decision checkpoints

At the end of every phase, answer:

1. What did we learn that changes the next step?
2. Which hypothesis became weaker?
3. What is the cheapest experiment that could falsify the current framing?
4. Are we building infrastructure because evidence requires it, or because it is technically attractive?
5. Has an external expert changed the artifact yet?

## Hard pivot review

A pivot review is mandatory if:

- two independent experts identify the same stronger prior work that substantially subsumes the central claim;
- three independent experts classify at least two of the three Smoke cases as `NOT_APPLICABLE` for principled architectural reasons;
- a case still requires UT-specific vocabulary after two external revisions.

Candidate pivot: narrow the comparative domain or make PlanFuzz/configuration-aware composition testing the primary artifact.

## Challenge-organization gate

Do not propose an LWC/ICST/public competition before:

- Public MVP exists;
- one non-UT executable result exists;
- at least two maintainers/researchers say the task is fair/useful.
