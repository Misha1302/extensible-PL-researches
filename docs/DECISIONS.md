# LCLS Decision Log

This log records material decisions that change a case, claim, taxonomy, applicability boundary, comparison target, or research direction.

A deleted or invalidated case is a successful research result when evidence shows the original abstraction was wrong.

## Decision template

```markdown
## D-YYYY-NNN — <short title>

**Date:** YYYY-MM-DD  
**Status:** proposed | accepted | rejected | superseded  
**Cases:** LCxxx  
**Raised by:** <person / review / experiment / prior art>  
**Attribution permission:** yes | no | pending

### Previous assumption

...

### Evidence / objection

...

### Alternatives considered

...

### Decision

...

### Effect on specification

...

### Evidence identity

- source/path:
- revision/version:
- locator:
- replay/artifact:
```

---

## D-2026-001 — External validity moves before corpus completion

**Date:** 2026-09-08  
**Status:** accepted  
**Cases:** LC002, LC004, LC005  
**Raised by:** adoption/research-strategy red-team review  
**Attribution permission:** n/a

### Previous assumption

Draft 8–12 case specifications, then implement the surviving set in UniversalToolchain, then conduct the first serious external review.

### Evidence / objection

Prior art already covers language-workbench challenge comparison, modular language composition, reliable independent composition/non-interference, and generic configuration/differential testing. The highest-risk remaining hypothesis is therefore not whether more cases can be written, but whether independent experts consider a small case set fair and portable.

### Alternatives considered

- keep the 8–12 case paper-first sequence;
- make PlanFuzz immediately primary;
- propose an existing-challenge subtrack immediately;
- first validate a three-case micro-challenge.

### Decision

Use a three-case Smoke MVP and seek decision-changing external criticism before broad UT implementation or corpus expansion.

### Effect on specification

`RESEARCH_SPEC.md` and `ROADMAP.md` now define Smoke/Public/Research MVP gates and make `NOT_APPLICABLE` / `INVALID_CASE` first-class evidence.

### Evidence identity

- source/path: `LCLS_MVP_ADOPTION_AND_RESEARCH_STRATEGY.md`
- project baseline: PR #1, branch `research-spec-roadmap-v1`
- baseline head at decision time: `1985907f42ae20ab404132843a9f26b607c41631`
