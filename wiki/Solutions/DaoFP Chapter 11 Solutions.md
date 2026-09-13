#solution

Solutions to the exercises of DaoFP, Chapter 11: [[DaoFP Chapter 11 Exercises]]. Index: [[Map of Content]].

## Solution 11.1.1

#program — [[DaoFP Chapter 11 Exercises#Exercise 11.1.1|Exercise 11.1.1]]

```haskell
tailV :: Vec ('S n) a -> Vec n a
tailV (VCons _ as) = as
-- tailV emptyV   -- type error: couldn't match 'Z with 'S n
```

The compiler rejects `tailV emptyV` because `Vec 'Z Int` does not unify with `Vec ('S n) a`; the pattern match is exhaustive since `VNil` cannot have type `Vec ('S n) a`.

> Sources: DaoFP Exercise 11.1.1.

## Solution 11.2.1

#proof — [[DaoFP Chapter 11 Exercises#Exercise 11.2.1|Exercise 11.2.1]]

A cone over $b \to 1 \leftarrow e$ is a pair of arrows $q_1 : x \to b$, $q_2 : x \to e$ with $! \circ q_1 = ! \circ q_2$ — a condition that is automatic since there is only one arrow $x \to 1$. So a cone is just a pair of arrows, and the pullback's universal property (unique $h : x \to b \times_1 e$ with $\pi_1 h = q_1$, $\pi_2 h = q_2$) is exactly the universal property of $b \times e$. Fibrationally: pulling $e$ back along $! : b \to 1$ plants a copy of $e$ over every point of $b$ — the trivial bundle ([[Base Change Functor]]).

> Sources: DaoFP Exercise 11.2.1.

## Solution 11.2.2

#proof — [[DaoFP Chapter 11 Exercises#Exercise 11.2.2|Exercise 11.2.2]]

A diagram of that shape is a [[Cospan]] $f : A \to B \leftarrow C : g$. A [[Cone]] with apex $x$ consists of $q_a : x \to A$, $q_b : x \to B$, $q_c : x \to C$ with $f \circ q_a = q_b = g \circ q_c$; so $q_b$ is redundant and a cone is a pair $(q_a, q_c)$ with $f q_a = g q_c$ — a commuting square. The limit (terminal cone) is then an object $e'$ with $p' : e' \to A$, $h : e' \to C$ such that every such square factors uniquely through it: precisely the pullback.

> Sources: DaoFP Exercise 11.2.2.

## Solution 11.2.3

#proof — [[DaoFP Chapter 11 Exercises#Exercise 11.2.3|Exercise 11.2.3]]

Let $\langle e, p \rangle$, $\langle e', p' \rangle$ be objects of $\mathcal{C}/b$ and $e \times_b e'$ their pullback with legs $\pi, \pi'$. It is an object of $\mathcal{C}/b$ via $p \circ \pi = p' \circ \pi'$, and $\pi, \pi'$ are slice morphisms (they commute with the projections by construction). Given a slice object $\langle x, q \rangle$ with slice morphisms $u : x \to e$, $u' : x \to e'$ — i.e. $p u = q = p' u'$ — the square commutes, so the pullback gives a unique $h : x \to e \times_b e'$ with $\pi h = u$, $\pi' h = u'$; $h$ is a slice morphism since $(p \pi) h = p u = q$. This is the universal property of the product in $\mathcal{C}/b$. (Kittenlab's "typed products" $A \times_T A'$ in $\mathbf{FinSet}/T$ are exactly this.)

> Sources: DaoFP Exercise 11.2.3; Kittenlab Lecture 13.

## Solution 11.2.4

#proof — [[DaoFP Chapter 11 Exercises#Exercise 11.2.4|Exercise 11.2.4]]

Let $g' : f^* e' \to e'$ and $g : f^* e \to e$ be the pullback legs. The composite $g' \mathbin{;} h : f^* e' \to e$ and the projection $f^* p' : f^* e' \to b$ satisfy $p \circ (h \circ g') = p' \circ g' = f \circ f^* p'$, so they form a commuting square over $b \xrightarrow{f} a \xleftarrow{p} e$. By the universal property of the pullback $f^* e$ there is a unique $f^* h : f^* e' \to f^* e$ with $g \circ f^* h = h \circ g'$ and $f^* p \circ f^* h = f^* p'$ — the latter saying $f^* h$ is a morphism in $\mathcal{C}/b$. Uniqueness gives functoriality ($f^*(h \circ k) = f^* h \circ f^* k$, $f^* \mathrm{id} = \mathrm{id}$).

> Sources: DaoFP Exercise 11.2.4.

## Solution 11.4.1

#example — [[DaoFP Chapter 11 Exercises#Exercise 11.4.1|Exercise 11.4.1]]

$f^{-1}(1) = B$ and $f^{-1}(0) = \varnothing$. The fiber of $\Pi_f E$ over $1$ is the set of sections of $E$ over all of $B$; the fiber over $0$ is the set of sections over the empty patch — a singleton (the empty section). Correspondingly $f^* G$ contains only the fiber of $G$ over $1$, replanted over every point of $B$, so a map $f^* G \to E$ is a family of sections indexed by $G_1$; the right-hand side $\phi^T : G \to \Pi_f E$ sends $G_1$ to those sections and sends the fiber $G_0$ to the unique point of $(\Pi_f E)_0$ — there is no other choice.

> Sources: DaoFP Exercise 11.4.1.

## Solution 11.4.2

#proof — [[DaoFP Chapter 11 Exercises#Exercise 11.4.2|Exercise 11.4.2]]

(i) $f^* 1 = \{(b, *) \mid f(b) = x\} \cong f^{-1}(x) \subseteq B$, one point over each $b \in f^{-1}(x)$, nothing elsewhere. (ii) A fiberwise map $f^* 1 \to E$ picks, for each $b \in f^{-1}(x)$, an element of $p^{-1}(b)$: a section of $E$ over the patch $f^{-1}(x)$. (iii) The adjunction gives $(\mathcal{C}/B)(f^* 1, E) \cong (\mathcal{C}/A)(\langle 1, x \rangle, \Pi_f E)$, and the right side is the set of points of $\Pi_f E$ lying over $x$; so that fiber *is* the set of partial sections over $f^{-1}(x)$. (iv) If $A = 1$ then $f^{-1}(x) = B$ and $\Pi_f E = S(E)$, the object of global sections.

> Sources: DaoFP Exercise 11.4.2.
