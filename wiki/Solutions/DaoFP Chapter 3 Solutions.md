#solution

Solutions to the exercises of DaoFP, Chapter 3: [[DaoFP Chapter 3 Exercises]]. Index: [[Map of Content]].

## Solution 3.1.1

[[DaoFP Chapter 3 Exercises#Exercise 3.1.1|Exercise 3.1.1]]

For $f : a \cong b$, pre-composition $(- \circ f) : \mathcal{C}(b, x) \to \mathcal{C}(a, x)$ has inverse $(- \circ f^{-1})$, since $(h \circ f) \circ f^{-1} = h$ and $(k \circ f^{-1}) \circ f = k$. See [[Isomorphism]].

> Sources: DaoFP Exercise 3.1.1.

## Solution 3.1.2

[[DaoFP Chapter 3 Exercises#Exercise 3.1.2|Exercise 3.1.2]]

$\mathrm{id}_a$ is its own inverse: $\mathrm{id}_a \circ \mathrm{id}_a = \mathrm{id}_a$.

> Sources: DaoFP Exercise 3.1.2.

## Solution 3.1.3

#proof — [[DaoFP Chapter 3 Exercises#Exercise 3.1.3|Exercise 3.1.3]]

Let $1, 1'$ be terminal with unique $a : 1 \to 1'$, $b : 1' \to 1$. Then $b \circ a : 1 \to 1$ is an arrow into the terminal $1$, hence equals $\mathrm{id}_1$; likewise $a \circ b = \mathrm{id}_{1'}$. See [[Terminal Object]].

> Sources: DaoFP Exercise 3.1.3.

## Solution 3.1.4

#proof — [[DaoFP Chapter 3 Exercises#Exercise 3.1.4|Exercise 3.1.4]]

Any isomorphism $1 \to 1'$ is an arrow into a terminal object, of which there is exactly one. "Unique up to unique isomorphism."

> Sources: DaoFP Exercise 3.1.4.

## Solution 3.2.1

[[DaoFP Chapter 3 Exercises#Exercise 3.2.1|Exercise 3.2.1]]

Left: $(g \circ h) \circ f^{-1}$; right: $g \circ (h \circ f^{-1})$; equal by associativity — the naturality is automatic here. See [[Isomorphism]], [[Natural Transformation]].

> Sources: DaoFP Exercise 3.2.1.

## Solution 3.3.1

#proof — [[DaoFP Chapter 3 Exercises#Exercise 3.3.1|Exercise 3.3.1]]

Set $f^{-1} := \beta_a(\mathrm{id}_a) : b \to a$. Naturality with $x = a$, $h = \mathrm{id}_a$, $g : a \to y$ gives $\beta_y(g) = \beta_y(g \circ \mathrm{id}_a) = g \circ \beta_a(\mathrm{id}_a) = g \circ f^{-1}$, so $\beta_y = (- \circ f^{-1})$ ([[DaoFP Chapter 3 Exercises#Exercise 3.3.2|DaoFP Exercise 3.3.2]]); with $f := \beta_b^{-1}(\mathrm{id}_b)$ one checks the two are inverse. See [[Isomorphism]], [[Yoneda Lemma]].

> Sources: DaoFP Exercise 3.3.1.

## Solution 3.3.2

[[DaoFP Chapter 3 Exercises#Exercise 3.3.2|Exercise 3.3.2]]

$\beta_y(g) = g \circ f^{-1}$: the whole family is determined by its value on $\mathrm{id}_a$.

> Sources: DaoFP Exercise 3.3.2.
