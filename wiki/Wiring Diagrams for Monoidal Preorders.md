#definition #example #theorem

Given a [[Symmetric Monoidal Preorder]] $(X, \leq, I, \otimes)$, a [[Wiring Diagram]] has wires labeled by elements of $X$ and boxes representing inequalities. Two parallel wires $x, y$ and one wire $x \otimes y$ mean the same thing; a wire labeled $I$ and "nothing" mean the same thing (the unit is the empty monoidal product). A diagram from wires $x_1, \dots, x_m$ on the left to $y_1, \dots, y_n$ on the right is **valid** if $x_1 \otimes \cdots \otimes x_m \leq y_1 \otimes \cdots \otimes y_n$.

> Sources: 7 Sketches §2.2.2, Eqs. (2.10)–(2.18), Examples 2.14, 2.19, Exercise 2.20.

## Axioms as diagram rules

| axiom | diagram |
|---|---|
| reflexivity $x \leq x$ | a bare wire is valid |
| transitivity | valid boxes may be connected in series |
| (a) monotonicity | valid boxes may be stacked in parallel |
| (b) unitality | blank space / $I$-wires can be ignored |
| (c) associativity | no distinction between building from top or bottom; $(x \otimes y) \otimes z$ and $x \otimes (y \otimes z)$ are the same three wires |
| (d) symmetry | wires may cross (a new icon) |

**Example 2.14.** In $(\mathbb{R}, \leq, 0, +)$, parallel wires add: wires $3.14$ and $-1$ equal a single wire $2.14$; a wire $0$ equals no wire. The facts $4 \leq 7$ and $2 + 5 \leq -1 + 5 + 3$ are boxes.

## Wiring diagrams as graphical proofs

A wiring diagram with interior boxes inside an exterior box is a proof: *if all interior assertions hold, so does the exterior one*. For the diagram (2.15) with interior boxes
$$t \leq v \otimes w, \qquad w \otimes u \leq x \otimes z, \qquad v \otimes x \leq y \qquad (2.16)$$
the exterior assertion is $t \otimes u \leq y \otimes z$ (2.17), proved by the chain of vertical slices
$$t \otimes u \leq v \otimes w \otimes u \leq v \otimes x \otimes z \leq y \otimes z. \qquad (2.18)$$
Formally ([[7S Exercise 2.20]]) each step uses monotonicity with a reflexivity $u \leq u$ on the untouched wire, associativity to re-bracket, and transitivity to chain; symmetry is needed only if wires cross. The [[Resource Theory|lemon meringue pie]] diagram (Example 2.19) is a proof that if you can separate eggs, make filling, make meringue, fill the crust and add meringue, then you can prepare a pie.

```tikz
\usepackage{tikz}
\begin{document}
\begin{tikzpicture}[box/.style={draw, minimum height=1.1cm, minimum width=1cm}]
\node[box] (a) at (0,0.6) {$\leq$};
\node[box] (b) at (2,-0.4) {$\leq$};
\node[box] (c) at (4,0.6) {$\leq$};
\draw (-1.2,0.6) node[left]{$t$} -- (a.west);
\draw (a.east) ++(0,0.3) -- ++(0.6,0) node[above]{$v$} |- (c.west);
\draw (a.east) ++(0,-0.3) -- ++(0.4,0) node[below]{$w$} |- ([yshift=0.3cm]b.west);
\draw (-1.2,-0.7) node[left]{$u$} -| ([yshift=-0.3cm]b.west);
\draw ([yshift=0.3cm]b.east) -- ++(0.4,0) node[above]{$x$} |- ([yshift=-0.3cm]c.west);
\draw ([yshift=-0.3cm]b.east) -- (6,-0.7) node[right]{$z$};
\draw (c.east) -- (6,0.6) node[right]{$y$};
\draw[rounded corners, dashed] (-0.9,-1.3) rectangle (5.2,1.5);
\end{tikzpicture}
\end{document}
```

Boxes in a monoidal *preorder* are anonymous (they merely assert $\leq$); in a [[Monoidal Category]] they carry the *names* of morphisms and the diagrams denote morphisms rather than proofs of inequalities (§4.4.2).
