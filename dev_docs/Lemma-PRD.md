# Lemma — Product Requirements Document

**Version:** 1.0  
**Date:** February 2026  
**Author:** Shujie  
**Status:** Draft

---

## Table of Contents

1. [Project Overview](#1-project-overview)
2. [Design Principles](#2-design-principles)
3. [Functional Requirements](#3-functional-requirements)
4. [Content Plan](#4-content-plan)
5. [Technical Architecture](#5-technical-architecture)
6. [Non-Functional Requirements](#6-non-functional-requirements)
7. [Delivery Milestones](#7-delivery-milestones)
8. [Open Questions](#8-open-questions)

---

## 1. Project Overview

### 1.1 Name

The project is named **Lemma**.

In mathematics, a *lemma* is a proven intermediate result established before—and in service of—a larger theorem. The name captures the site's philosophy: every concept is a building block. Readers accumulate lemmas until the full architecture of causal inference becomes visible.

The word is short, language-neutral, and carries immediate academic register without being discipline-exclusive.

### 1.2 Positioning

Lemma is an open-source econometrics learning site built on Quarto. It targets the gap between formal textbooks (dense, proof-heavy) and applied guides (light on rigor). Its two defining features are:

**Narrative-first proofs.** The prose tells a coherent mathematical story. Detailed derivations are collapsed by default; readers expand them on demand. The page never becomes a wall of equations.

**In-browser interactive code.** Each key result is accompanied by a Shinylive code block that runs Python directly in the browser via WebAssembly. No installation required. Readers adjust parameters, press Run, and watch the theorem come alive.

The site serves two audiences simultaneously: the author's personal knowledge archive, and a public learning resource.

### 1.3 Target Users

| User | Background | Core Need |
|---|---|---|
| Economics graduate students | MA/PhD coursework in econometrics | Quick reference for definitions, assumptions, and proofs; exam review |
| Economics predocs | Recent graduates preparing for RA positions | Understand identification strategies; see working code implementations |
| Author | Active researcher in empirical economics | Accumulate a searchable, citable personal notes system |
| General readers | Quantitative social scientists, statisticians | Accessible entry to econometric theory beyond cookbook recipes |

---

## 2. Design Principles

### 2.1 Content Layers: Intuition → Formalization → Proof → Code

Every section follows a strict four-layer structure. The layers are progressive, not parallel.

| Layer | Form | Role | Default State |
|---|---|---|---|
| ① Intuition | Prose, analogy | Explain *why* before *what* | Fully visible |
| ② Formalization | Definitions, assumptions, theorem statement | The precise mathematical claim | Fully visible |
| ③ Proof | Step-by-step derivation, matrix algebra | The verification | **Collapsed by default** |
| ④ Interactive code | Runnable Python simulation | Experiential confirmation | Visible, ready to run |

The proof layer is collapsed not because it is unimportant, but because forcing readers through every derivation before they grasp the result is pedagogically backwards. Readers who want the proof can always get it; readers who do not are not penalized.

### 2.2 Visual Design

Lemma is an academic tool. The visual language is deliberate, restrained, and information-dense without being cluttered.

**No decorative elements.** No illustrations, icon grids, gradient banners, or background textures. Every visual element earns its place by serving comprehension.

**Color is functional, not expressive.** Color distinguishes content types (proof blocks, definition blocks, interactive blocks) and does not carry emotional register.

**Typography.** Body text and equations use a serif face (Georgia / MathJax). Code uses a monospace face (IBM Plex Mono). Navigation and headings use a sans-serif face (IBM Plex Sans). The combination is legible, academic, and unsurprising—which is exactly right for a reference site.

**Generous whitespace.** Line height ≥ 1.8. Adequate paragraph spacing. The page should breathe; it must not feel like a scanned PDF.

**Light/dark themes.** Both themes fully support all block types. User preference persists across sessions.

### 2.3 Interaction Philosophy

Interaction exists solely to reduce the cost of understanding. It does not exist to make the page feel modern.

The following interaction patterns are **prohibited:**

- Decorative animations or transitions
- Modals, pop-overs, or forced attention-grabbing overlays
- Login walls or registration requirements for core content
- Infinite scroll or pagination that fragments a chapter

Every interactive element must satisfy three conditions: zero installation (WebAssembly); results immediately visible after action; silent graceful degradation if it fails—reading must never be blocked.

### 2.4 Citation and Reference Philosophy

Econometrics is taught from a small number of canonical texts. But those texts are expensive, and many foundational papers are locked behind journal paywalls. Where a public version exists—a working paper, NBER preprint, SSRN draft, author's website, or arXiv posting—the citation should link to it directly.

This serves the site's stated purpose: a one-stop learning resource. A citation without an accessible link is an incomplete citation for a reader who does not have institutional access.

**Convention:** Every bibliographic entry that has a public version includes a `[PDF]` or `[Working Paper]` link immediately after the citation metadata. If the canonical published version differs materially from the working paper (e.g., identification strategy changed), a brief note is added.

---

## 3. Functional Requirements

Priority levels:
- **P0** — Must ship in the initial release
- **P1** — Target for initial release; deferrable if necessary
- **P2** — Future versions

### 3.1 P0: Core Features

#### Collapsible proof blocks

Proof blocks use Quarto's native `callout` with `collapse: true`. Default state is collapsed. The block title follows the pattern `📐 Expand proof`. The body ends with `$\blacksquare$`. Multi-step proofs may use a nested ordered list with explicit step labels (`Step 1`, `Step 2`, …). The collapsed state shows only the title bar; no partial content is visible.

```markdown
::: {.callout-tip collapse="true"}
## 📐 Expand proof

**Step 1.** Substitute $Y = X\beta + \varepsilon$ into the estimator...

...

$\blacksquare$
:::
```

#### Interactive code blocks (Shinylive)

Interactive blocks use `shinylive-python` with `standalone: true`. The Python Shiny app runs entirely in the browser via Pyodide/WebAssembly; no server is required. Each block includes:

- Parameter controls (sliders, dropdowns) with English labels
- A **Run / Resample** button
- A matplotlib figure output with labeled axes and a descriptive title
- Brief inline comments explaining what each parameter controls

The block should be self-explanatory to a reader who has not read the surrounding prose—someone who lands directly on the figure should understand what they are looking at.

```markdown
```{shinylive-python}
#| standalone: true
#| viewerHeight: 520
...
```
```

#### Mathematical typesetting

- MathJax 3 for LaTeX rendering
- Inline math: `$...$`
- Display math: `$$...$$`
- Equation numbering: `\tag{n}` for manual numbering; `\label{}` / `\eqref{}` for cross-references
- Equations that overflow on mobile scroll horizontally rather than wrapping

#### Full-text search

- Quarto built-in search, accessible from the navbar
- Covers headings, body text, and code comments
- Results highlight the matched term in context
- Search overlay dismisses with `Escape`

#### Light/dark theme toggle

- Two complete themes: `cosmo` (light) and `darkly` (dark)
- Toggle button in the top-right of the navbar
- User preference stored in `localStorage` and restored on next visit
- Both themes: proof blocks readable, code syntax highlighting correct, math contrast sufficient

#### Navigation structure

- **Navbar (top):** Module-level groupings (Foundations, Causal Inference, Problem Sets, References), GitHub link
- **Sidebar (left):** All chapters within the current module, collapsible at section level
- **TOC (right):** Current page outline, depth ≤ 3, sticky on scroll
- **Footer:** "Edit this page on GitHub" link, pointing to the source `.qmd` file

### 3.2 P1: Extended Features

#### Stata code blocks

Non-executed, syntax-highlighted code blocks for Stata. A clearly visible note reads: *"Stata cannot run in the browser. Copy this code into your Stata session."* The block has a copy button.

#### GitHub edit links

Each page footer includes a direct link to the source file on GitHub (`repo-actions: [edit, issue]` in `_quarto.yml`). The link opens the GitHub editor; readers can submit corrections via pull request without leaving the browser.

#### Bibliography and citations

- `references.bib` (BibTeX) as the citation source, processed by Pandoc citeproc
- Inline citations: `@wooldridge2010` or `[@wooldridge2010, p. 52]`
- Reference list auto-generated at the end of each chapter
- **Every entry with a public version includes a linked `[PDF]` or `[Working Paper]` tag** (see Section 4.3)
- CSL style: Chicago author-date (`chicago-author-date.csl`)

#### Problem sets module

- Housed in `problem-sets/` directory
- Each problem set has an index page listing problems with difficulty indicators
- Analytical problems: question visible, solution collapsed behind a callout block
- Computational problems: question + interactive code block for verification

#### Build cache

`execute: freeze: auto` in `_quarto.yml`. Code blocks re-execute only when the source `.qmd` file changes. The `_freeze/` directory is committed to Git to make CI builds fast and reproducible.

### 3.3 P2: Future Features

| Feature | Description |
|---|---|
| Reading progress | Per-chapter read/unread status stored in `localStorage` |
| Annotation | Local paragraph highlighting and personal notes, stored client-side |
| PDF export | Per-chapter export with static code blocks and full math rendering |
| R support | R code blocks alongside Python, using WebR for browser execution |
| i18n | Chinese/English toggle for bilingual readers |

---

## 4. Content Plan

### 4.1 Chapter Structure

The site is organized into two modules. Within each module, chapters follow the order: Foundations first, then applications.

| # | Title | Core Content | Interactive Code Theme |
|---|---|---|---|
| 01 | OLS Foundations | Model setup, normal equations, unbiasedness, Gauss-Markov theorem, variance estimation | Adjustable OLS scatter fit with residual distribution |
| 02 | Large-Sample Theory | LLN, CLT, consistency, asymptotic normality, Delta method | Sample size vs. sampling distribution shape |
| 03 | Instrumental Variables | Sources of endogeneity, exclusion restriction, 2SLS derivation, weak instruments | IV vs. OLS estimates across instrument strength |
| 04 | Difference-in-Differences | Parallel trends, event study design, heterogeneous treatment effects, staggered adoption | Monte Carlo: policy intensity vs. DiD estimator |
| 05 | Regression Discontinuity | Sharp/Fuzzy RDD, local linear regression, bandwidth selection, McCrary density test | Bandwidth parameter vs. RDD estimate precision |

### 4.2 Chapter Writing Conventions

To ensure cross-chapter consistency, all chapters follow these conventions.

**Opening.** Every chapter begins with 1–2 paragraphs of intuitive motivation in plain prose. No equations in the first paragraph.

**Assumptions.** Assumptions are enumerated (e.g., OLS.1–OLS.5) and presented in a table with columns: ID, Name, Statement, Role in proof.

**Theorems and propositions.** Stated in a definition callout block with a clear label and number.

**Proofs.** Collapsed by default. Each step references the assumption it uses. The proof ends with `$\blacksquare$`. Steps are numbered if the proof has more than three distinct moves.

**Interactive blocks.** Each block is self-contained (`standalone: true`). Comments explain parameters in English. Figure titles and axis labels use full words, not abbreviations. The first line of the block is a one-line description comment: `# Simulation: [what this shows]`.

**Chapter summary.** Every chapter ends with a summary table: theorem name, required assumptions, conclusion.

**Cross-references.** Internal section links use `@sec-` anchors. Citations use `@key` referencing `.bib` entries.

### 4.3 Citation and Reference Conventions

Each bibliographic entry follows this format:

```
Author, First. (Year). *Title*. Publisher/Journal. 
[[PDF](url)] [[Working Paper](url)] [[DOI](url)]
```

For working papers and textbooks with public drafts:

| Entry | Public Version |
|---|---|
| Hansen (2022), *Econometrics* | [Full PDF on author's website](https://www.ssc.wisc.edu/~bhansen/econometrics/) |
| Angrist & Pischke (2009), *Mostly Harmless* | No free version; link to [Amazon](https://www.amazon.com/dp/0691120358) |
| Cunningham (2021), *Causal Inference: The Mixtape* | [Free web version](https://mixtape.scunning.com/) |
| Huntington-Klein (2021), *The Effect* | [Free web version](https://theeffectbook.net/) |
| Callaway & Sant'Anna (2021), DiD paper | [SSRN working paper](https://ssrn.com/abstract=3460877) |
| Calonico, Cattaneo & Titiunik (2014), RDD paper | [Author's website PDF](https://cattaneo.princeton.edu/papers/Calonico-Cattaneo-Titiunik_2014_ECMA.pdf) |

If a working paper version contains a known material difference from the published version (e.g., a different identification approach, additional appendix content), a brief parenthetical note is added after the link.

---

## 5. Technical Architecture

### 5.1 Technology Stack

| Layer | Choice | Rationale |
|---|---|---|
| Static site generator | Quarto ≥ 1.5 | Native LaTeX, code execution, callouts, multi-format output; the de facto standard for academic publishing |
| Interactive code | Shinylive + Shiny for Python | WebAssembly execution, zero server cost, mature widget ecosystem |
| Math rendering | MathJax 3 | Best LaTeX compatibility; supports automatic equation numbering and `\eqref` cross-references |
| Style system | SCSS → Bootstrap 5 | Quarto's built-in theme system; deep customization via variable overrides only |
| Body typeface | Georgia (system) | Zero dependency; excellent math-adjacent readability |
| UI typeface | IBM Plex Sans/Mono | Open source; self-hostable; consistent across platforms |
| Hosting | GitHub Pages | Free; integrates with `quarto publish gh-pages` |
| Version control | Git + GitHub | Supports `repo-actions: [edit, issue]`; readers can submit corrections via PR |

### 5.2 Directory Structure

```
lemma/
├── _quarto.yml              # Site config: navigation, themes, execution options
├── index.qmd                # Homepage: chapter map, feature demo
├── references.qmd           # Annotated bibliography with public version links
├── references.bib           # BibTeX entries
├── chapters/
│   ├── 01-ols.qmd
│   ├── 02-asymptotics.qmd
│   ├── 03-iv.qmd
│   ├── 04-did.qmd
│   └── 05-rdd.qmd
├── problem-sets/
│   ├── index.qmd
│   └── pset-01.qmd
├── styles/
│   ├── custom.scss          # Light theme overrides
│   └── custom-dark.scss     # Dark theme overrides
├── _freeze/                 # Code execution cache (committed to Git)
└── .github/
    └── workflows/
        └── publish.yml      # CI: build and deploy to gh-pages
```

### 5.3 Key Configuration (`_quarto.yml`)

```yaml
project:
  type: website
  output-dir: _site

website:
  title: "Lemma"
  repo-url: https://github.com/your-username/lemma
  repo-actions: [edit, issue]

format:
  html:
    theme:
      light: [cosmo, styles/custom.scss]
      dark:  [darkly, styles/custom-dark.scss]
    toc: true
    toc-depth: 3
    number-sections: true
    html-math-method: mathjax
    code-copy: true
    smooth-scroll: true

execute:
  freeze: auto
  warning: false

filters:
  - shinylive

bibliography: references.bib
csl: styles/chicago-author-date.csl
```

### 5.4 Build and Deployment

**Local development:**

```bash
quarto add quarto-ext/shinylive   # one-time setup
quarto preview                    # hot-reload local server
```

**CI/CD (GitHub Actions):**

1. Push to `main` triggers `publish.yml`
2. Uses `quarto-actions/publish@v2` with `target: gh-pages`
3. `_freeze/` is committed to the repo; CI reads the cache instead of re-executing Python
4. Build artifact deploys to `gh-pages` branch; GitHub Pages serves it automatically

**Manual deploy:**

```bash
quarto render
quarto publish gh-pages
```

### 5.5 Shinylive Block Specification

Every interactive block must satisfy:

- `standalone: true` — self-contained, no external app server
- `viewerHeight` — set explicitly (400–580px depending on content)
- Package imports via `micropip.install()` at the top if needed
- All text-facing strings (labels, titles, button text) in English
- Figure must render correctly at 700px wide (Quarto default content width)
- `np.random.seed(None)` or equivalent so reruns produce fresh samples
- Wrap the app in a `try/except` so Pyodide import errors produce a readable message rather than a blank block

---

## 6. Non-Functional Requirements

### 6.1 Performance

- **LCP < 2.5 seconds** on a 4G connection (excluding Shinylive WebAssembly initialization)
- **Shinylive cold start:** 3–8 seconds acceptable for first execution (WebAssembly environment load); subsequent interactions must respond in < 500ms
- **Build time:** Full site render from cold cache < 5 minutes; with `_freeze/` warm cache < 60 seconds
- **Page weight:** Aim for < 500KB transferred per page excluding Shinylive bundle (loaded lazily on demand)

### 6.2 Accessibility

- MathJax outputs include `aria-label` attributes by default (MathJax 3 behavior)
- Color contrast for all body text and code: WCAG AA (≥ 4.5:1)
- Collapsible proof blocks operable via keyboard (`Enter` / `Space` on the title bar)
- Navigation landmark elements (`<nav>`, `<main>`, `<footer>`) present in the rendered HTML
- Page titles follow the pattern `Chapter Title | Lemma` for screen reader context

### 6.3 Maintainability

- Adding a chapter requires only: (1) create `.qmd` in `chapters/`, (2) add one line to `_quarto.yml` sidebar. No other files need to change.
- All style changes go through `styles/custom.scss`. The base Quarto theme files are never modified directly.
- Shinylive blocks are fully self-contained (`standalone: true`). They do not depend on any external Python environment or installed packages.
- The `_freeze/` cache is committed to Git. A contributor can clone the repo, run `quarto preview`, and see a working site without re-executing any Python.

### 6.4 Openness

- All content is licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) unless otherwise noted
- Source code (Quarto infrastructure, SCSS, JS) is MIT licensed
- The public version link convention (Section 4.3) is itself a commitment: Lemma will not link to paywalled content as the primary access route when a free version exists

---

## 7. Delivery Milestones

| Phase | Timeline | Deliverables | Acceptance Criteria |
|---|---|---|---|
| M0 | Now | Project scaffold: `_quarto.yml`, directory structure, SCSS themes, `README.md` | `quarto preview` runs without errors; light/dark toggle works |
| M1 | Weeks 1–2 | Chapter 01 (OLS): full four-layer structure | Proof collapses/expands correctly; Shinylive block runs in browser; page deploys to GitHub Pages |
| M2 | Weeks 3–4 | Chapter 02 (Asymptotics) + Problem Set 01 | Full-text search functional; CI/CD pipeline live; `_freeze/` cache committed |
| M3 | Weeks 6–8 | Chapters 03 (IV) + 04 (DiD) | All P1 features delivered; bibliography with public version links rendered correctly |
| M4 | TBD | Chapter 05 (RDD) + P2 feature exploration | All five chapters complete; site announced publicly |

---

## 8. Open Questions

These questions are deferred but must be resolved before M3.

| # | Question | Options |
|---|---|---|
| Q1 | Accept external contributions (PRs)? | **A:** Fully open with `CONTRIBUTING.md` and issue templates. **B:** Issues only; author maintains content independently. |
| Q2 | Priority of R code support? | **A:** Parallel with Python from M1, using WebR for browser execution. **B:** Python-only initially; R added on demand. |
| Q3 | How to handle Stata code blocks? | **A:** Static display with a copy button and a note explaining browser limitations. **B:** Link to a Binder/MyBinder remote environment for full execution. |
| Q4 | Google Analytics or equivalent? | **A:** Enable; understand reader geography and chapter engagement. **B:** Disable; respect reader privacy, no tracking. |
| Q5 | Versioning strategy for content? | **A:** Semantic versioning (v1.0, v1.1) with a changelog page. **B:** Git commit history as the implicit changelog; no explicit versioning. |

---

*Lemma · Product Requirements Document · v1.0 · February 2026*
