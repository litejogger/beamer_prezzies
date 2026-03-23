# CLAUDE.md — beamer_prezzies

This repo contains Beamer (LaTeX) presentation slides for PhD research briefings on the natural gas market model. All presentations follow a consistent house style derived from the existing `.tex` files.

## Repo Structure

```
beamer_prezzies/
├── figs/                  # All figure images (PNG) referenced by slides
├── *.tex                  # One .tex file per presentation
└── CLAUDE.md
```

Each presentation is a self-contained `.tex` file. Figures live in `figs/` and are referenced by filename only (no path prefix needed — `\graphicspath{{figs/}}` is set in the preamble).

## Existing Presentations

- `v14_dual_brief.tex` — Root cause analysis of Part 3 dual value drops; covers LP degeneracy ruling, Qstep_prod fix, and remaining NE injection season depression
- `pa_ny_pipeline_brief.tex` — Diagnostic investigation of the PA→NY pipeline 4 bcf/day flow ceiling; concludes it is economic (15× tariff jump), not a bug
- `storage_injection_brief.tex` — Explains why Midwest injects zero in Part 3; covers three-layer injection constraint mechanics and fix strategy (Fix A + Fix B)

## Standard Preamble

Every presentation uses this exact preamble structure:

```latex
\documentclass[aspectratio=169, 11pt]{beamer}
\usetheme{metropolis}

\usepackage{graphicx}
\usepackage{booktabs}
\usepackage{xcolor}
\usepackage{appendixnumberbeamer}
% add \usepackage{amsmath} when math-heavy

% Custom colors
\definecolor{alertred}{RGB}{204, 36, 29}
\definecolor{okgreen}{RGB}{40, 140, 40}
\definecolor{noteblue}{RGB}{30, 90, 160}

\setbeamercolor{alerted text}{fg=alertred}

\graphicspath{{figs/}}

\title{Short Title:\\Subtitle on Second Line}
\subtitle{Descriptive Subtitle}
\author{Caleb Sy}
\date{Month Year}
\institute{PhD Research Briefing}
```

Do not change the theme, aspect ratio, font size, custom colors, or author/institute fields.

## Document Structure

```latex
\begin{document}
\maketitle

%% ============================================================
%% PART I: NAME
%% ============================================================

\section{Section Name}

% ---- Slide N: Description ----
\begin{frame}{Slide Title}
  ...
\end{frame}

\end{document}
```

- Use `\maketitle` immediately after `\begin{document}` — no frame wrapper needed with metropolis
- Divide presentations into parts with the `%%` banner comment and `\section{}`
- Label each slide with a `% ---- Slide N: Description ----` comment immediately before `\begin{frame}`
- Part banners use `%% ============================================================` (60 `=` signs)

## Slide Patterns

### Bullet list slide
```latex
\begin{frame}{Title}
  One-sentence context.
  \begin{itemize}
    \item Normal point
    \item \textbf{Bold emphasis}
    \item \alert{Alert text} (renders in alertred)
  \end{itemize}
  \vspace{0.5em}
  \begin{block}{Block Title}
    Block content.
  \end{block}
\end{frame}
```

### Figure slide
```latex
\begin{frame}{Title}
  \begin{figure}
    \centering
    \includegraphics[width=0.88\linewidth]{filename.png}
  \end{figure}
  \vspace{-0.5em}
  {\small \textbf{Left}: description. \textbf{Right}: description. Key finding in \alert{alert}.}
\end{frame}
```

- Caption text goes below the figure as `{\small ...}`, not inside a `\caption{}`
- Standard widths: `0.88\linewidth` for full-width figures, `0.78\linewidth` for slightly inset, `0.68\linewidth` for smaller
- Use `\vspace{-0.5em}` between the figure and the caption line to tighten spacing

### Table slide
```latex
\begin{frame}{Title}
  \begin{table}
    \centering
    \begin{tabular}{lcc}
      \toprule
      \textbf{Metric} & \textbf{Before} & \textbf{After} \\
      \midrule
      Row label & value & \textcolor{okgreen}{\textbf{improved value}} \\
      \bottomrule
    \end{tabular}
  \end{table}
\end{frame}
```

- Always use `booktabs` rules: `\toprule`, `\midrule`, `\bottomrule` — never `\hline`
- Color-code result cells: `\textcolor{okgreen}{...}` for good, `\textcolor{alertred}{...}` for bad/flagged
- Use `\rowcolor{gray!15}` (requires `\usepackage{colortbl}`) to highlight a specific row

### Two-column slide
```latex
\begin{frame}{Title}
  \begin{columns}[T]
    \column{0.48\textwidth}
      \textbf{Left heading}\\[0.3em]
      Content here.

    \column{0.48\textwidth}
      \begin{block}{Right Block}
        Block content.
      \end{block}
  \end{columns}
\end{frame}
```

### Math slide
```latex
\begin{frame}{Title}
  Inline math: $x = y + z$. Display math:
  \[
    \sum_{r} \text{region\_injection}[r,t] \;==\; I_\text{total}[t]
  \]
  \begin{alertblock}{Key finding}
    Interpretation of the equation.
  \end{alertblock}
\end{frame}
```

## Block Environments

| Environment | When to use |
|---|---|
| `block` | Neutral conclusions, definitions, key takeaways |
| `alertblock` | Problems, warnings, failure modes |
| `example` | Concrete numeric examples, worked cases |

## Color Usage

- `\alert{text}` — draws attention to a critical number or finding (renders alertred)
- `\textcolor{okgreen}{text}` — positive result, passed check, resolved issue
- `\textcolor{alertred}{text}` — negative result, flag, open problem
- `\textcolor{noteblue}{text}` — informational callout, neither good nor bad
- `\textcolor{green!60!black}{text}` — for plot legend labels referencing green lines (e.g., "relaxed" model variant)

## Spacing Conventions

- `\vspace{0.5em}` — standard gap between a paragraph/equation and the next element
- `\vspace{0.3em}` — tighter gap within a logical group
- `\vspace{-0.5em}` — pull caption up after a figure
- `\vspace{0.8em}` — before a concluding block at the bottom of a slide
- `\\[0.3em]` — line break with small vertical skip inside tabular or column environments

## Presentation Content Conventions

- **Framing**: each presentation investigates one specific model question; state it as a hypothesis or question in the first substantive slide
- **Structure**: Background → Investigation (numbered parts if multiple) → Recommendations/Fix → Next Steps
- **Verdict slides**: include a checklist table or two-column pass/fail summary at the end of each investigation
- **Equation references**: always cite the source file and line number when showing a model constraint, e.g., `(\texttt{v14\_gas\_model\_dev.py}, line~572)`
- **Numbers**: use `\sim` for approximate values (`$\sim$4.048~bcf/day`), `~` for non-breaking space before units
- **Units**: always attach units to numbers (`\$2.50/mmbtu`, `4.048~bcf/day`)
- **Checkmarks**: `\item[$\checkmark$]` for confirmed/resolved items in itemize

## Compiling

```bash
pdflatex filename.tex
```

Run twice if cross-references or section numbers are used. Output PDF lands in the same directory as the `.tex` file.
