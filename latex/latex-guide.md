# LaTeX Complete Reference Guide

A comprehensive guide to LaTeX document preparation system, covering everything from basic document structure to advanced mathematical typesetting and formatting.

## Table of Contents

1. [Introduction](#introduction)
2. [Document Structure](#document-structure)
3. [Text Formatting](#text-formatting)
4. [Mathematical Typesetting](#mathematical-typesetting)
5. [GitHub Mathematical Expressions](#github-mathematical-expressions)
6. [Lists and Environments](#lists-and-environments)
7. [Tables](#tables)
8. [Figures and Graphics](#figures-and-graphics)
9. [Cross-References](#cross-references)
10. [Bibliography](#bibliography)
11. [Advanced Features](#advanced-features)
12. [Common Packages](#common-packages)
13. [Troubleshooting](#troubleshooting)

## Introduction

LaTeX is a document preparation system widely used for technical and scientific documents. It excels at typesetting mathematical equations, creating structured documents, and maintaining consistent formatting.

### Key Advantages

- **Professional Typesetting**: Automatic formatting and spacing
- **Mathematical Excellence**: Superior math equation rendering
- **Cross-References**: Automatic numbering and referencing
- **Bibliography Management**: Automated citation handling
- **Platform Independence**: Consistent output across systems

### Basic Document Structure

```latex
\documentclass[options]{class}
\usepackage[options]{package}

\begin{document}
% Document content here
\end{document}
```

## Document Structure

### Document Classes

```latex
\documentclass{article}        % Articles, papers
\documentclass{report}         % Longer reports, theses
\documentclass{book}           % Books
\documentclass{letter}         % Letters
\documentclass{beamer}         % Presentations
\documentclass{memoir}         % Books with memoir features
```

### Common Options

```latex
\documentclass[12pt,a4paper,twoside]{article}
% 12pt: font size
% a4paper: paper size
% twoside: two-sided printing
% oneside: single-sided printing
% landscape: landscape orientation
% draft: draft mode (faster compilation)
```

### Document Parts

```latex
\documentclass{article}
\usepackage[utf8]{inputenc}
\usepackage{geometry}

\title{Document Title}
\author{Author Name}
\date{\today}

\begin{document}

\maketitle                    % Generate title page
\tableofcontents              % Table of contents
\listoffigures                % List of figures
\listoftables                 % List of tables

\section{Introduction}
\subsection{Background}
\subsubsection{Details}

\section{Main Content}
\section{Conclusion}

\appendix                     % Start appendix
\section{Appendix A}

\end{document}
```

## Text Formatting

### Font Styles

```latex
\textbf{bold text}            % Bold
\textit{italic text}          % Italic
\texttt{typewriter text}      % Monospace
\textsc{small caps}           % Small capitals
\underline{underlined text}   % Underlined
\textsuperscript{superscript} % Superscript
\textsubscript{subscript}     % Subscript
```

### Font Sizes

```latex
\tiny                         % Tiny
\scriptsize                   % Script size
\footnotesize                 % Footnote size
\small                        % Small
\normalsize                   % Normal (default)
\large                        % Large
\Large                        % Larger
\LARGE                        % Very large
\huge                         % Huge
\Huge                         % Very huge
```

### Spacing and Alignment

```latex
% Horizontal spacing
\quad                         % 1em space
\qquad                        % 2em space
\hspace{1cm}                  % Custom space
\hfill                        % Fill available space

% Vertical spacing
\vspace{1cm}                  % Vertical space
\vfill                        % Fill vertical space

% Alignment
\begin{center}
Centered text
\end{center}

\begin{flushleft}
Left-aligned text
\end{flushleft}

\begin{flushright}
Right-aligned text
\end{flushright}
```

### Special Characters

```latex
\%                            % Percent sign
\$                            % Dollar sign
\{ \}                         % Braces
\#                            % Hash
\&                            % Ampersand
\_                            % Underscore
\~                            % Tilde
\^                            % Caret
\textbackslash                % Backslash
```

## Mathematical Typesetting

### Inline Math

```latex
The quadratic formula is $x = \frac{-b \pm \sqrt{b^2-4ac}}{2a}$.
```

### Display Math

```latex
\begin{equation}
E = mc^2
\end{equation}

\begin{align}
y &= mx + b \\
  &= 2x + 3
\end{align}

\begin{gather}
a + b = c \\
d + e = f
\end{gather}
```

### Mathematical Symbols

```latex
% Greek letters
\alpha, \beta, \gamma, \delta, \epsilon, \zeta, \eta, \theta
\iota, \kappa, \lambda, \mu, \nu, \xi, \pi, \rho, \sigma
\tau, \upsilon, \phi, \chi, \psi, \omega

% Capital Greek letters
\Gamma, \Delta, \Theta, \Lambda, \Xi, \Pi, \Sigma, \Phi, \Psi, \Omega

% Operators
\pm, \mp, \times, \div, \cdot, \ast, \star, \dagger, \ddagger
\leq, \geq, \neq, \approx, \equiv, \propto, \infty

% Arrows
\leftarrow, \rightarrow, \Leftarrow, \Rightarrow
\leftrightarrow, \Leftrightarrow, \mapsto, \to

% Set theory
\in, \notin, \subset, \supset, \subseteq, \supseteq
\cup, \cap, \emptyset, \mathbb{R}, \mathbb{Z}, \mathbb{N}
```

### Fractions and Roots

```latex
\frac{numerator}{denominator} % Fraction
\sqrt{expression}             % Square root
\sqrt[n]{expression}          % nth root
```

### Subscripts and Superscripts

```latex
x^2                          % Superscript
x_i                          % Subscript
x_i^2                        % Both
x^{2n}                       % Multiple characters
x_{i,j}                      % Multiple subscripts
```

### Matrices

```latex
\begin{matrix}
a & b \\
c & d
\end{matrix}

\begin{pmatrix}
a & b \\
c & d
\end{pmatrix}

\begin{bmatrix}
a & b \\
c & d
\end{bmatrix}

\begin{vmatrix}
a & b \\
c & d
\end{vmatrix}

\begin{Vmatrix}
a & b \\
c & d
\end{Vmatrix}
```

### Cases and Piecewise Functions

```latex
f(x) = \begin{cases}
x^2 & \text{if } x > 0 \\
0 & \text{if } x = 0 \\
-x^2 & \text{if } x < 0
\end{cases}
```

## GitHub Mathematical Expressions

GitHub supports LaTeX-formatted mathematical expressions in Markdown files, issues, discussions, pull requests, and wikis. This allows you to display mathematical equations directly in your GitHub content using MathJax rendering.

### Inline Mathematical Expressions

There are two syntax options for inline mathematical expressions:

#### Option 1: Dollar Symbol Delimiters
```markdown
This sentence uses `$` delimiters to show math inline: $\sqrt{3x-1}+(1+x)^2$
```

#### Option 2: Backtick Syntax (Recommended)
```markdown
This sentence uses $` and `$ delimiters to show math inline: $`\sqrt{3x-1}+(1+x)^2`$
```

**Best Practice**: Use the backtick syntax (```$`...`$```) when your mathematical expression contains characters that overlap with Markdown syntax. This prevents parsing conflicts and ensures proper rendering.

### Block Mathematical Expressions

For standalone mathematical expressions, use double dollar symbols:

```markdown
**The Cauchy-Schwarz Inequality**

$$\left( \sum_{k=1}^n a_k b_k \right)^2 \leq \left( \sum_{k=1}^n a_k^2 \right) \left( \sum_{k=1}^n b_k^2 \right)$$
```

#### Alternative: Math Code Block Syntax

You can also use the ```` ```math ```` code block syntax, which doesn't require `$$` delimiters:

```markdown
**The Cauchy-Schwarz Inequality**

```math
\left( \sum_{k=1}^n a_k b_k \right)^2 \leq \left( \sum_{k=1}^n a_k^2 \right) \left( \sum_{k=1}^n b_k^2 \right)
```
```

### Handling Dollar Signs in Mathematical Expressions

When you need to display dollar signs within mathematical expressions:

#### Within Math Expressions
Escape the dollar sign with a backslash:
```markdown
This expression uses `\$` to display a dollar sign: $`\sqrt{\$4}`$
```

#### Outside Math Expressions
Use span tags around the dollar sign:
```markdown
To split <span>$</span>100 in half, we calculate $100/2$
```

### Supported LaTeX Features

GitHub's math rendering supports:
- **MathJax**: Open-source JavaScript display engine
- **LaTeX Macros**: Wide range of LaTeX mathematical commands
- **Accessibility**: Built-in accessibility extensions
- **Cross-Platform**: Consistent rendering across different devices

### Where Mathematical Expressions Work

Mathematical expressions are supported in:
- GitHub Issues
- GitHub Discussions
- Pull Requests
- Wikis
- Markdown files (`.md`)

### Example Usage

```markdown
# Mathematical Documentation

## Inline Examples
The quadratic formula is $`x = \frac{-b \pm \sqrt{b^2-4ac}}{2a}`$.

## Block Examples
The Pythagorean theorem states:

$$a^2 + b^2 = c^2$$

## Complex Equations
The Euler-Lagrange equation:

```math
\frac{d}{dt}\left(\frac{\partial L}{\partial \dot{q}}\right) - \frac{\partial L}{\partial q} = 0
```
```

For more information, see [GitHub's Mathematical Expressions Documentation](https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/writing-mathematical-expressions).

## Lists and Environments

### Itemized Lists

```latex
\begin{itemize}
\item First item
\item Second item
  \begin{itemize}
  \item Sub-item
  \item Another sub-item
  \end{itemize}
\item Third item
\end{itemize}
```

### Enumerated Lists

```latex
\begin{enumerate}
\item First item
\item Second item
  \begin{enumerate}
  \item Sub-item
  \item Another sub-item
  \end{enumerate}
\item Third item
\end{enumerate}
```

### Description Lists

```latex
\begin{description}
\item[Term 1] Definition of term 1
\item[Term 2] Definition of term 2
\item[Term 3] Definition of term 3
\end{description}
```

### Custom Lists

```latex
\begin{enumerate}[label=(\alph*)]
\item Item with letter label
\item Another item
\end{enumerate}
```

## Tables

### Basic Table

```latex
\begin{table}[h]
\centering
\begin{tabular}{|l|c|r|}
\hline
Left & Center & Right \\
\hline
A & B & C \\
D & E & F \\
\hline
\end{tabular}
\caption{A simple table}
\label{tab:simple}
\end{table}
```

### Table Formatting

```latex
\begin{tabular}{lcrp{3cm}}
% l: left-aligned
% c: center-aligned
% r: right-aligned
% p{width}: paragraph with fixed width
```

### Advanced Tables with booktabs

```latex
\usepackage{booktabs}

\begin{table}[h]
\centering
\begin{tabular}{lcc}
\toprule
Header 1 & Header 2 & Header 3 \\
\midrule
Data 1 & Data 2 & Data 3 \\
Data 4 & Data 5 & Data 6 \\
\bottomrule
\end{tabular}
\caption{Professional table}
\label{tab:professional}
\end{table}
```

### Multi-row and Multi-column

```latex
\usepackage{multirow}

\begin{tabular}{|c|c|c|}
\hline
\multirow{2}{*}{Multi-row} & Column 2 & Column 3 \\
\cline{2-3}
& Data & Data \\
\hline
\end{tabular}
```

## Figures and Graphics

### Including Images

```latex
\usepackage{graphicx}

\begin{figure}[h]
\centering
\includegraphics[width=0.5\textwidth]{image.png}
\caption{Description of the image}
\label{fig:example}
\end{figure}
```

### Image Options

```latex
\includegraphics[width=5cm]{image.png}
\includegraphics[height=3cm]{image.png}
\includegraphics[scale=0.5]{image.png}
\includegraphics[angle=45]{image.png}
```

### Subfigures

```latex
\usepackage{subcaption}

\begin{figure}[h]
\centering
\begin{subfigure}[b]{0.45\textwidth}
\includegraphics[width=\textwidth]{image1.png}
\caption{First subfigure}
\end{subfigure}
\begin{subfigure}[b]{0.45\textwidth}
\includegraphics[width=\textwidth]{image2.png}
\caption{Second subfigure}
\end{subfigure}
\caption{Main figure caption}
\label{fig:subfigures}
\end{figure}
```

### TikZ Graphics

```latex
\usepackage{tikz}

\begin{tikzpicture}
\draw (0,0) circle (1cm);
\draw (0,0) -- (1,1);
\node at (0,0) {Origin};
\end{tikzpicture}
```

## Cross-References

### Labels and References

```latex
\section{Introduction}\label{sec:intro}
\subsection{Background}\label{subsec:background}

As discussed in Section~\ref{sec:intro}, we can see that...

See Figure~\ref{fig:example} for details.

The results are shown in Table~\ref{tab:results}.
```

### Page References

```latex
\pageref{label}               % Page number
\autoref{label}               % Automatic reference type
```

### Bibliography References

```latex
\cite{key}                    % Citation
\citep{key}                   % Parenthetical citation
\citet{key}                   % Text citation
\cite[see][p.~45]{key}        % Citation with page
```

## Bibliography

### BibTeX

```latex
\bibliographystyle{plain}
\bibliography{references}
```

### Bibliography Entry Example

```bibtex
@article{key,
  author = {Author Name},
  title = {Article Title},
  journal = {Journal Name},
  year = {2023},
  volume = {1},
  number = {1},
  pages = {1--10}
}

@book{bookkey,
  author = {Author Name},
  title = {Book Title},
  publisher = {Publisher},
  year = {2023},
  address = {City}
}
```

### natbib Package

```latex
\usepackage[numbers,sort&compress]{natbib}

\citep{key}                   % [1]
\citet{key}                   % Author (2023)
\citeauthor{key}              % Author
\citeyear{key}                % 2023
```

## Advanced Features

### Custom Commands

```latex
\newcommand{\R}{\mathbb{R}}
\newcommand{\abs}[1]{\left|#1\right|}
\newcommand{\norm}[1]{\left\|#1\right\|}

% Usage
$\R$                          % Real numbers
$\abs{x}$                     % Absolute value
$\norm{v}$                    % Norm
```

### Custom Environments

```latex
\newenvironment{theorem}
{\begin{trivlist}\item[\hskip \labelsep {\bfseries Theorem.}]}
{\end{trivlist}}

\begin{theorem}
This is a theorem.
\end{theorem}
```

### Conditional Compilation

```latex
\usepackage{comment}

\begin{comment}
This text will not appear in the final document.
\end{comment}

\includeonly{chapter1,chapter3}  % Only include specific files
```

### Footnotes and Margin Notes

```latex
This is text with a footnote\footnote{Footnote text}.

This is text with a margin note\marginpar{Margin note text}.
```

## Common Packages

### Essential Packages

```latex
\usepackage[utf8]{inputenc}   % UTF-8 encoding
\usepackage[T1]{fontenc}      % Font encoding
\usepackage{geometry}         % Page geometry
\usepackage{amsmath}          % Advanced math
\usepackage{amssymb}          % Math symbols
\usepackage{graphicx}         % Graphics
\usepackage{hyperref}         % Hyperlinks
\usepackage{url}              % URL formatting
\usepackage{booktabs}         % Professional tables
\usepackage{multirow}         % Multi-row tables
\usepackage{subcaption}       % Subfigures
\usepackage{listings}         % Code listings
\usepackage{color}            % Color support
\usepackage{xcolor}           % Extended color support
```

### Specialized Packages

```latex
\usepackage{algorithm}        % Algorithms
\usepackage{algorithmic}      % Algorithm pseudocode
\usepackage{listings}         % Code listings
\usepackage{minted}           % Syntax highlighting
\usepackage{tikz}             % Graphics
\usepackage{pgfplots}         % Plots
\usepackage{siunitx}          % Units
\usepackage{chemfig}          % Chemical formulas
\usepackage{circuitikz}       % Circuit diagrams
```

## Troubleshooting

### Common Errors

1. **Missing $**: Ensure math mode is properly enclosed
2. **Undefined control sequence**: Check package imports and command spelling
3. **Missing \begin{document}**: Ensure document structure is complete
4. **File not found**: Check file paths and extensions

### Debugging Tips

```latex
\usepackage{showframe}        % Show page margins
\usepackage{trace}            % Enable tracing
\errorcontextlines=5          % Show more error context
```

### Performance Optimization

```latex
\usepackage{syntonly}         % Syntax check only
\usepackage{draft}            % Draft mode
```

### Best Practices

1. **Use meaningful labels**: `\label{sec:introduction}` not `\label{sec1}`
2. **Organize packages**: Group related packages together
3. **Comment your code**: Add comments for complex structures
4. **Use consistent formatting**: Maintain consistent style throughout
5. **Backup your work**: Use version control for important documents

### Compilation Workflow

```bash
# Basic compilation
pdflatex document.tex

# With bibliography
pdflatex document.tex
bibtex document
pdflatex document.tex
pdflatex document.tex

# With index
pdflatex document.tex
makeindex document.idx
pdflatex document.tex
```

This comprehensive guide covers the essential aspects of LaTeX document preparation. For more advanced topics, refer to the LaTeX documentation and community resources.
