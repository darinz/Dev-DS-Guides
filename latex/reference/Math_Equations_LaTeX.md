# LaTeX References

Google Search: "list of mathematical symbols latex"

[LaTeX/Mathematics](https://en.wikibooks.org/wiki/LaTeX/Mathematics)

List of mathematical symbols by subject **(Set theory, Arithmetic, Linear algebra, Calculus, etc.)** in the following format **(LaTeX, HTML, and Unicode)**:
https://en.wikipedia.org/wiki/List_of_mathematical_symbols_by_subject


List of LaTeX mathematical symbols:
https://oeis.org/wiki/List_of_LaTeX_mathematical_symbols

## Examples:

Composition of functions: $f \circ g$

**Horizontal Spacing**

$
\, \text{small space} \\ 
\: \text{medium space} \\
\; \text{large space} \\
\! \text{negative space} \\
\quad \text{one tab} \\
\qquad \text{double tab}
$

**\quad and \qquad:**

$
\exists p. (Person(p) \land \\
\quad \forall q. ((Person(q) \land p \neq q) \rightarrow \\
\qquad \lnot Loves(p, q) \\
\quad ) \\
)
$

**Modular Equivalence Symbol:**
$\equiv$

Alignment using \&:

$$\begin{align*}
a & = n^2 \\
& = n^2 \\
& = 1 + 3 + 5 * 4
\end{align*}$$

Examples of markdown text with tags repeated, escaped the second time, to clarify their function:

\**italics*\* and \__italics_\_

**\*\*bold\*\***

\~\~~~strikethrough~~\~\~

\``monospace`\`

No indent
>\>One level of indentation
>>\>\>Two levels of indentation

An ordered list:
1. 1\. One
1. 1\. Two
1. 1\. Three

An unordered list:
* \* One
* \* Two
* \* Three

A naked URL: https://google.com

Linked URL: \[[Colaboratory](https://research.google.com/colaboratory)]\(https://research.google.com/colaboratory)

A linked URL using references:

>\[[Colaboratory][colaboratory-label]]\[colaboratory-label]

>\[colaboratory-label]: https://research.google.com/colaboratory
[colaboratory-label]: https://research.google.com/colaboratory

An inline image: !\[Google's logo](https://www.google.com/images/logos/google_logo_41.png)
>![Google's logo](https://www.google.com/images/logos/google_logo_41.png)

Equations:

>$y=x^2$

>$e^{i\pi} + 1 = 0$

>$e^x=\sum_{i=0}^\infty \frac{1}{i!}x^i$

>$\frac{n!}{k!(n-k)!} = {n \choose k}$

>$A_{m,n} =
 \begin{pmatrix}
  a_{1,1} & a_{1,2} & \cdots & a_{1,n} \\
  a_{2,1} & a_{2,2} & \cdots & a_{2,n} \\
  \vdots  & \vdots  & \ddots & \vdots  \\
  a_{m,1} & a_{m,2} & \cdots & a_{m,n}
 \end{pmatrix}$

Tables:
>```
First column name | Second column name
--- | ---
Row 1, Col 1 | Row 1, Col 2
Row 2, Col 1 | Row 2, Col 2
```

becomes:

>First column name | Second column name
>--- | ---
>Row 1, Col 1 | Row 1, Col 2
>Row 2, Col 1 | Row 2, Col 2

Horizontal rule done with three dashes (\-\-\-):

---

# LaTeX: Set Notation

the Empty set: $\emptyset$

Set: $\{a, b, ... \}$

Set of elements ${\displaystyle a}$, that satisfy the condition ${\displaystyle T(a)}$: $\{ a \mid T(a) \}$ 

T(a): $\{a \colon T(a) \}$

$A \cup B$

$A \cap B$

$A \setminus B$

$A \triangle B$

$\wp(A)$

$\subset$

$\subsetneq$

$\subseteq$

$\supset$

$\in$

$\notin$

set of natural numbers: $\mathbb{N}$

set of integers: $\mathbb{Z}$

$\mathbb{R}$

Cardinality: $\vert A \vert$ 

Infinite cardinals (Aleph number): $\aleph_0, \aleph_1, ...$ 

# LaTeX: Math Equations

https://jupyter-notebook.readthedocs.io/en/stable/examples/Notebook/Typesetting%20Equations.html

The Markdown parser included in the Jupyter Notebook is MathJax-aware. This means that you can freely mix in mathematical expressions using the MathJax subset of Tex and LaTeX. Some examples from the MathJax site are reproduced below, as well as the Markdown+TeX source.

## The following include examples on how to input math equations in Jupyter Notebook

### The Lorenz Equations

\begin{align}
\dot{x} & = \sigma(y-x) \\
\dot{y} & = \rho x - y - xz \\
\dot{z} & = -\beta z + xy
\end{align}

### The Cauchy-Schwarz Inequality

\begin{equation*}
\left( \sum_{k=1}^n a_k b_k \right)^2 \leq \left( \sum_{k=1}^n a_k^2 \right) \left( \sum_{k=1}^n b_k^2 \right)
\end{equation*}

### A Cross Product Formula

\begin{equation*}
\mathbf{V}_1 \times \mathbf{V}_2 =  \begin{vmatrix}
\mathbf{i} & \mathbf{j} & \mathbf{k} \\
\frac{\partial X}{\partial u} &  \frac{\partial Y}{\partial u} & 0 \\
\frac{\partial X}{\partial v} &  \frac{\partial Y}{\partial v} & 0
\end{vmatrix}
\end{equation*}

### The probability of getting (k) heads when flipping (n) coins is

\begin{equation*}
P(E)   = {n \choose k} p^k (1-p)^{ n-k}
\end{equation*}

### An Identity of Ramanujan

\begin{equation*}
\frac{1}{\Bigl(\sqrt{\phi \sqrt{5}}-\phi\Bigr) e^{\frac25 \pi}} =
1+\frac{e^{-2\pi}} {1+\frac{e^{-4\pi}} {1+\frac{e^{-6\pi}}
{1+\frac{e^{-8\pi}} {1+\ldots} } } }
\end{equation*}

### A Rogers-Ramanujan Identity

\begin{equation*}
1 +  \frac{q^2}{(1-q)}+\frac{q^6}{(1-q)(1-q^2)}+\cdots =
\prod_{j=0}^{\infty}\frac{1}{(1-q^{5j+2})(1-q^{5j+3})},
\quad\quad \text{for $|q|<1$}.
\end{equation*}

### Maxwell's Equations

\begin{align}
\nabla \times \vec{\mathbf{B}} -\, \frac1c\, \frac{\partial\vec{\mathbf{E}}}{\partial t} & = \frac{4\pi}{c}\vec{\mathbf{j}} \\   \nabla \cdot \vec{\mathbf{E}} & = 4 \pi \rho \\
\nabla \times \vec{\mathbf{E}}\, +\, \frac1c\, \frac{\partial\vec{\mathbf{B}}}{\partial t} & = \vec{\mathbf{0}} \\
\nabla \cdot \vec{\mathbf{B}} & = 0
\end{align}

### Equation Numbering and References

Equation numbering and referencing will be available in a future version of the Jupyter notebook.

### Inline Typesetting (Mixing Markdown and TeX)

While display equations look good for a page of samples, the ability to mix math and formatted text in a paragraph is also important.

This expression $\sqrt{3x-1}+(1+x)^2$ is an example of a TeX inline equation in a [Markdown-formatted](https://daringfireball.net/projects/markdown/) sentence.

### Other Syntax

You will notice in other places on the web that $$ are needed explicitly to begin and end MathJax typesetting. This is not required if you will be using TeX environments, but the Jupyter notebook will accept this syntax on legacy notebooks.

$$
\begin{array}{c}
y_1 \\\\
y_2 \mathtt{t}_i \\\\
z_{3,4}
\end{array}
$$

$$
\begin{array}{c}
y_1 \cr
y_2 \mathtt{t}_i \cr
y_{3}
\end{array}
$$

$$\begin{eqnarray}
x' &=& &x \sin\phi &+& z \cos\phi \\
z' &=& - &x \cos\phi &+& z \sin\phi \\
\end{eqnarray}$$

$$
x=4
$$ 