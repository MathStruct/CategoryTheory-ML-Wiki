#solution

Solutions to the exercises of 7 Sketches, Chapter 4: [[7S Chapter 4 Exercises]]. Index: [[Map of Content]].

## Solution 4.4

[[7S Chapter 4 Exercises#Exercise 4.4|Exercise 4.4]]

1. Six elements: $(\mathsf{category}, \mathsf{nothing})$ at the bottom, then $(\mathsf{monoid}, \mathsf{nothing})$, $(\mathsf{preorder}, \mathsf{nothing})$, $(\mathsf{category}, \mathsf{this\ book})$, then $(\mathsf{monoid}, \mathsf{this\ book})$, $(\mathsf{preorder}, \mathsf{this\ book})$ on top — tasks in *decreasing difficulty*.
2. E.g. $\Lambda = \mathsf{true}$ on $(\mathsf{monoid}, -)$, $(\mathsf{preorder}, \mathsf{this\ book})$, $(\mathsf{category}, \mathsf{this\ book})$ and $\mathsf{false}$ on $(\mathsf{preorder}, \mathsf{nothing})$, $(\mathsf{category}, \mathsf{nothing})$: she can explain monoids unaided and categories with the book. Upper set: if she can do a task, she can do any easier one. See [[Feasibility Relation]].

> Sources: 7 Sketches, Exercise 4.4 and Solution A.4.

## Solution 4.7

[[7S Chapter 4 Exercises#Exercise 4.7|Exercise 4.7]]

Same as [[7S Chapter 2 Exercises#Exercise 2.84|7S Exercise 2.84]]: if $c = \mathsf{false}$ both sides always hold; if $c = \mathsf{true}$ both sides say $b \leq d$. See [[Bool (Monoidal Preorder)]].

> Sources: 7 Sketches, Exercise 4.7 and Solution A.4.

## Solution 4.9

#proof — [[7S Chapter 4 Exercises#Exercise 4.9|Exercise 4.9]]

A $\mathcal{V}$-functor condition reads $(\mathcal{X}^{\mathrm{op}} \times \mathcal{Y})((x,y),(x',y')) \leq \mathcal{V}(\Phi(x,y), \Phi(x',y'))$, i.e. $\mathcal{X}(x', x) \otimes \mathcal{Y}(y, y') \leq \Phi(x, y) \multimap \Phi(x', y')$ using the [[Opposite Enriched Category|opposite]], the [[Product of Enriched Categories|product]] and self-enrichment. By the hom-element adjunction (2.80) and symmetry this is $\mathcal{X}(x',x) \otimes \Phi(x,y) \otimes \mathcal{Y}(y,y') \leq \Phi(x',y')$.

> Sources: 7 Sketches, Exercise 4.9 and Solution A.4.

## Solution 4.10

[[7S Chapter 4 Exercises#Exercise 4.10|Exercise 4.10]]

Yes: a $\mathbf{Bool}$-functor is exactly a monotone map, so the definitions line up perfectly.

> Sources: 7 Sketches, Exercise 4.10 and Solution A.4.

## Solution 4.12

[[7S Chapter 4 Exercises#Exercise 4.12|Exercise 4.12]]

| $\Phi$ | a | b | c | d | e |
|---|---|---|---|---|---|
| N | t | f | t | f | t |
| E | t | t | t | t | t |
| W | t | f | t | f | t |
| S | t | t | t | t | t |

> Sources: 7 Sketches, Exercise 4.12 and Solution A.4.

## Solution 4.15

[[7S Chapter 4 Exercises#Exercise 4.15|Exercise 4.15]]

| $\Phi$ | x | y | z |
|---|---|---|---|
| A | 17 | 20 | 20 |
| B | 11 | 14 | 14 |
| C | 14 | 17 | 17 |
| D | 12 | 9 | 15 |

> Sources: 7 Sketches, Exercise 4.15 and Solution A.4.

## Solution 4.17

[[7S Chapter 4 Exercises#Exercise 4.17|Exercise 4.17]]

$M_X^3 = d_X$ (the distance matrix of [[7S Chapter 2 Exercises#Exercise 2.58|7S Exercise 2.58]]), $M_Y^2 = d_Y$; $d_X \ast M_\Phi$ has columns $(17, 11, 14, 20)$, $(20, 14, 17, 9)$, $(\infty, \infty, \infty, \infty)$, and multiplying by $d_Y$ gives exactly the matrix of Exercise 4.15. They agree. See [[Matrix Multiplication in a Quantale]].

> Sources: 7 Sketches, Exercise 4.17 and Solution A.4.

## Solution 4.18

[[7S Chapter 4 Exercises#Exercise 4.18|Exercise 4.18]]

Valid: $\Phi((\mathsf{g/n}, \mathsf{funny}), p) = \mathsf{false}$ for all $p \in \$$ — a good-natured funny movie is not feasible at any of the listed costs (at least not under a million dollars). See [[Co-design]].

> Sources: 7 Sketches, Exercise 4.18 and Solution A.4.

## Solution 4.22

[[7S Chapter 4 Exercises#Exercise 4.22|Exercise 4.22]]

All shortest paths go through the bridges $D \to y$ (length 9) and $y \to r$ (length 0), so $(\Phi \mathbin{;} \Psi)(-, -) = X(-, D) + 9 + Z(r, -)$:

| | p | q | r | s |
|---|---|---|---|---|
| A | 22 | 24 | 20 | 21 |
| B | 16 | 18 | 14 | 15 |
| C | 19 | 21 | 17 | 18 |
| D | 11 | 13 | 9 | 10 |

Alternatively $\Phi \ast M_\Psi \ast M_Z^3$ by min-plus multiplication. See [[Category of Profunctors]].

> Sources: 7 Sketches, Exercise 4.22 and Solution A.4.

## Solution 4.26

[[7S Chapter 4 Exercises#Exercise 4.26|Exercise 4.26]]

Take $X$ from Eq. (2.56); draw two copies of its weighted graph side by side and connect each vertex to its copy by a bridge of length $0$. Then $U_X(x, y) = d_X(x, y)$. See [[Category of Profunctors]].

> Sources: 7 Sketches, Exercise 4.26 and Solution A.4.

## Solution 4.30

#proof — [[7S Chapter 4 Exercises#Exercise 4.30|Exercise 4.30]]

1. (4.28): $\Phi(p,q) = I \otimes \Phi(p,q)$ (unitality); $\leq P(p,p) \otimes \Phi(p,q)$ (monotonicity of $\otimes$ with $I \leq P(p,p)$); $\leq \bigvee_{p_1} P(p,p_1) \otimes \Phi(p_1,q)$ (a join bounds each term); $= (U_P \mathbin{;} \Phi)(p,q)$ (definition).
2. In $\mathbf{Bool}$, $I = \mathsf{true}$ is top, so $P(p,p) = \mathsf{true}$ and the first inequality is an equality. For the second: if $\Phi(p,q) = \mathsf{true}$ equality is forced; if $\mathsf{false}$, then whenever $P(p, p_1) = \mathsf{true}$ monotonicity gives $\Phi(p_1, q) \leq \Phi(p, q) = \mathsf{false}$, so every term of the join is $\mathsf{false}$.
3. (4.29): $v \otimes I = v$; $I \leq Q(q,q)$ with monotonicity; the profunctor inequality of [[7S Chapter 4 Exercises#Exercise 4.9|7S Exercise 4.9]].

> Sources: 7 Sketches, Exercise 4.30 and Solution A.4.

## Solution 4.32

#proof — [[7S Chapter 4 Exercises#Exercise 4.32|Exercise 4.32]]

As in [[7S Chapter 2 Exercises#Exercise 2.104|7S Exercise 2.104]]: $((\Phi \mathbin{;} \Psi) \mathbin{;} \Upsilon)(p,s) = \bigvee_r \big(\bigvee_q \Phi(p,q) \otimes \Psi(q,r)\big) \otimes \Upsilon(r,s) = \bigvee_{q,r} \Phi(p,q) \otimes \Psi(q,r) \otimes \Upsilon(r,s) = \bigvee_q \Phi(p,q) \otimes \big(\bigvee_r \Psi(q,r) \otimes \Upsilon(r,s)\big) = (\Phi \mathbin{;} (\Psi \mathbin{;} \Upsilon))(p,s)$, using distributivity of $\otimes$ over $\vee$ (closedness) and skeletality to turn $\cong$ into $=$.

> Sources: 7 Sketches, Exercise 4.32 and Solution A.4.

## Solution 4.36

[[7S Chapter 4 Exercises#Exercise 4.36|Exercise 4.36]]

$\widehat{\mathrm{id}}(p, q) = P(\mathrm{id}(p), q) = P(p, q) = U_P(p, q)$.

> Sources: 7 Sketches, Exercise 4.36 and Solution A.4.

## Solution 4.38

[[7S Chapter 4 Exercises#Exercise 4.38|Exercise 4.38]]

$\check{+} : \mathbb{R} \nrightarrow \mathbb{R}^3$, $(a, (b, c, d)) \mapsto \mathbb{R}(a, b + c + d) = [a \leq b + c + d]$.

> Sources: 7 Sketches, Exercise 4.38 and Solution A.4.

## Solution 4.41

#proof — [[7S Chapter 4 Exercises#Exercise 4.41|Exercise 4.41]]

1. $\hat F(p,q) = Q(F p, q)$ and $\check G(p, q) = Q(p, G q)$; by skeletality, adjointness $Q(Fp, q) \cong P(p, Gq)$ is the equality $\hat F = \check G$.
2. $\mathrm{id}$ is adjoint to itself (both sides equal $P(p,q)$), so $\widehat{\mathrm{id}} = \check{\mathrm{id}}$. See [[Companion and Conjoint]].

> Sources: 7 Sketches, Exercise 4.41 and Solution A.4.

## Solution 4.44

[[7S Chapter 4 Exercises#Exercise 4.44|Exercise 4.44]]

The union of the two weighted graphs $X$ (on $A, B, C, D$) and $Y$ (on $x, y, z$) together with the bridges $B \xrightarrow{11} x$ and $D \xrightarrow{9} y$ as extra weighted edges.

> Sources: 7 Sketches, Exercise 4.44 and Solution A.4.

## Solution 4.48

[[7S Chapter 4 Exercises#Exercise 4.48|Exercise 4.48]]

Constituent (i) agrees (a unit element/object). For (ii), Definition 2.2 asks for a *function* $\otimes : P \times P \to P$, Definition 4.45 for a *functor*; functors between preorders are monotone maps, and monotonicity of $\otimes$ is exactly axiom (a). The natural isomorphisms (a)–(d) of Definition 4.45 become the equations/equivalences (b)–(d) of Definition 2.2 (unitality gives both unitors).

> Sources: 7 Sketches, Exercise 4.48 and Solution A.4.

## Solution 4.50

[[7S Chapter 4 Exercises#Exercise 4.50|Exercise 4.50]]

1. $g_E(5,3) = \mathsf{false}$, $g_F(5,3) = 2$. 2. $g_E(3,5) = \mathsf{true}$, $g_F(3,5) = -2$. 3. $h(5, \mathsf{true}) = 5$. 4. $h(-5, \mathsf{true}) = -5$. 5. $h(-5, \mathsf{false}) = 6$. 6. $q_G(-2, 3) = 2$, $q_F(-2,3) = -13$ (since $f_C = 2$, $f_D = -10$, $g_E(-10, 3) = \mathsf{true}$, $g_F = -13$, $h(2, \mathsf{true}) = 2$). 7. $q_G(2,3) = -1$, $q_F(2,3) = 7$ ($f_D = 10$, $g_E(10, 3) = \mathsf{false}$, $h(2, \mathsf{false}) = -1$).

> Sources: 7 Sketches, Exercise 4.50 and Solution A.4.

## Solution 4.52

[[7S Chapter 4 Exercises#Exercise 4.52|Exercise 4.52]]

Yes: objects and hom-sets agree; $\mathrm{id}_x : \{1\} \to \mathcal{C}(x,x)$ is an element of $\mathcal{C}(x,x)$; composition $\mathcal{C}(x,y) \times \mathcal{C}(y,z) \to \mathcal{C}(x,z)$ is the composite; "the usual associative and unital laws" are the two axioms. Categories are $\mathbf{Set}$-categories ([[Enriched Category]]).

> Sources: 7 Sketches, Exercise 4.52 and Solution A.4.

## Solution 4.54

[[7S Chapter 4 Exercises#Exercise 4.54|Exercise 4.54]]

A morphism $I \to X(x,x)$ in $\mathbf{Cost}$ is the condition $0 \geq d(x,x)$, hence $d(x,x) = 0$: the distance from a point to itself is zero.

> Sources: 7 Sketches, Exercise 4.54 and Solution A.4.

## Solution 4.62

[[7S Chapter 4 Exercises#Exercise 4.62|Exercise 4.62]]

Unit and counit are the same equivalence relation on $\underline{3} \sqcup \underline{3}$, pairing each $i$ in the first copy with $i$ in the second. Composing $\mathrm{id} \sqcup \eta$ with $\varepsilon \sqcup \mathrm{id}$ on $\underline{3} \sqcup \underline{3} \sqcup \underline{3}$: element $i$ of the first copy is linked to $i$ of the second (by $\varepsilon$) which is linked to $i$ of the third (by $\eta$), so after restricting to the outer copies we get the pairing $i \sim i$: the identity corelation. See [[Compact Closed Category]].

> Sources: 7 Sketches, Exercise 4.62 and Solution A.4.

## Solution 4.64

[[7S Chapter 4 Exercises#Exercise 4.64|Exercise 4.64]]

$X \times Y$ is the preorder of pairs of resources with $(x,y) \leq (x',y')$ iff $x$ is available given $x'$ and $y$ given $y'$. $\Phi \times \Psi$ is the conjunction: $(x_2, y_2)$ can be obtained given $(x_1, y_1)$ iff $x_2$ can be obtained given $x_1$ AND $y_2$ given $y_1$. See [[Category of Profunctors]].

> Sources: 7 Sketches, Exercise 4.64 and Solution A.4.

## Solution 4.65

#proof — [[7S Chapter 4 Exercises#Exercise 4.65|Exercise 4.65]]

$\alpha : X \times \mathbf{1} \nrightarrow X$, $\alpha((x,1), y) := X(x,y)$, with inverse $\alpha^{-1}(x, (y, 1)) := X(x, y)$. Then $(\alpha^{-1} \mathbin{;} \alpha)(x,z) = \bigvee_y X(x,y) \otimes X(y,z) = X(x,z) = U_X(x,z)$: $\geq$ by $X(x,z) \otimes I \leq X(x,z) \otimes X(z,z)$, $\leq$ by composition in $X$. Similarly $\alpha \mathbin{;} \alpha^{-1} = U_{X \times \mathbf{1}}$, and $\beta((1,x), y) := X(x,y)$ handles $\mathbf{1} \times X$.

> Sources: 7 Sketches, Exercise 4.65 and Solution A.4.

## Solution 4.66

#proof — [[7S Chapter 4 Exercises#Exercise 4.66|Exercise 4.66]]

The composite $X \xrightarrow{\alpha^{-1}} X \times \mathbf{1} \xrightarrow{U_X \times \eta_X} X \times X^{\mathrm{op}} \times X \xrightarrow{\varepsilon_X \times U_X} \mathbf{1} \times X \xrightarrow{\alpha} X$ has value at $(x, y)$ equal to $\bigvee_{a,b,c,d,e} X(x,a) \otimes X(a,b) \otimes X(c,d) \otimes X(b,c) \otimes X(d,e) \otimes X(e,y)$ (using distributivity), which collapses to $X(x,y)$ by repeatedly applying Lemma 4.27 (composing with the unit profunctor is the identity). So the composite is $U_X$; the other snake equation is analogous. See [[Compact Closed Category]].

> Sources: 7 Sketches, Exercise 4.66 and Solution A.4.
