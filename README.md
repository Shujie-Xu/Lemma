# Lemma

An open-source econometrics learning site built with [Quarto](https://quarto.org/).

## What is Lemma?

In mathematics, a *lemma* is a proven intermediate result established before — and in service of — a larger theorem. Every concept here is a building block.

Lemma targets the gap between formal textbooks (dense, proof-heavy) and applied guides (light on rigor). Its two defining features:

- **Narrative-first proofs** — Detailed derivations collapsed by default; expand on demand
- **In-browser interactive code** — Python simulations via WebAssembly, no installation required

## Chapters

| # | Title | Status |
|---|---|---|
| 01 | OLS Foundations | Planned |
| 02 | Large-Sample Theory | Planned |
| 03 | Instrumental Variables | Planned |
| 04 | Difference-in-Differences | Planned |
| 05 | Regression Discontinuity | Planned |

## Local Development

### Prerequisites

- [Quarto](https://quarto.org/docs/get-started/) ≥ 1.5

### Setup

```bash
# Install the Shinylive extension (one-time)
quarto add quarto-ext/shinylive

# Preview with hot-reload
quarto preview
```

### Build

```bash
quarto render
```

### Deploy

```bash
quarto publish gh-pages
```

## Project Structure

```
lemma/
├── _quarto.yml              # Site configuration
├── index.qmd                # Homepage
├── references.qmd           # Annotated bibliography
├── references.bib           # BibTeX entries
├── chapters/                # Chapter content
│   ├── 01-ols.qmd
│   ├── 02-asymptotics.qmd
│   ├── 03-iv.qmd
│   ├── 04-did.qmd
│   └── 05-rdd.qmd
├── problem-sets/            # Practice problems
├── styles/                  # SCSS theme overrides
│   ├── custom.scss
│   └── custom-dark.scss
├── dev_docs/                # Development documentation
└── .github/workflows/       # CI/CD
```

## License

- **Content**: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
- **Code**: [MIT](https://opensource.org/licenses/MIT)
