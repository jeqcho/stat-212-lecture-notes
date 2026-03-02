# Instructions for Converting Handwritten Lecture Notes to LaTeX

## Overview

This project converts handwritten STAT 212 lecture notes (scanned PDFs in `reference/`) into formatted LaTeX with colored theorem boxes. Each lecture gets its own `.tex` file in `lectures/`, and `main.tex` combines them all.

## Project Structure

```
main.tex          — Master document (report class). Each lecture = one \chapter.
preamble.tex      — All packages, math macros, tcolorbox environments, colors.
titlepage.tex     — Course info title page.
lectures/         — One file per lecture (lecture01.tex, lecture02.tex, ...).
reference/        — Source scanned PDFs. Never edit these.
```

## Step-by-Step Conversion Process

### 1. Read the PDF

Read the new PDF from `reference/`. Identify lecture boundaries by looking for "STAT 212" or "STAT 212 Lecture [number]" headers at the top of pages. Note the Agenda and Announcements.

### 2. Create the Lecture File

Create `lectures/lectureNN.tex` (two-digit number). The file should **not** contain `\chapter` — that goes in `main.tex`.

### 3. Add the Chapter to main.tex

Add to `main.tex`:
```latex
\chapter{Title of Lecture}
\input{lectures/lectureNN}
```

### 4. Classify Content into Environments

Map handwritten labels to LaTeX environments:

| Handwritten Label | LaTeX Environment | Color |
|---|---|---|
| "Def:" or "Def" (underlined) | `\begin{definition}{Title}{label}` | Blue |
| "Thm" or "Theorem" (underlined) | `\begin{theorem}{Title}{label}` | Red |
| "Lemma" (underlined) | `\begin{lemma}{Title}{label}` | Teal |
| "Cor:" or "Corollary" | `\begin{corollary}{Title}{label}` | Purple |
| "Remark" | `\begin{remark}{Title}{label}` | Gray |
| "Claim" | `\begin{claim}{Title}{label}` | Indigo |
| "Ex:" or "Example" | `\begin{example}{Title}{label}` | Green |
| "Pf:" or "Proof" | `\begin{proof} ... \end{proof}` | Gray left border |
| Underlined section headings | `\section{}` or `\subsection{}` | — |
| "Q." (questions) | Italic narrative text | — |
| Boxed equations in notes | `\boxed{}` in display math | — |

Each tcolorbox environment takes the form:
```latex
\begin{theorem}{Title Goes Here}{short-label}
    Content here.
\end{theorem}
```

The `{short-label}` is used for cross-references (e.g., `\ref{thm:short-label}`).

### 5. Math Notation Conventions

Use the macros defined in `preamble.tex`:

| Notation | Macro | Output |
|---|---|---|
| Expectation (blackboard bold E) | `\E` | $\mathbb{E}$ |
| Probability | `\Prob` | $\mathbb{P}$ |
| Real numbers | `\R` | $\mathbb{R}$ |
| Natural numbers | `\N` | $\mathbb{N}$ |
| Integers | `\Z` | $\mathbb{Z}$ |
| Rationals | `\Q` | $\mathbb{Q}$ |
| Index set | `\T` | $\mathbb{T}$ |
| Sigma-algebra (calligraphic F) | `\F` | $\mathcal{F}$ |
| Sub-sigma-algebra | `\G` | $\mathcal{G}$ |
| Borel sigma-algebra | `\B` | $\mathcal{B}$ |
| Calligraphic H | `\cH` | $\mathcal{H}$ |
| State space | `\cS` | $\mathcal{S}$ |
| Indicator function | `\ind` | $\mathbbm{1}$ |
| Variance | `\Var` | Var |
| Covariance | `\Cov` | Cov |
| Almost sure convergence | `\asto` | $\xrightarrow{\text{a.s.}}$ |
| L^p convergence | `\Lpto{p}` | $\xrightarrow{L^p}$ |
| Convergence in distribution | `\dto` | $\xrightarrow{d}$ |
| Convergence in probability | `\pto` | $\xrightarrow{p}$ |
| Absolute value | `\abs{x}` | $|x|$ |
| Norm | `\norm{x}` | $\|x\|$ |

Additional conventions:
- Fractions: use `\frac{}{}` in display math, `\tfrac{}{}` or `/` inline.
- Sub/superscripts on sigma-algebras: `\F_n`, `\F_t`, `\F_{-\infty}`.
- "s.t." in the notes means "such that" — write it as `s.t.` or spell out.
- Use `\colon` instead of `:` for function notation: `f \colon \R \to \R`.

### 6. Structural Conventions

- **Sections**: Use `\section{}` for major topic shifts, `\subsection{}` for sub-topics.
- **Narrative text**: The professor's motivational questions ("Q. Why care?") and explanations should be regular paragraphs, optionally in italic.
- **Agenda/Announcements**: Can be omitted or placed as a brief italic note. These are course logistics, not mathematical content.
- **"Recall:" blocks**: Brief recap paragraphs at the start of lectures. Write as italic text.
- **Multi-step proofs**: Use `\textbf{Step 1.}`, `\textbf{Step 2.}`, etc.
- **Proof sketches**: `\begin{proof}[Proof sketch]`
- **References to textbook**: Keep verbatim (e.g., "See Theorem 4.1.9 in Durrett").
- **"HW" or homework notes**: Write as "(see Homework)" or similar parenthetical.

### 7. Cross-References

- Label format: `{short-descriptive-name}` — e.g., `{condexp-general}`, `{rn-theorem}`, `{jensen}`.
- When the notes say "proved last time" or "from last lecture", add a `\cref` if the referenced result has a label.
- The tcolorbox numbering is automatic (Definition 1.1, Theorem 1.2, etc.) — don't manually number.

### 8. Faithfulness to Source

**This is critical.** The LaTeX should be a faithful transcription of the handwritten notes:

- Include all content: definitions, theorems, proofs, proof sketches, examples, remarks, motivational text.
- Preserve the logical flow and ordering of the notes.
- If the professor crossed something out, omit it (it's a correction).
- If there's a boxed equation or result in the notes, use `\boxed{}`.
- Include intuition notes (e.g., "Intuition: The RN derivative is reweighing of μ").
- Do not add content that isn't in the notes.
- Do not reorganize the material — keep it in lecture order.

### 9. Diagrams

Some lectures contain hand-drawn diagrams (e.g., BM path sketches, Venn-like diagrams for set constructions). For these:

- Simple cases: use TikZ.
- Complex cases: add a comment `% TODO: TikZ diagram of [description]` and move on.

### 10. Building the PDF

```bash
# Using pdflatex (run twice for TOC)
pdflatex main.tex && pdflatex main.tex

# Or using latexmk
latexmk -pdf main.tex
```

For Overleaf: push to the synced repo and it will compile automatically.

### 11. Quality Checklist

After converting a new lecture, verify:

- [ ] All definitions, theorems, lemmas are in colored tcolorbox environments
- [ ] Proofs use `\begin{proof}` and end with QED symbol
- [ ] Math notation uses the macros from preamble.tex
- [ ] No `\chapter` in the lecture file (only in main.tex)
- [ ] Content is faithful to the handwritten notes
- [ ] The file compiles without errors when included via main.tex
- [ ] New `\chapter` + `\input` added to main.tex
