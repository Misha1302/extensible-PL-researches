# UniversalToolchain reference evidence for LC004

**LCLS case:** `LC004 Irrelevant Extension Non-Interference`  
**Evidence type:** existing PlanFuzz seeded-fault / harness-adequacy evidence  
**Not:** a new LCLS execution, a newly discovered compiler defect, or proof of cross-system portability

## Pinned UniversalToolchain revision

- repository: `Misha1302/UniversalToolchain`
- branch observed: `master`
- revision: `40117eb68c630f7129c120aaaadc69be8f4ecbfb`
- commit message: `Make conversion ordering part of route-search feasibility (#369)`

## Existing PlanFuzz evidence

Pinned source:

`internal-docs/proposals/planfuzz/evidence/phase3-surface-oracles-smoke-summary.json`

At the pinned revision the record states:

- observation schema version: `4`;
- seeded fault: `SF-011-extension-noninterference`;
- oracle: `O-005-extension-noninterference` version `2`;
- mechanism: a **test-owned runtime provider** invokes extension-owned interference logic that activates its owner and changes the result;
- repeat: `3`;
- confirmed violation: `true`;
- flaky: `false`;
- inconclusive: `false`;
- infrastructure failure: `false`;
- case ID: `df68aae93e53188a714a49d2c781df97b59e95a694c8596eaf2ce212c0bb539c`;
- exact fingerprint: `d4f4021a581a3dacd432aa85e97e602e586da371e9911b2b0757063e184923f3`;
- replay report SHA-256: `d9ba482e97b95b960c22bc0a7e496d794389a5c2200b7cd4d0cd906683097db0`;
- manifest SHA-256: `149c95e1bee828242041544f3c28cebe82341276fb42da63d76b602f753fa5b2`.

Pinned evidence URL:

https://github.com/Misha1302/UniversalToolchain/blob/40117eb68c630f7129c120aaaadc69be8f4ecbfb/internal-docs/proposals/planfuzz/evidence/phase3-surface-oracles-smoke-summary.json

Relevant implementation surfaces at the same revision include:

- `UniversalToolchain.PlanFuzz.Core/ExtensionNoninterferenceOracle.cs`
- `UniversalToolchain.PlanFuzz.Core/PlanFuzzOracleIds.cs`
- `UniversalToolchain.PlanFuzz.Adapter.Acme/AcmePlanFuzzConstants.cs`
- `UniversalToolchain.PlanFuzz.Tests/AcmePlanFuzzOracleTests.cs`

## Mapping to LC004

The existing PlanFuzz experiment gives LC004 a concrete **UT-side observation shape**:

```text
base observation
+ independent/unselected extension mutation
+ explicit owner/activation evidence
+ result comparison
+ oracle verdict
+ repeat confirmation
+ exact replay/fingerprint provenance
```

That is useful for LCLS because it demonstrates that LC004 can bind an interference claim to runtime-produced evidence rather than a prose assertion.

It does **not** establish that another architecture should define “irrelevant extension” in the same way. External review must attack that boundary.

## Claim boundary

The source evidence explicitly says the faults are test-owned seeded faults and **do not count as discovered compiler defects**. It also states that lifecycle/session/concurrency schedules, equal-budget superiority, publication novelty, and production certification remain unverified.

LCLS preserves that boundary.

## Replay status

This repository does not copy or claim to have freshly executed the UT campaign. It pins the upstream revision and the upstream replay identities. A future LCLS execution may add a clean-room command transcript here, but that is a separate evidence event.
