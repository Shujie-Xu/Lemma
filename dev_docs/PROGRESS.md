# Lemma — Development Progress

**Last updated:** February 2026

---

## Current State: M3 In Progress

Chapters 00 (Selection Bias and CIA), 01 (OLS Foundations), 02 (Large-Sample Theory), 03 (Instrumental Variables), and 04 (Difference-in-Differences) are fully written. Curriculum architecture v2 is active across navigation and docs.

---

## Milestone Tracker

### M0 — Project Scaffold ✅

| Deliverable | Status | Notes |
|---|---|---|
| `_quarto.yml` configuration | ✅ Done | Navbar, sidebar, light/dark themes, MathJax, execution freeze |
| Directory structure | ✅ Done | chapters/, problem-sets/, styles/, .github/workflows/, dev_docs/ |
| `styles/custom.scss` (light) | ✅ Done | Georgia body, IBM Plex Sans headings, functional colors, callout styling |
| `styles/custom-dark.scss` (dark) | ✅ Done | Full dark variant, math contrast, code blocks, callouts |
| `index.qmd` homepage | ✅ Done | Chapter map, feature overview, example proof block |
| Chapter placeholders (01–05) | ✅ Done | Each has motivation, "What You Will Learn", Coming Soon callout |
| `references.qmd` + `references.bib` | ✅ Done | Core textbooks + key papers with public version links |
| `problem-sets/` scaffold | ✅ Done | Index + pset-01 placeholder |
| `.github/workflows/publish.yml` | ✅ Done | Quarto render → GitHub Pages deployment |
| `README.md` | ✅ Done | Project overview, setup, structure |
| `.gitignore` | ✅ Done | _site/, .quarto/, OS files, IDE files |
| `quarto render` passes | ✅ Done | 9 files, clean build, dev_docs excluded |
| dev_docs/ documentation | ✅ Done | AGENTS.md, CONVENTIONS.md, PROGRESS.md |

**Acceptance Criteria Met:**
- [x] `quarto render` runs without errors
- [x] Light/dark theme toggle configured (cosmo/darkly)
- [x] dev_docs excluded from Quarto render

---

### M1 — Chapter 01 (OLS Foundations) ✅

**Target:** Full four-layer structure for OLS chapter.

| Deliverable | Status | Notes |
|---|---|---|
| Intuition section | ✅ Done | 3 paragraphs of plain prose motivation, no equations in first paragraph |
| Assumptions table (OLS.1–OLS.5) | ✅ Done | ID, Name, Statement, Role format + callout-warning on assumption strength |
| Theorem statements | ✅ Done | 5 theorems: OLS formula, unbiasedness, variance, Gauss-Markov, σ² estimation |
| Proof blocks (collapsed) | ✅ Done | 5 proofs, each with numbered steps referencing assumptions, ending with $\blacksquare$ |
| Shinylive interactive block | ✅ Done | Scatter + fitted line (OLS vs true) + residual histogram with normal overlay |
| Chapter summary table | ✅ Done | Theorem → Required Assumptions → Conclusion |
| Geometric interpretation | ✅ Done | Projection matrix, annihilator matrix, orthogonal decomposition |
| Shinylive extension installed | ✅ Done | quarto-ext/shinylive v0.2.0 |

**Content Structure (sections):**
1. Motivation (intuition, no equations first paragraph)
2. The Linear Model (matrix notation + assumptions table)
3. The OLS Estimator (FOC derivation + normal equations + geometric interpretation)
4. Unbiasedness (Theorem 1.2 + collapsed proof)
5. Gauss-Markov Theorem (variance + BLUE + collapsed proofs + warning on limitations)
6. Estimating the Error Variance (Theorem 1.5 + collapsed proof + standard errors)
7. Interactive Simulation (Shinylive: scatter fit + residual distribution)
8. Summary (5-row theorem table)

**Acceptance Criteria:**
- [x] All four layers present and properly formatted
- [x] Proof blocks use `.callout-tip collapse="true"` with `📐 Expand proof` title
- [x] Shinylive block is standalone with sliders + resample button
- [x] `quarto render` passes (1 expected warning: cross-file ref to IV chapter)
- [ ] Shinylive block runs in browser (needs `quarto preview` verification)
- [ ] Page deploys to GitHub Pages (needs CI/CD push)

---

### M1.5 — Curriculum Restructure ✅

| Deliverable | Status | Notes |
|---|---|---|
| Top-level taxonomy redesign | ✅ Done | Foundations of Causal Thinking / Estimation and Regression / Causal Designs / Advanced Inference |
| Navbar and sidebar updated | ✅ Done | `_quarto.yml` now includes new modules and links |
| New chapter placeholders added | ✅ Done | `00`, `06`, `07`, `08`, `09`, `10` created |
| Homepage chapter map updated | ✅ Done | `index.qmd` now reflects the new architecture |
| dev_docs synchronized | ✅ Done | `AGENTS.md`, `PROGRESS.md`, `CURRICULUM-ARCHITECTURE.md` updated |

**Acceptance Criteria:**
- [x] RCT explicitly included in Causal Designs
- [x] Synthetic Control added as frontier extension in Causal Designs
- [x] Standard Errors (robust/cluster/HAC) added as a standalone advanced chapter
- [x] OLS/MLE/GMM grouped into Estimation and Regression

---

### M2 — Chapter 02 + Problem Set 01 🔲

| Deliverable | Status |
|---|---|
| Chapter 00 (Selection Bias and CIA) full content | ✅ Done |
| Chapter 02 (Asymptotics) full content | ✅ Done |
| Problem Set 01 (OLS) problems + solutions | ✅ Done (Draft v1) |
| Full-text search functional | 🔲 Pending |
| CI/CD pipeline live | 🔲 Pending |
| `_freeze/` cache committed | 🔲 Pending |

---

### M3 — Chapters 03 + 04 🔲

| Deliverable | Status |
|---|---|
| Chapter 03 (IV) full content | ✅ Done |
| Chapter 04 (DiD) full content | ✅ Done |
| All P1 features delivered | 🔲 Pending |
| Bibliography with public links rendered | 🔲 Pending |

**Quality upgrades applied in this pass:**
- Expert sub-agent review policy added to dev docs.
- IV chapter revised after review: clearer instrument matrix notation (`Q=[Z\;W]`), exact 2SLS matrix estimator statement, stronger weak-IV caution, simulation aligned with projection-matrix 2SLS.
- DiD chapter completed from placeholder to full four-layer draft: assumptions table (DID.1-DID.4), 2x2 identification theorem with collapsible proof, event-study/staggered-adoption guidance, and standalone Shinylive simulation.
- Writing policy upgraded in dev docs: first-draft two-pass review loop (pre-draft + post-draft) and local knowledge-node/source-traceability conventions added.
- Local knowledge base initialized under `knowledge/` with source-card templates and first source nodes (MHE, Mixtape, Hansen) plus initial concept node (`parallel-trends`).
- Local corpus pipeline added (`scripts/knowledge_corpus.py`) for legal ingestion into chunked JSONL; initial open ingestion completed for Mixtape and authorized Hansen companion pages.

---

### M4 — Chapter 05 + P2 🔲

| Deliverable | Status |
|---|---|
| Chapter 05 (RDD) full content | 🔲 Pending |
| P2 feature exploration | 🔲 Pending |
| Site public announcement | 🔲 Pending |

---

## Open Decisions

| # | Question | Current Default |
|---|---|---|
| Q1 | External contributions? | Not yet decided |
| Q2 | R code support priority? | Python-only initially |
| Q3 | Stata code blocks format? | Static display with copy button (P1) |
| Q4 | Analytics? | Not yet decided |
| Q5 | Versioning strategy? | Git history as changelog |
