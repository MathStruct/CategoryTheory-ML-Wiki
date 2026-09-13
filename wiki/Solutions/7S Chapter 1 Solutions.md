#solution

Solutions to the exercises of 7 Sketches, Chapter 1: [[7S Chapter 1 Exercises]]. Index: [[Map of Content]].

## Solution 1.1

[[7S Chapter 1 Exercises#Exercise 1.1|Exercise 1.1]]

- **Order**: $f(x) = x + 5$ preserves order; $g(x) = -x$ does not ($1 \leq 2$ but $-1 \not\leq -2$).
- **Metric**: $f(x) = x + 5$ preserves the metric; $g(x) = 2x$ does not ($|1 - 2| = 1$ but $|2 - 4| = 2$).
- **Addition**: $f(x) = 3x$ preserves addition; $g(x) = x + 1$ does not ($g(0 + 0) = 1 \neq 2 = g(0) + g(0)$).

The moral: "asking which aspects of $X$ one wants to preserve under the observation $f$ becomes the question *what category are you working in?*"

> Sources: 7 Sketches, Exercise 1.1 and Solution A.1.

## Solution 1.4

[[7S Chapter 1 Exercises#Exercise 1.4|Exercise 1.4]]

Take the transitive closure of the union of connections: $11 \sim 12$ and $12 \sim 13$ give $\{11, 12, 13\}$, and the second system adds nothing to the bottom row, so the join is $\{11,12,13\},\{21,22,23\}$.

> Sources: 7 Sketches, Exercise 1.4 and Solution A.1.

## Solution 1.6

[[7S Chapter 1 Exercises#Exercise 1.6|Exercise 1.6]]

1. $(1)(2) \leq (12)$: two elements, one arrow.
2. The 15 partitions of $\{1,2,3,4\}$ in four rows: bottom $(1)(2)(3)(4)$; then the six with one pair $(12)(3)(4), (13)(2)(4), (14)(2)(3), (1)(23)(4), (1)(24)(3), (1)(2)(34)$; then the seven with a triple or two pairs $(123)(4), (124)(3), (134)(2), (1)(234), (12)(34), (13)(24), (14)(23)$; top $(1234)$.
   Choose $A = (12)(3)(4)$, $B = (13)(2)(4)$.
3. $A \vee B = (123)(4)$.
4. Yes.
5. $C \in \{(123)(4), (1234)\}$.
6. Yes: $(123)(4) \leq (123)(4)$ and $(123)(4) \leq (1234)$.

> Sources: 7 Sketches, Exercise 1.6 and Solution A.1.

## Solution 1.7

[[7S Chapter 1 Exercises#Exercise 1.7|Exercise 1.7]]

$\mathsf{true}$, $\mathsf{true}$, $\mathsf{true}$, $\mathsf{false}$ — the join in $\mathbb{B}$ is OR.

> Sources: 7 Sketches, Exercise 1.7 and Solution A.1.

## Solution 1.10

[[7S Chapter 1 Exercises#Exercise 1.10|Exercise 1.10]]

1. True. 2. False: $0 \in \mathbb{N}$ but $0 \notin \{n \geq 1\}$. 3. True: no integer lies strictly between $1$ and $2$.

> Sources: 7 Sketches, Exercise 1.10 and Solution A.1.

## Solution 1.11

[[7S Chapter 1 Exercises#Exercise 1.11|Exercise 1.11]]

1. $\varnothing, \{1\}, \{2\}, \{3\}, \{1,2\}, \{1,3\}, \{2,3\}, \{1,2,3\}$.
2. E.g. $\{1,2,3\} \cup \{1\} = \{1,2,3\}$.
3. $(h,1), (h,2), (h,3), (1,1), (1,2), (1,3)$.
4. $(h,1), (1,1), (1,2), (2,2), (3,2)$ (tagged by which set they come from).
5. $h, 1, 2, 3$.

> Sources: 7 Sketches, Exercise 1.11 and Solution A.1.

## Solution 1.16

#proof — [[7S Chapter 1 Exercises#Exercise 1.16|Exercise 1.16]]

1. If $A_p = A'_{p'_1} = A'_{p'_2}$ then $A'_{p'_1} \cap A'_{p'_2} = A'_{p'_1} \neq \varnothing$; by the partition axiom distinct labels have disjoint parts, so $p'_1 = p'_2$.
2. Pick $a \in A'_{p'}$ (parts are nonempty). Since $A = \bigcup_p A_p$ there is $p$ with $a \in A_p$, and by assumption $A_p = A'_{p''}$ for some $p''$. Then $a \in A'_{p'} \cap A'_{p''}$, so $p' = p''$ and $A_p = A'_{p'}$.

Hence "same partition up to relabeling" is a well-defined notion.

> Sources: 7 Sketches, Exercise 1.16 and Solution A.1.

## Solution 1.17

[[7S Chapter 1 Exercises#Exercise 1.17|Exercise 1.17]]

$(11,11), (11,12), (12,11), (12,12), (13,13), (21,21), (22,22), (22,23), (23,22), (23,23)$.

> Sources: 7 Sketches, Exercise 1.17 and Solution A.1.

## Solution 1.20

#proof — [[7S Chapter 1 Exercises#Exercise 1.20|Exercise 1.20]]

1. Connected subsets are nonempty by definition.
2. Suppose $a \in A_p \cap A_q$. For $a' \in A_p$, connectedness gives $a \sim a'$, and closedness of $A_q$ gives $a' \in A_q$; symmetrically $A_q \subseteq A_p$. So $A_p = A_q$, contradicting $p \neq q$.
3. For $a \in A$ let $X := \{a' \mid a' \sim a\}$. $X$ is closed (if $a' \in X$ and $b \sim a'$ then $b \sim a$ by transitivity/symmetry), connected (if $b, c \in X$ then $b \sim c$), and contains $a$ (reflexivity). So $a$ lies in some part.

> Sources: 7 Sketches, Exercise 1.20 and Solution A.1.

## Solution 1.24

[[7S Chapter 1 Exercises#Exercise 1.24|Exercise 1.24]]

1. The unique function $\varnothing \to \{1\}$. 2. The unique function $\{a, b\} \to \{1\}$.
3–4. The second and third relations are not functions (the second is not *deterministic* — one element related to two — and neither is *total*). The first is a function that is neither injective nor surjective; the fourth is a [[Bijection]].

> Sources: 7 Sketches, Exercise 1.24 and Solution A.1.

## Solution 1.25

#proof — [[7S Chapter 1 Exercises#Exercise 1.25|Exercise 1.25]]

By Definition 1.22, $f$ is a subset $F \subseteq A \times \varnothing$ such that for every $a \in A$ there is a unique $b \in \varnothing$ with $(a, b) \in F$. There are no $b \in \varnothing$, so there can be no $a \in A$: $A = \varnothing$. (Categorically: $\varnothing$ is [[Initial Object|initial]] and *strict* in $\mathbf{Set}$ — any map into it is an isomorphism.)

> Sources: 7 Sketches, Exercise 1.25 and Solution A.1.

## Solution 1.27

[[7S Chapter 1 Exercises#Exercise 1.27|Exercise 1.27]]

| partition | surjection onto |
|---|---|
| $(\bullet)(\ast)(\circ)$ | $\{p_1, p_2, p_3\}$, $\bullet \mapsto p_1, \ast \mapsto p_2, \circ \mapsto p_3$ |
| $(\bullet\ast)(\circ)$ | $\{p_1, p_2\}$, $\bullet, \ast \mapsto p_1$, $\circ \mapsto p_2$ |
| $(\bullet\circ)(\ast)$ | $\{p_1, p_2\}$, $\bullet, \circ \mapsto p_1$, $\ast \mapsto p_2$ |
| $(\bullet)(\ast\circ)$ | $\{p_1, p_2\}$, $\bullet \mapsto p_1$, $\ast, \circ \mapsto p_2$ |
| $(\bullet\ast\circ)$ | $\{p_1\}$, everything $\mapsto p_1$ |

> Sources: 7 Sketches, Exercise 1.27 and Solution A.1.

## Solution 1.38

[[7S Chapter 1 Exercises#Exercise 1.38|Exercise 1.38]]

$a : 1 \to 2$, $b : 1 \to 3$, $c : 1 \to 3$, $d : 2 \to 2$, $e : 2 \to 3$.

> Sources: 7 Sketches, Exercise 1.38 and Solution A.1.

## Solution 1.40

[[7S Chapter 1 Exercises#Exercise 1.40|Exercise 1.40]]

$P = \{1,2,3,4\}$ with $p \leq q$ iff there is a path $p \to q$: $1 \leq 1, 1 \leq 2, 1 \leq 3, 2 \leq 2, 2 \leq 3, 3 \leq 3, 4 \leq 4$. The parallel arrows $b, c$ and the loop $d$ are "useless" from a preorder point of view but do no harm.

> Sources: 7 Sketches, Exercise 1.40 and Solution A.1.

## Solution 1.41

[[7S Chapter 1 Exercises#Exercise 1.41|Exercise 1.41]]

Yes: it is the Hasse diagram of the discrete order $x \leq y$ iff $x = y$ (a graph with no arrows).

> Sources: 7 Sketches, Exercise 1.41 and Solution A.1.

## Solution 1.42

[[7S Chapter 1 Exercises#Exercise 1.42|Exercise 1.42]]

Writing $\alpha = (\bullet)(\circ)(\ast)$, $\beta = (\bullet\circ)(\ast)$, $\gamma = (\bullet\ast)(\circ)$, $\delta = (\bullet)(\circ\ast)$, $\omega = (\bullet\circ\ast)$: the five reflexive pairs $\alpha \leq \alpha, \dots, \omega \leq \omega$; $\alpha \leq \beta, \alpha \leq \gamma, \alpha \leq \delta, \alpha \leq \omega$; $\beta \leq \omega, \gamma \leq \omega, \delta \leq \omega$.

> Sources: 7 Sketches, Exercise 1.42 and Solution A.1.

## Solution 1.44

[[7S Chapter 1 Exercises#Exercise 1.44|Exercise 1.44]]

Almost: every element is comparable with itself. A discrete preorder is one where $x$ and $y$ are comparable iff $x = y$.

> Sources: 7 Sketches, Exercise 1.44 and Solution A.1.

## Solution 1.46

[[7S Chapter 1 Exercises#Exercise 1.46|Exercise 1.46]]

No: e.g. $4 \nmid 6$ and $6 \nmid 4$, so $4$ and $6$ are incomparable.

> Sources: 7 Sketches, Exercise 1.46 and Solution A.1.

## Solution 1.48

[[7S Chapter 1 Exercises#Exercise 1.48|Exercise 1.48]]

Yes: for all $a, b \in \mathbb{R}$, either $a \leq b$ or $b \leq a$. See [[Real Numbers]].

> Sources: 7 Sketches, Exercise 1.48 and Solution A.1.

## Solution 1.51

[[7S Chapter 1 Exercises#Exercise 1.51|Exercise 1.51]]

$\mathcal{P}(\varnothing) = \{\varnothing\}$: a single point. $\mathcal{P}\{1\}$: $\varnothing \to \{1\}$. $\mathcal{P}\{1,2\}$: a square $\varnothing \to \{1\}, \{2\} \to \{1,2\}$.

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}[row sep=small]
\varnothing & & \{1\} & & & \{1,2\} & \\
 & & \varnothing \arrow[u] & & \{1\} \arrow[ur] & & \{2\} \arrow[ul] \\
 & & & & & \varnothing \arrow[ul] \arrow[ur] &
\end{tikzcd}
\end{document}
```

> Sources: 7 Sketches, Exercise 1.51 and Solution A.1.

## Solution 1.53

[[7S Chapter 1 Exercises#Exercise 1.53|Exercise 1.53]]

Coarsest: the unique map $! : S \to \{1\}$. Finest: the identity $\mathrm{id}_S : S \to S$. See [[Preorder of Partitions]].

> Sources: 7 Sketches, Exercise 1.53 and Solution A.1.

## Solution 1.55

#proof — [[7S Chapter 1 Exercises#Exercise 1.55|Exercise 1.55]]

Every subset $U \subseteq X$ is an upper set: if $p \in U$, the only $q$ with $p \leq q$ is $p$ itself, which is in $U$. So $\mathcal{U}(X)$ contains all subsets and is ordered by inclusion, i.e. $\mathcal{U}(X) = \mathcal{P}(X)$.

> Sources: 7 Sketches, Exercise 1.55 and Solution A.1.

## Solution 1.57

[[7S Chapter 1 Exercises#Exercise 1.57|Exercise 1.57]]

The product has six elements: $(a,1)$ at the bottom; $(c,1), (a,2), (b,1)$ above it; $(c,2)$ above $(c,1), (a,2)$; $(b,2)$ above $(a,2), (b,1)$ (diagram in [[Product Preorder]]).

Upper sets (14 of them, ordered by inclusion): $\varnothing$; $\{(c,2)\}$, $\{(b,2)\}$; $\{(c,1),(c,2)\}$, $\{(b,2),(c,2)\}$, $\{(b,1),(b,2)\}$; $\{(b,2),(c,1),(c,2)\}$, $\{(a,2),(b,2),(c,2)\}$, $\{(b,1),(b,2),(c,2)\}$; $\{(a,2),(b,2),(c,1),(c,2)\}$, $\{(a,2),(b,1),(b,2),(c,2)\}$; $\{(a,2),(b,1),(b,2),(c,1),(c,2)\}$; and all six elements.

> Sources: 7 Sketches, Exercise 1.57 and Solution A.1.

## Solution 1.63

[[7S Chapter 1 Exercises#Exercise 1.63|Exercise 1.63]]

$\mathcal{P}(X)$ is the cube in [[Power Set]]; the chain is $0 \to 1 \to 2 \to 3$; $|\cdot|$ sends $\varnothing \mapsto 0$, singletons $\mapsto 1$, pairs $\mapsto 2$, $X \mapsto 3$ — each level of the cube to the corresponding element of the chain. It is a [[Monotone Map]].

> Sources: 7 Sketches, Exercise 1.63 and Solution A.1.

## Solution 1.65

[[7S Chapter 1 Exercises#Exercise 1.65|Exercise 1.65]]

$\mathcal{U}(\mathbb{B}) = \varnothing \to \{\mathsf{true}\} \to \{\mathsf{true},\mathsf{false}\}$ maps into the square $\mathcal{P}(\mathbb{B})$ by inclusion, missing only $\{\mathsf{false}\}$. See [[Upper Set]], [[Booleans]].

> Sources: 7 Sketches, Exercise 1.65 and Solution A.1.

## Solution 1.66

#proof — [[7S Chapter 1 Exercises#Exercise 1.66|Exercise 1.66]]

See [[Yoneda Lemma for Preorders]] for the proofs. For 4: $\uparrow a = \{a,b,c\}$, $\uparrow b = \{b\}$, $\uparrow c = \{c\}$; in $\mathcal{U}(P)$, $\{b\}, \{c\} \subseteq \{b, c\} \subseteq \{a,b,c\}$, and $\uparrow$ sends the bottom element $a$ of $P$ to the top element of $\mathcal{U}(P)$.

> Sources: 7 Sketches, Exercise 1.66 and Solution A.1.

## Solution 1.67

#proof — [[7S Chapter 1 Exercises#Exercise 1.67|Exercise 1.67]]

If $p_1 \leq_P p_2$ then $p_1 = p_2$ (discreteness), so $f(p_1) = f(p_2)$, and hence $f(p_1) \leq_Q f(p_2)$ by reflexivity.

> Sources: 7 Sketches, Exercise 1.67 and Solution A.1.

## Solution 1.69

[[7S Chapter 1 Exercises#Exercise 1.69|Exercise 1.69]]

Let $X = \mathbb{Z}$, $Y = \{n, z, p\}$, $f$ sending negatives to $n$, zero to $z$, positives to $p$. With $P = (nz)(p)$ and $Q = (np)(z)$: $f^*(P) = \{\{x \leq 0\}, \{x \geq 1\}\}$ and $f^*(Q) = \{\{0\}, \{x \neq 0\}\}$.

> Sources: 7 Sketches, Exercise 1.69 and Solution A.1.

## Solution 1.71

#proof — [[7S Chapter 1 Exercises#Exercise 1.71|Exercise 1.71]]

1. $\mathrm{id}_P(p) = p$, so $p_1 \leq p_2$ implies $\mathrm{id}(p_1) \leq \mathrm{id}(p_2)$.
2. $p_1 \leq p_2 \Rightarrow f(p_1) \leq f(p_2) \Rightarrow g(f(p_1)) \leq g(f(p_2))$, which is monotonicity of $f \mathbin{;} g$. See [[Category of Preorders]].

> Sources: 7 Sketches, Exercise 1.71 and Solution A.1.

## Solution 1.73

#proof — [[7S Chapter 1 Exercises#Exercise 1.73|Exercise 1.73]]

Skeletal: $p_1 \leq p_2$ and $p_2 \leq p_1$ imply $p_1 = p_2$. Dagger: $p_1 \leq p_2$ implies $p_2 \leq p_1$. Hence $p_1 \leq p_2$ implies $p_1 = p_2$, which is the definition of discrete. So such a preorder "can be identified with" its underlying set.

> Sources: 7 Sketches, Exercise 1.73 and Solution A.1.

## Solution 1.77

#proof — [[7S Chapter 1 Exercises#Exercise 1.77|Exercise 1.77]]

Let $P \leq Q$ be partitions, i.e. $P$ is finer: $x \sim_P y$ implies $x \sim_Q y$. If $\Phi(P) = \mathsf{true}$ then $\bullet \sim_P \ast$, hence $\bullet \sim_Q \ast$, so $\Phi(Q) = \mathsf{true}$. Thus $\Phi(P) \leq \Phi(Q)$. It nonetheless has a [[Generative Effect]].

> Sources: 7 Sketches, Exercise 1.77 and Solution A.1.

## Solution 1.79

#proof — [[7S Chapter 1 Exercises#Exercise 1.79|Exercise 1.79]]

Let $u : Q \to \mathbb{B}$ classify $U$, i.e. $u(q) = \mathsf{true}$ iff $q \in U$. Then $(f \mathbin{;} u)(p) = \mathsf{true}$ iff $f(p) \in U$ iff $p \in f^{-1}(U)$, so $f \mathbin{;} u$ classifies $f^{-1}(U) = f^*(U)$. This is the preorder version of [[Direct Image, Preimage, and Dual Image|preimage as precomposition]] (Kittenlab Lecture 14).

> Sources: 7 Sketches, Exercise 1.79 and Solution A.1.

## Solution 1.80

#proof — [[7S Chapter 1 Exercises#Exercise 1.80|Exercise 1.80]]

1. $0 \leq \frac{1}{n+1}$ for all $n$.
2. Suppose $b$ is a lower bound with $0 < b$. Pick $n$ with $1/b < n + 1$; then $\frac{1}{n+1} < b$, contradicting that $b$ is a lower bound. So every lower bound is $\leq 0$.

> Sources: 7 Sketches, Exercise 1.80 and Solution A.1.

## Solution 1.85

#proof — [[7S Chapter 1 Exercises#Exercise 1.85|Exercise 1.85]]

1. $p \leq a$ for the only $a = p$; and if $q \leq p$ then $q \leq p$: so $p$ is a [[Meet]]. Any other meet $q$ satisfies $q \leq p$ and $p \leq q$, so $q \cong p$.
2. In a partial order $p \cong q$ implies $p = q$.
3. Yes; replace $\leq$ by $\geq$ and "meet" by "[[Join|join]]" throughout.

> Sources: 7 Sketches, Exercise 1.85 and Solution A.1.

## Solution 1.90

[[7S Chapter 1 Exercises#Exercise 1.90|Exercise 1.90]]

$4 \wedge 6 = 2$ and $4 \vee 6 = 12$: the meet is the **greatest common divisor**, the join the **least common multiple**.

> Sources: 7 Sketches, Exercise 1.90 and Solution A.1.

## Solution 1.94

#proof — [[7S Chapter 1 Exercises#Exercise 1.94|Exercise 1.94]]

Since $a \leq a \vee b$ and $b \leq a \vee b$, monotonicity gives $f(a) \leq f(a \vee b)$ and $f(b) \leq f(a \vee b)$. So $f(a \vee b)$ is an upper bound of $\{f(a), f(b)\}$, and the join $f(a) \vee f(b)$ is the least one: $f(a) \vee f(b) \leq f(a \vee b)$.

> Sources: 7 Sketches, Exercise 1.94 and Solution A.1.

## Solution 1.98

#proof — [[7S Chapter 1 Exercises#Exercise 1.98|Exercise 1.98]]

The right adjoint is $\lfloor -/3 \rfloor : \mathbb{R} \to \mathbb{Z}$; we must show $3z \leq r$ iff $z \leq \lfloor r/3 \rfloor$. If $z \leq \lfloor r/3 \rfloor$ then $3z \leq 3 \lfloor r/3 \rfloor \leq r$. If $3z \leq r$ then $z \leq r/3$, and since $z$ is an integer below $r/3$ it is below the greatest such, $\lfloor r/3 \rfloor$.

> Sources: 7 Sketches, Exercise 1.98 and Solution A.1.

## Solution 1.99

[[7S Chapter 1 Exercises#Exercise 1.99|Exercise 1.99]]

1. $f = (1 \mapsto 1, 2 \mapsto 1, 3 \mapsto 3)$, $g = (1 \mapsto 2, 2 \mapsto 2, 3 \mapsto 3)$. Checking all nine pairs, $f(p) \leq q$ iff $p \leq g(q)$ holds (for $(p,q) = (3,1), (3,2)$ both sides fail; otherwise both hold), so $f \dashv g$.
2. Here $f(2) = 1$ but $2 \not\leq g(1)$, so $f$ is *not* left adjoint to $g$. In pictures of [[Total Order|total orders]], adjoint pairs are exactly those whose bent arrows do not cross (Remark 1.100).

> Sources: 7 Sketches, Exercise 1.99 and Solution A.1.

## Solution 1.101

#proof — [[7S Chapter 1 Exercises#Exercise 1.101|Exercise 1.101]]

Suppose $L \dashv \lceil -/3 \rceil$. Then $L(z) \leq r$ iff $z \leq \lceil r/3 \rceil$. Take $z = 1$, $r = 0.01$: $\lceil 0.01/3 \rceil = 1 \geq 1$, so $L(1) \leq 0.01$; similarly $L(1) \leq r$ for every $r > 0$, so $L(1) \leq 0$. But then $1 \leq \lceil 0/3 \rceil = 0$, a contradiction. So there is no left adjoint. (Equivalently: $\lceil -/3 \rceil$ does not preserve meets — $\bigwedge_{r > 0} \lceil r/3 \rceil = 1 \neq 0 = \lceil \bigwedge_{r>0} r / 3 \rceil$; see [[Adjoint Functor Theorem for Preorders]].)

> Sources: 7 Sketches, Exercise 1.101 and Solution A.1.

## Solution 1.103

[[7S Chapter 1 Exercises#Exercise 1.103|Exercise 1.103]]

$g_!((1)(2)(3)(4)) = (12)(3)(4)$; $g_!((12)(3)(4)) = (12)(3)(4)$; $g_!((13)(2)(4)) = (12\,3)(4)$; $g_!((1)(2)(34)) = (12)(34)$; $g_!((14)(23)) = (12\,3\,4)$; $g_!((1)(234)) = (12\,3\,4)$. In general merge $1, 2$ into $12$ and take the transitive closure.

> Sources: 7 Sketches, Exercise 1.103 and Solution A.1.

## Solution 1.105

[[7S Chapter 1 Exercises#Exercise 1.105|Exercise 1.105]]

$s_1 \sim s_2$ iff $g(s_1) \sim g(s_2)$; since $g(1) = g(2)$, elements $1, 2$ are always identified: $(12)(3)(4)$, $(12\,3)(4)$, $(12\,4)(3)$, $(12)(34)$, $(1234)$.

> Sources: 7 Sketches, Exercise 1.105 and Solution A.1.

## Solution 1.106

[[7S Chapter 1 Exercises#Exercise 1.106|Exercise 1.106]]

Take $c = (13)(2)(4)$; then $g_!(c) = (12\,3)(4)$. Let $d = (12\,3\,4)$ (coarser) and $e = (12)(34)$ (not coarser). Then $g^*(d) = (1234)$ and $g^*(e) = (12)(34)$. Indeed $c \leq g^*(d)$, but $c \not\leq g^*(e)$ since $1 \sim_c 3$ while $1 \not\sim 3$ in $(12)(34)$ — consistent with the [[Galois Connection]] formula $g_!(c) \leq d \iff c \leq g^*(d)$.

> Sources: 7 Sketches, Exercise 1.106 and Solution A.1.

## Solution 1.109

#proof — [[7S Chapter 1 Exercises#Exercise 1.109|Exercise 1.109]]

1. Apply the definition with $p := g(q)$ to the reflexivity fact $g(q) \leq g(q)$: $f(g(q)) \leq q$.
2. If $p \leq g(q)$, apply $f$: $f(p) \leq f(g(q)) \leq q$. If $f(p) \leq q$, apply $g$: $p \leq g(f(p)) \leq g(q)$.

> Sources: 7 Sketches, Exercise 1.109 and Solution A.1.

## Solution 1.110

#proof — [[7S Chapter 1 Exercises#Exercise 1.110|Exercise 1.110]]

1. Using $p \leq g'(f(p))$ with $p = g(q)$ and monotonicity of $g'$ applied to $f(g(q)) \leq q$: $g(q) \leq g'(f(g(q))) \leq g'(q)$. Symmetrically $g'(q) \leq g(q)$.
2. Yes, by the dual argument. (Categorically: adjoints are unique up to unique [[Natural Isomorphism]].)

> Sources: 7 Sketches, Exercise 1.110 and Solution A.1.

## Solution 1.112

#proof — [[7S Chapter 1 Exercises#Exercise 1.112|Exercise 1.112]]

Let $f \dashv g$, $A \subseteq P$ with join $j$. Monotonicity gives $f(a) \leq f(j)$ for all $a \in A$, so $f(j)$ is an upper bound of $f(A)$. If $b$ is another upper bound, $f(a) \leq b$ for all $a$, so by adjunction $a \leq g(b)$ for all $a$, hence $j \leq g(b)$, hence $f(j) \leq b$. So $f(j) = \bigvee f(A)$.

> Sources: 7 Sketches, Exercise 1.112 and Solution A.1.

## Solution 1.114

[[7S Chapter 1 Exercises#Exercise 1.114|Exercise 1.114]]

| $p$ | $q$ | $f(p) \leq q$ | $p \leq g(q)$ |
|---|---|---|---|
| 1 | 1 | yes | yes |
| 1 | 2 | no | no |
| 1 | 4 | yes | yes |
| 2 | 1 | no | no |
| 2 | 2 | yes | yes |
| 2 | 4 | yes | yes |
| 3.9 | 1 | no | no |
| 3.9 | 2 | no | no |
| 3.9 | 4 | yes | yes |
| 4 | 1 | no | no |
| 4 | 2 | no | no |
| 4 | 4 | yes | yes |

All agree, so $f \dashv g$; yet $g$ does not preserve joins ([[Right Adjoints Preserve Meets]] but not joins).

> Sources: 7 Sketches, Exercise 1.114 and Solution A.1.

## Solution 1.118

[[7S Chapter 1 Exercises#Exercise 1.118|Exercise 1.118]]

$X = \{a_1, c_1, c_2\}$, $Y = \{a, b, c\}$, $f$ "projects down" ($a_1 \mapsto a$, $c_i \mapsto c$). 1. $f^*\{a, b\} = \{a_1\}$, $f^*\{c\} = \{c_1, c_2\}$. 2. $f_!\varnothing = \varnothing$, $f_!\{a_1, c_1\} = \{a, c\}$. 3. $f_*\varnothing = \{b\}$, $f_*\{a_1, c_1\} = \{a, b\}$.

> Sources: 7 Sketches, Exercise 1.118 and Solution A.1.

## Solution 1.119

#proof — [[7S Chapter 1 Exercises#Exercise 1.119|Exercise 1.119]]

1. This is the unit inequality of Proposition 1.107.
2. $\geq$: apply (1) to $g(f(p))$. $\leq$: the counit gives $f(g(f(p))) \leq f(p)$; apply the monotone $g$ to get $g(f(g(f(p)))) \leq g(f(p))$.

> Sources: 7 Sketches, Exercise 1.119 and Solution A.1.

## Solution 1.124

[[7S Chapter 1 Exercises#Exercise 1.124|Exercise 1.124]]

$\mathrm{Rel}(\{1,2\}) = \mathcal{P}(\{1,2\} \times \{1,2\})$ is the [[Power Set]] of a four-element set: a 4-dimensional cube with 16 vertices, from $\varnothing$ at the bottom to the total relation $\{(1,1),(1,2),(2,1),(2,2)\}$ at the top.

> Sources: 7 Sketches, Exercise 1.124 and Solution A.1.

## Solution 1.125

[[7S Chapter 1 Exercises#Exercise 1.125|Exercise 1.125]]

1. Take $1 \leq 2 \leq 3$: $U(\leq) = \{(1,1),(2,2),(3,3),(1,2),(2,3),(1,3)\}$.
2. $Q = \{(1,2)\}$, $Q' = \{(2,1)\}$.
3. $\mathrm{Cl}(Q) = \{(1,1),(2,2),(3,3),(1,2)\} \subseteq U(\leq)$.
4. $\mathrm{Cl}(Q') \ni (2,1) \notin U(\leq)$. This illustrates the adjunction $\mathrm{Cl} \dashv U$ of [[Reflexive Transitive Closure]].

> Sources: 7 Sketches, Exercise 1.125 and Solution A.1.
