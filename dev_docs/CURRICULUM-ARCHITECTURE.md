# Lemma — Curriculum Architecture (v2)

This document defines the new instructional architecture after the curriculum redesign discussion.

---

## Why We Restructured

The previous structure mixed two logics:

- theory-first econometrics (OLS first)
- causal-thinking-first pedagogy (selection problem first)

To align with the project's goals (rigor + engagement), we now separate these explicitly.

---

## Top-Level Modules

1. **Foundations of Causal Thinking**
2. **Estimation and Regression**
3. **Causal Designs**
4. **Advanced Inference and Extensions**

This allows readers to first understand *why identification is hard*, then learn *how estimators work*, then apply designs, and finally handle inference pitfalls in realistic settings.

---

## Chapter Map (Current + Planned)

### Foundations of Causal Thinking

- `00-selection-bias-cia.qmd` — Selection bias, potential outcomes, CIA

### Estimation and Regression

- `01-ols.qmd` — OLS foundations (completed)
- `02-asymptotics.qmd` — Large-sample theory (planned)
- `09-mle.qmd` — Maximum likelihood estimation (planned)
- `10-gmm.qmd` — Generalized method of moments (planned)

### Causal Designs

- `06-rct.qmd` — Randomized controlled trials (planned)
- `03-iv.qmd` — Instrumental variables (planned)
- `04-did.qmd` — Difference-in-differences (planned)
- `05-rdd.qmd` — Regression discontinuity (planned)
- `08-synthetic-control.qmd` — Synthetic control (planned)

### Advanced Inference and Extensions

- `07-standard-errors.qmd` — Robust, cluster, HAC, and practical inference choices (planned)

---

## Placement Rules

- **OLS/MLE/GMM** belong in **Estimation and Regression** (estimator family logic).
- **RCT/IV/DiD/RDD/Synthetic Control** belong in **Causal Designs** (identification design logic).
- **Robust/Cluster/HAC and hypothesis-testing pitfalls** belong in **Advanced Inference**.
- **Selection bias/CIA/potential outcomes** belong in **Foundations of Causal Thinking**.

---

## Inter-Chapter Link Policy (Core UX Feature)

Lemma should provide "no-break" learning flow. Readers should never need to leave the site to find prerequisites.

This policy is principle-based, not a hardcoded link map. Authors and AI agents should create links using pedagogical judgment: add the right bridge where readers naturally need the next concept.

Authoring rule:

- Add links at first mention and at major theorem/assumption transitions, not only at chapter endings.

---

## Notes for AI Agents

- Preserve this module taxonomy in navbar, sidebar, index, and all future docs.
- When adding new chapters, maintain filename numbering and section anchors.
- Keep chapter format consistent with `dev_docs/CONVENTIONS.md`.
- Run the mandatory expert sub-agent review loop after each major writing iteration.
