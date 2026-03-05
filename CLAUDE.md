# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**Lemma** is an open-source econometrics learning website built with Quarto. It features narrative-first proofs (collapsed by default) and in-browser interactive Python simulations via Shinylive/Pyodide. Currently in Milestone 3 with 6 of 11 chapters complete.

**Repository:** github.com/Shujie-Xu/Lemma
**Live site:** Deployed to GitHub Pages via GitHub Actions

## Build & Development Commands

```bash
quarto preview              # Hot-reload dev server
quarto render               # Full build to _site/
```

CI/CD: GitHub Actions (`.github/workflows/publish.yml`) runs Python 3.12 + `pip install shinylive` + `quarto render` on push to `main`, then deploys `_site/` to GitHub Pages.

## Tech Stack

- **Quarto** (static site generator, `_quarto.yml` config)
- **Shinylive** extension (`_extensions/quarto-ext/shinylive/`) — Python Shiny apps running in-browser via WebAssembly
- **SCSS** theming (`styles/custom.scss`, `styles/custom-dark.scss`) — cosmo (light) / darkly (dark) base themes
- **MathJax 3** for equations
- **BibTeX** (`references.bib`) for citations

## Project Structure

```
chapters/           # 11 chapter .qmd files (NN-slug.qmd format)
problem-sets/       # Problem set .qmd files (pset-NN.qmd format)
syllabus/           # Teaching outlines and source references (not rendered)
dev_docs/           # Developer docs (not rendered): AGENTS, CONVENTIONS, PROGRESS, PRD, CURRICULUM-ARCHITECTURE
styles/             # SCSS theme files
_extensions/        # Shinylive Quarto extension
```

**Dev docs reading order for new sessions:** AGENTS.md → PROGRESS.md → CONVENTIONS.md → CURRICULUM-ARCHITECTURE.md → Lemma-PRD.md

## Chapter Conventions

### Four-Layer Structure
1. **Intuition** — Plain prose, no equations in first paragraph
2. **Formalization** — Assumptions table, theorem statements
3. **Proof** — Collapsed callout blocks
4. **Interactive Code** — Shinylive Python simulations

### Theorem Blocks
```markdown
::: {.callout-note}
## Theorem X.Y (Name)
Statement with math.
:::
```

### Proof Blocks
```markdown
::: {.callout-tip collapse="true"}
## 📐 Expand proof
**Step 1.** ...
**Step 2.** ...
$\blacksquare$
:::
```

### Assumptions Tables
| ID | Name | Statement | Role |
|---|---|---|---|
| XXX.1 | **Name** | Statement | Role in proof |

### Shinylive Code Blocks
````markdown
```{shinylive-python}
#| standalone: true
#| viewerHeight: 520
from shiny import App, ui, render, reactive
import numpy as np
import matplotlib.pyplot as plt
# ... app code ...
```
````

### Cross-Chapter Links
- 4-8 contextual links per chapter, placed at point of need (not only in "Further Reading")
- Use `@sec-topic` for section references, descriptive anchor text

## Math Notation

- Matrices: bold uppercase ($\mathbf{X}$)
- Vectors: bold lowercase ($\mathbf{x}$)
- Scalars: italic ($\beta$)
- Equation numbering: chapter-prefixed (e.g., 6.1 for Ch 06)

## Callout Colors (for inline SVG or custom elements)

- Theorems (`.callout-note`): `#2563eb` border, `#f0f5ff` bg
- Proofs (`.callout-tip`): `#6366f1` border, `#f8f7ff` bg
- Warnings (`.callout-warning`): `#d97706` border, `#fffbeb` bg
- Important (`.callout-important`): `#dc2626` border, `#fef2f2` bg

## Writing Quality Bar

- Edited pages must render successfully (`quarto render`)
- Follow conventions in `dev_docs/CONVENTIONS.md`
- Cross-chapter links added contextually
- Citations include direct `[PDF]`/`[Working Paper]` links when available
- For writing tasks: follow pre-draft design review + post-draft content review loop (see `dev_docs/AGENTS.md`)
