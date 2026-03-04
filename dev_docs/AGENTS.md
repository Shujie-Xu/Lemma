# Lemma — AI Agent Development Guide

**Purpose:** This document provides AI agents with all context needed to continue development across sessions.

---

## Project Identity

- **Name:** Lemma
- **Type:** Open-source econometrics learning site (Quarto static site)
- **Author:** Shujie
- **Status:** M3 in progress. Chapters 00, 01, 02, 03, 04 and Problem Set 01 (draft v1) complete; next major writing target is Chapter 05 (RDD).

## Tech Stack

| Layer | Technology |
|---|---|
| Static site generator | Quarto 1.8+ |
| Interactive code | Shinylive + Shiny for Python (WebAssembly) |
| Math rendering | MathJax 3 |
| Style system | SCSS → Bootstrap 5 (cosmo light / darkly dark) |
| Typography | Georgia (body), IBM Plex Sans (headings/nav), IBM Plex Mono (code) |
| Hosting | GitHub Pages |
| CI/CD | GitHub Actions (`quarto-actions/publish@v2`) |
| Bibliography | BibTeX + Pandoc citeproc |

## Directory Structure

```
lemma/
├── _quarto.yml              # Site config (navigation, themes, execution)
├── index.qmd                # Homepage
├── references.qmd           # Annotated bibliography
├── references.bib           # BibTeX entries
├── chapters/
│   ├── 00-selection-bias-cia.qmd  # Selection Bias and CIA
│   ├── 01-ols.qmd           # OLS Foundations
│   ├── 02-asymptotics.qmd   # Large-Sample Theory
│   ├── 03-iv.qmd            # Instrumental Variables
│   ├── 04-did.qmd           # Difference-in-Differences
│   ├── 05-rdd.qmd           # Regression Discontinuity
│   ├── 06-rct.qmd           # Randomized Controlled Trials
│   ├── 07-standard-errors.qmd  # Robust/Cluster/HAC inference
│   ├── 08-synthetic-control.qmd # Synthetic Control
│   ├── 09-mle.qmd           # Maximum Likelihood Estimation
│   └── 10-gmm.qmd           # Generalized Method of Moments
├── problem-sets/
│   ├── index.qmd            # Problem set overview
│   └── pset-01.qmd          # OLS problem set
├── styles/
│   ├── custom.scss           # Light theme overrides
│   └── custom-dark.scss      # Dark theme overrides
├── dev_docs/                 # AI agent docs (NOT rendered by Quarto)
│   ├── AGENTS.md             # This file — agent development guide
│   ├── Lemma-PRD.md          # Product Requirements Document
│   ├── CONVENTIONS.md        # Writing and code conventions
│   ├── PROGRESS.md           # Milestone progress tracker
│   └── CURRICULUM-ARCHITECTURE.md  # Curriculum structure decisions
├── .github/workflows/
│   └── publish.yml           # CI/CD pipeline
├── .gitignore
└── README.md
```

## Key Commands

```bash
quarto preview              # Hot-reload local dev server
quarto render               # Full build → _site/
quarto publish gh-pages     # Deploy to GitHub Pages
quarto add quarto-ext/shinylive  # Install Shinylive extension (one-time)
```

## Content Architecture

Every chapter follows a **four-layer structure**:

1. **Intuition** — Plain prose, no equations in first paragraph
2. **Formalization** — Definitions, assumptions table, theorem statement
3. **Proof** — Step-by-step derivation, collapsed by default
4. **Interactive code** — Shinylive Python simulation, standalone

## Critical Conventions

- **Proof blocks:** Use `.callout-tip` with `collapse: true`. Title: `📐 Expand proof`. End with `$\blacksquare$`.
- **Theorem blocks:** Use `.callout-note` with clear label and number.
- **Assumptions:** Enumerated table (ID, Name, Statement, Role in proof).
- **Interactive blocks:** `shinylive-python` with `standalone: true`. Self-explanatory. English labels.
- **Citations:** `@key` format. Every entry with public version gets a `[PDF]` link.
- **Cross-refs:** `@sec-` anchors for internal links.
- **Frictionless learning links:** when a concept depends on another chapter, add an inline cross-chapter link immediately where the concept appears, using contextual judgment rather than fixed link rules.

## Milestones

| Phase | Status | Deliverables |
|---|---|---|
| M0 | ✅ Complete | Project scaffold, themes, directory structure |
| M1 | ✅ Complete | Chapter 01 (OLS): 5 theorems, 5 proofs, Shinylive simulation |
| M1.5 | ✅ Complete | Curriculum v2 restructure (Foundations / Estimation / Causal Designs / Advanced Inference) |
| M2 | 🔲 Next | Fill causal-design chapters and stabilize cross-chapter links |
| M3 | 🔲 In Progress | Fill causal designs (RCT, IV, DiD, RDD, Synthetic Control) |
| M4 | 🔲 Planned | Fill MLE/GMM + standard errors chapter + P2 features |

## For AI Agents: How to Continue

1. **Read this file first** to understand project state.
2. **Check `PROGRESS.md`** for detailed milestone status.
3. **Read `CONVENTIONS.md`** before writing any content.
4. **Read `Lemma-PRD.md`** for full product requirements.
5. **Always run `quarto render`** after changes to verify build.
6. **Never modify** base Quarto theme files — only `styles/custom.scss` and `styles/custom-dark.scss`.
7. **dev_docs/ is excluded from Quarto render** — safe to add `.md` files here.
8. **Minimize reader friction** — default to proactive cross-chapter linking so readers can continue learning without leaving the site.
9. **Mandatory sub-agent review for writing** — for each substantial writing pass, call one high-quality review sub-agent in economist + website-advisor mode, have it read dev docs + edited sections, then revise based on findings.
10. **First-draft writing uses a two-pass review loop** — for new chapters or major rewrites, run one pre-draft design review (outline/assumption checks) and one post-draft review (content quality checks).
11. **Use source-grounded local notes for writing** — when external references are used, maintain local source nodes with citation metadata and access status before or during drafting.

Review output handling rule:
- For each major review suggestion, either apply a concrete revision or record a brief rationale for not applying it.

---

## New Session Bootstrap (Mandatory)

Use this exact startup checklist at the beginning of every new session so a new model can take over immediately.

### Step 0: Read Order (do not skip)

1. `dev_docs/AGENTS.md` (state + workflow)
2. `dev_docs/PROGRESS.md` (what is done vs pending)
3. `dev_docs/CONVENTIONS.md` (writing/math/code rules)
4. `dev_docs/CURRICULUM-ARCHITECTURE.md` (module taxonomy + placement logic)
5. `dev_docs/Lemma-PRD.md` (product scope and goals)

### Step 1: Extract Non-Negotiable Rules

Before editing, explicitly internalize these rules:

- Keep chapter structure in 4 layers: intuition -> formalization -> proof -> interactive code.
- Proofs must be collapsible callouts ending with `$\blacksquare$`.
- Use contextual cross-chapter links to minimize user friction.
- New chapters and major rewrites require two reviews: one pre-draft design review and one post-draft quality review.
- After review, apply major suggestions or record rationale for rejecting them.
- External-source-backed writing must be traceable to local source notes with citation metadata.

### Step 2: Verify Technical Baseline

Run:

```bash
quarto render
```

If render fails, fix build first. Do not continue content work on a broken build.

### Step 3: Working Loop for Writing Tasks

For each chapter/problem-set update:

1. Build a short outline and assumptions map.
2. Run pre-draft review (econometrician + website advisor role) on outline/identification strategy.
3. Draft/update content.
4. Run post-draft review (econometrician + website advisor role) on full text.
5. Revise based on both review passes.
6. Re-run `quarto render`.
7. Update `dev_docs/PROGRESS.md` and this file's status if milestone state changed.
8. If external references were used, synchronize local source-note metadata (access type, citation keys, URLs, and page anchors where applicable).

### Step 4: Handoff Quality Bar

A session is not "done" unless all are true:

- Edited pages render successfully.
- New text follows `CONVENTIONS.md`.
- Cross-chapter links are added where readers naturally need them.
- Pre-draft and post-draft review loops were executed and addressed (for substantial writing).
- Progress docs were synchronized.
- External references are traceable to local source notes.
