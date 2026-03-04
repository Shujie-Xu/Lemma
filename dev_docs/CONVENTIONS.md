# Lemma — Writing and Code Conventions

**Purpose:** Ensure cross-chapter consistency. Every AI agent and human contributor must follow these conventions.

---

## Chapter Structure

### Opening
- 1–2 paragraphs of intuitive motivation in plain prose
- **No equations in the first paragraph**
- Explain *why* before *what*

### Assumptions Table
Every chapter with formal results includes an assumptions table:

```markdown
| ID | Name | Statement | Role in Proof |
|---|---|---|---|
| OLS.1 | Linearity | $Y = X\beta + \varepsilon$ | Model specification |
| OLS.2 | Random sampling | $(Y_i, X_i)$ i.i.d. | Independence across obs |
| ... | ... | ... | ... |
```

### Theorem/Definition Blocks
Use `.callout-note`:

```markdown
::: {.callout-note}
## Theorem 1.1 (Unbiasedness of OLS)

Under assumptions OLS.1–OLS.4, the OLS estimator is unbiased:
$$\mathbb{E}[\hat{\beta} \mid X] = \beta$$
:::
```

### Proof Blocks
Use `.callout-tip` with `collapse: true`:

```markdown
::: {.callout-tip collapse="true"}
## 📐 Expand proof

**Step 1.** Begin with the OLS estimator...

**Step 2.** Substitute the model...

**Step 3.** Apply the exogeneity assumption...

$\blacksquare$
:::
```

Rules:
- Default state: **collapsed**
- Title: always `📐 Expand proof` (or `📐 Expand proof of Theorem X.Y`)
- End with `$\blacksquare$`
- Number steps if proof has > 3 distinct moves
- Each step references the assumption it uses

### Interactive Code Blocks

```markdown
```{shinylive-python}
#| standalone: true
#| viewerHeight: 520

# Simulation: [one-line description of what this shows]

from shiny import App, ui, render, reactive
import numpy as np
import matplotlib.pyplot as plt

# ... app code ...
```
```

Rules:
- Always `standalone: true`
- Set `viewerHeight` explicitly (400–580px)
- First line: `# Simulation: [description]`
- All labels, titles, buttons in **English**
- Figure axes: full words, not abbreviations
- `np.random.seed(None)` for fresh samples on each run
- Wrap in `try/except` for graceful degradation
- Self-explanatory to someone who hasn't read the prose

### Chapter Summary
Every chapter ends with a summary table:

```markdown
## Summary

| Result | Required Assumptions | Conclusion |
|---|---|---|
| Theorem 1.1 | OLS.1–OLS.4 | $\hat{\beta}$ is unbiased |
| Theorem 1.2 | OLS.1–OLS.5 | $\hat{\beta}$ is BLUE |
```

---

## Math Conventions

- Inline math: `$...$`
- Display math: `$$...$$`
- Equation numbering: `\tag{n}` for manual, `\label{}`/`\eqref{}` for cross-ref
- Matrices: bold uppercase ($\mathbf{X}$, $\mathbf{M}$)
- Vectors: bold lowercase ($\boldsymbol{\beta}$, $\boldsymbol{\varepsilon}$)
- Scalars: italic ($n$, $k$, $\sigma^2$)
- Estimators: hat notation ($\hat{\beta}$, $\hat{\sigma}^2$)
- Expectations: $\mathbb{E}[\cdot]$, $\text{Var}(\cdot)$, $\text{Cov}(\cdot, \cdot)$
- Probability: $\mathbb{P}(\cdot)$
- Convergence: $\xrightarrow{p}$ (in probability), $\xrightarrow{d}$ (in distribution)

---

## Cross-Reference Conventions

- Section anchors: `{#sec-topic-subtopic}` (e.g., `{#sec-ols-unbiasedness}`)
- Refer with: `@sec-ols-unbiasedness`
- Figure/table refs: `@fig-name`, `@tbl-name`
- Citations: `@key` or `[@key, p. 52]`

### Cross-Chapter Learning Links (Mandatory)

Lemma's core UX goal is **frictionless immersion**: readers should move to the next needed concept without leaving the site or searching manually.

Rules:
- Every chapter must include 4-8 contextual links to other chapters when concepts depend on each other.
- Links should appear at the exact moment of need (inline or in a short callout), not only in a "Further Reading" block.
- Prefer section-anchor links (`@sec-...`) when the target section exists; otherwise link to the target chapter file.
- Use descriptive link text (for example, "robust standard errors"), not "click here".

Judgment rule:
- Do not hardcode fixed cross-link pairs in prose templates.
- Authors and agents should decide links contextually based on the reader's immediate next question.
- When in doubt, prefer the link that reduces reader friction the most in that paragraph.

Preferred writing patterns:

```markdown
Under heteroskedasticity, classical standard errors are invalid; see [Standard Errors in Practice](07-standard-errors.qmd) for robust, cluster, and HAC alternatives.
```

```markdown
Binary-outcome models are introduced in [Maximum Likelihood Estimation](09-mle.qmd), where logit and probit are derived from first principles.
```

```markdown
This identification argument relies on CIA; see [Selection Bias and CIA](00-selection-bias-cia.qmd).
```

---

## Code Style (Python in Shinylive)

- Use `numpy` for numerical work, `matplotlib` for plots
- Variable names: descriptive (`sample_mean`, not `sm`)
- Comments: explain *why*, not *what* — except for parameter descriptions
- Slider labels: concise but full words ("Sample size (n)", not "n")
- Plot style: clean, minimal — no gridlines unless they aid interpretation
- Color: use a consistent palette; avoid red/green for accessibility

---

## File Naming

- Chapters: `NN-slug.qmd` (e.g., `01-ols.qmd`, `02-asymptotics.qmd`)
- Problem sets: `pset-NN.qmd` (e.g., `pset-01.qmd`)
- Dev docs: `UPPERCASE.md` in `dev_docs/`
- Style files: `custom.scss`, `custom-dark.scss` in `styles/`

---

## Stata Code Blocks (P1)

Non-executed, syntax-highlighted:

```markdown
```stata
reg y x1 x2, robust
est store model1
```

*Note: Stata cannot run in the browser. Copy this code into your Stata session.*
```

---

## Citation Format

Every bibliographic entry with a public version:

```
Author, First. (Year). *Title*. Publisher/Journal.
[[PDF](url)] [[Working Paper](url)] [[DOI](url)]
```

If working paper differs materially from published version, add a parenthetical note after the link.

---

## Local Knowledge-Node Conventions (Writing Support)

When authoring chapters with external references, maintain a local source-grounded note system in markdown.

Recommended structure:

- `knowledge/sources/` — source cards (book/paper/site level)
- `knowledge/concepts/` — concept nodes (parallel trends, exclusion, overlap, etc.)
- `knowledge/derivations/` — reusable proof/derivation fragments
- `knowledge/examples/` — empirical case notes and teaching examples

### Source card minimum schema

Each source card should include frontmatter:

```yaml
---
source_id: "hansen2022-econometrics"
title: "Econometrics"
author: "Bruce E. Hansen"
year: 2022
type: book
access_type: "open|paid|restricted"
url: "https://..."
citation_key: "hansen2022"
license_note: "official website / publisher / personal copy"
last_checked: "YYYY-MM-DD"
---
```

Body minimum:

1. One-paragraph source summary
2. Key claims used in Lemma drafts
3. Verbatim quotes (if any) with page/chapter location
4. Link-out to dependent concept nodes and chapter targets

### Legality and access policy (mandatory)

- Use only official/open-authorized public resources for downloadable text.
- Do not ingest or cite pirated/unauthorized mirrors.
- For paid books without open full text, keep metadata + legally obtained reading notes; store short quotations with precise page references only.
- Mark uncertain rights status as `access_type: restricted` until verified.

---

## Mandatory Expert Review Loop

For every non-trivial writing update (new chapter draft, major section rewrite, or new problem set), the authoring agent must run a sub-agent review loop before finalizing.

Protocol:

1. **Pre-draft review (required for new chapter drafts and major rewrites).**
   - Input: chapter outline, theorem list, assumptions map, and planned cross-links.
   - Output: conceptual-risk checks and structure advice before full prose drafting.

2. **Post-draft review (required for all non-trivial writing updates).**
   - Input: full updated text plus relevant dev docs.
   - Output: correctness, notation clarity, pedagogy/flow, and UX friction findings.

3. The reviewer must read:
   - `dev_docs/AGENTS.md`
   - `dev_docs/CONVENTIONS.md`
   - `dev_docs/CURRICULUM-ARCHITECTURE.md`
   - all sections edited in the current pass
4. Reviewer role: **economist + learning-design advisor**.
5. Reviewer outputs must include:
   - conceptual correctness issues
   - notation/assumption clarity issues
   - pedagogy and flow issues
   - website UX friction issues (where to add/remove links, callouts, examples)
6. The main agent must apply or explicitly reject each major suggestion with rationale in commit/session notes.

Minimum standard: no chapter is considered "done" without a completed review loop (pre-draft + post-draft for major writing; post-draft for minor non-trivial updates).
