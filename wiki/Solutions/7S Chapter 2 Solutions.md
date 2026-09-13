#solution

Solutions to the exercises of 7 Sketches, Chapter 2: [[7S Chapter 2 Exercises]]. Index: [[Map of Content]].

## Solution 2.5

[[7S Chapter 2 Exercises#Exercise 2.5|Exercise 2.5]]

Monotonicity (a) fails: $-1 \leq -1$ and $0 \leq 1$, but $(-1) \ast 0 = 0 \not\leq -1 = (-1) \ast 1$. (On $\mathbb{R}_{\geq 0}$ it would work.)

> Sources: 7 Sketches, Exercise 2.5 and Solution A.2.

## Solution 2.8

[[7S Chapter 2 Exercises#Exercise 2.8|Exercise 2.8]]

Yes. Condition (a) says $x_1 = y_1, x_2 = y_2 \Rightarrow x_1 \ast x_2 = y_1 \ast y_2$, a tautology; (b) and (c) are the monoid equations; (d) is commutativity.

> Sources: 7 Sketches, Exercise 2.8 and Solution A.2.

## Solution 2.20

#proof — [[7S Chapter 2 Exercises#Exercise 2.20|Exercise 2.20]]

1.

$$
\begin{aligned} t + u &\leq (v + w) + u && \text{(monotonicity: } t \leq v + w,\ u \leq u) \\ &= v + (w + u) && \text{(associativity)} \\ &\leq v + (x + z) && \text{(monotonicity: } v \leq v,\ w + u \leq x + z) \\ &= (v + x) + z && \text{(associativity)} \\ &\leq y + z && \text{(monotonicity: } v + x \leq y,\ z \leq z). \end{aligned}
$$

2. Reflexivity gives $u \leq u$, $v \leq v$, $z \leq z$; transitivity chains the inequalities into $t + u \leq y + z$.
3. No wires cross in the diagram, so symmetry is never invoked.

> Sources: 7 Sketches, Exercise 2.20 and Solution A.2.

## Solution 2.21

[[7S Chapter 2 Exercises#Exercise 2.21|Exercise 2.21]]

(a) If $x \to y$ and $z \to w$ are reactions then $x + z \to y + w$ is one. (b) Adding no material changes nothing. (c) Combining three collections is independent of bracketing. (d) Combining $x$ with $y$ is the same as $y$ with $x$. So it is a [[Symmetric Monoidal Preorder]].

> Sources: 7 Sketches, Exercise 2.21 and Solution A.2.

## Solution 2.29

[[7S Chapter 2 Exercises#Exercise 2.29|Exercise 2.29]]

The unit must be $\mathsf{false}$ ($\mathsf{false} \vee x = x$). The remaining conditions hold by checking all cases; with $\mathsf{false} = 0$, $\mathsf{true} = 1$, $\vee$ is $\max$. See [[Bool (Monoidal Preorder)]] — this second structure is not closed.

> Sources: 7 Sketches, Exercise 2.29 and Solution A.2.

## Solution 2.31

[[7S Chapter 2 Exercises#Exercise 2.31|Exercise 2.31]]

The unit is $1$. Multiplication of naturals is monotone ($x_1 \leq y_1, x_2 \leq y_2 \Rightarrow x_1 x_2 \leq y_1 y_2$), associative, unital, and commutative.

> Sources: 7 Sketches, Exercise 2.31 and Solution A.2.

## Solution 2.33

[[7S Chapter 2 Exercises#Exercise 2.33|Exercise 2.33]]

No: monotonicity fails. $1 \mid 1$ and $1 \mid 2$, but $1 + 1 = 2 \nmid 3 = 1 + 2$.

> Sources: 7 Sketches, Exercise 2.33 and Solution A.2.

## Solution 2.34

[[7S Chapter 2 Exercises#Exercise 2.34|Exercise 2.34]]

1. $\min$ takes the smaller element:

| $\min$ | no | maybe | yes |
|---|---|---|---|
| no | no | no | no |
| maybe | no | maybe | maybe |
| yes | no | maybe | yes |

2. (a) $x \leq y, z \leq w \Rightarrow \min(x,z) \leq \min(y,w)$; (b) $\min(x, \mathsf{yes}) = x$; (c), (d) associativity and commutativity of $\min$ — all by checking cases. So $\mathbf{NMY} = (P, \leq, \mathsf{yes}, \min)$ is a [[Symmetric Monoidal Preorder]]. $\mathbf{NMY}$-categories are interpreted in [[7S Chapter 2 Exercises#Exercise 2.61|7S Exercise 2.61]].

> Sources: 7 Sketches, Exercise 2.34 and Solution A.2.

## Solution 2.35

[[7S Chapter 2 Exercises#Exercise 2.35|Exercise 2.35]]

Yes: $\cap$ is monotone with respect to $\subseteq$, $S \cap A = A$, and $\cap$ is associative and commutative. It is in fact a [[Quantale]] ([[7S Chapter 2 Exercises#Exercise 2.94|7S Exercise 2.94]]).

> Sources: 7 Sketches, Exercise 2.35 and Solution A.2.

## Solution 2.36

[[7S Chapter 2 Exercises#Exercise 2.36|Exercise 2.36]]

Take unit $\mathsf{true}$ ("$n$ is a natural number") and product $\wedge$: $(P \wedge Q)(n)$ true iff both are. Alternatively unit $\mathsf{false}$ ("$n$ is made of cheese") and product $\vee$. Both give [[Symmetric Monoidal Preorder|symmetric monoidal preorders]] (compare [[Closure Operator|modal operators]] in Example 1.123).

> Sources: 7 Sketches, Exercise 2.36 and Solution A.2.

## Solution 2.39

#proof — [[7S Chapter 2 Exercises#Exercise 2.39|Exercise 2.39]]

Unitality and associativity are equations not involving the order, so they transfer. Symmetry asks $x \otimes y \cong y \otimes x$: in $X$ this means $x \otimes y \leq y \otimes x$ and $y \otimes x \leq x \otimes y$, which in $X^{\mathrm{op}}$ read $y \otimes x \leq x \otimes y$ and $x \otimes y \leq y \otimes x$ — the same two facts.

> Sources: 7 Sketches, Exercise 2.39 and Solution A.2.

## Solution 2.40

[[7S Chapter 2 Exercises#Exercise 2.40|Exercise 2.40]]

$([0, \infty], \leq)$ with the usual increasing order; unit $0$; product $+$. See [[Cost]].

> Sources: 7 Sketches, Exercise 2.40 and Solution A.2.

## Solution 2.43

[[7S Chapter 2 Exercises#Exercise 2.43|Exercise 2.43]]

Monotone: $\mathsf{false} \leq \mathsf{true}$ and $\infty \geq 0$. (a): $0 \geq 0 = g(\mathsf{true})$. (b): $g(a) + g(b) \geq g(a \wedge b)$ in all four cases ($\infty + \infty \geq \infty$, $\infty + 0 \geq \infty$, $0 + 0 \geq 0$). All are equalities, so $g$ is strict.

> Sources: 7 Sketches, Exercise 2.43 and Solution A.2.

## Solution 2.44

[[7S Chapter 2 Exercises#Exercise 2.44|Exercise 2.44]]

Yes to everything: both are strict [[Monoidal Monotone Map|monoidal monotones]] $\mathbf{Cost} \to \mathbf{Bool}$. $d$ asks "is $x = 0$?": $0$ is $0$, and a sum is $0$ iff both summands are. $u$ asks "is $x$ finite?": $0$ is finite, and a sum is finite iff both summands are. They give two different [[Change of Base|changes of base]] from [[Lawvere Metric Space|metric spaces]] to preorders ([[7S Chapter 2 Exercises#Exercise 2.68|7S Exercise 2.68]]).

> Sources: 7 Sketches, Exercise 2.44 and Solution A.2.

## Solution 2.45

[[7S Chapter 2 Exercises#Exercise 2.45|Exercise 2.45]]

1. Yes ([[7S Chapter 2 Exercises#Exercise 2.31|7S Exercise 2.31]]). 2. Yes: $f(n) = 1$ for all $n$ (in fact it is the unique one: $f(0) \geq 1$ forces $f(0) = 1$... and $f(n) \cdot f(m) \leq f(n + m)$ with monotonicity pins everything to $1$). 3. No: $\ast$ is not monotone on $\mathbb{Z}$, e.g. $-1 \leq 0$ but $(-1)(-1) = 1 \not\leq 0 = 0 \cdot 0$.

> Sources: 7 Sketches, Exercise 2.45 and Solution A.2.

## Solution 2.50

#proof — [[7S Chapter 2 Exercises#Exercise 2.50|Exercise 2.50]]

1. From $(P, \leq)$ build $\mathcal{X}_P$ with $\mathcal{X}_P(p, q) = \mathsf{true}$ iff $p \leq q$; the preorder recovered has $p \leq q$ iff $\mathcal{X}_P(p,q) = \mathsf{true}$ iff $p \leq q$ — the original.
2. From a $\mathbf{Bool}$-category $\mathcal{X}$ build the preorder $x \leq y$ iff $\mathcal{X}(x,y) = \mathsf{true}$, then the $\mathbf{Bool}$-category $\mathcal{X}'$ with $\mathcal{X}'(x,y) = \mathsf{true}$ iff $x \leq y$ iff $\mathcal{X}(x,y) = \mathsf{true}$. So $\mathcal{X}' = \mathcal{X}$.

> Sources: 7 Sketches, Exercise 2.50 and Solution A.2.

## Solution 2.52

[[7S Chapter 2 Exercises#Exercise 2.52|Exercise 2.52]]

$d(\mathrm{US}, \mathrm{Spain})$: from San Diego to anywhere in Spain is farther than from anywhere in Spain to New York. See [[Hausdorff Distance]], [[Metric Space]].

> Sources: 7 Sketches, Exercise 2.52 and Solution A.2.

## Solution 2.55

[[7S Chapter 2 Exercises#Exercise 2.55|Exercise 2.55]]

The latter forbids infinite distances: a "finite-distance Lawvere metric space".

> Sources: 7 Sketches, Exercise 2.55 and Solution A.2.

## Solution 2.58

[[7S Chapter 2 Exercises#Exercise 2.58|Exercise 2.58]]

| $d_X$ | A | B | C | D |
|---|---|---|---|---|
| A | 0 | 6 | 3 | 11 |
| B | 2 | 0 | 5 | 5 |
| C | 5 | 3 | 0 | 8 |
| D | 11 | 9 | 6 | 0 |

E.g. $d(A, B) = 3 + 3$ via $C$; $d(A, D) = 3 + 3 + 5$.

> Sources: 7 Sketches, Exercise 2.58 and Solution A.2.

## Solution 2.60

[[7S Chapter 2 Exercises#Exercise 2.60|Exercise 2.60]]

$$M_X = \begin{pmatrix} 0 & \infty & 3 & \infty \\ 2 & 0 & \infty & 5 \\ \infty & 3 & 0 & \infty \\ \infty & \infty & 6 & 0 \end{pmatrix}$$ (rows/columns $A, B, C, D$). Its powers give $d_X$ ([[7S Chapter 2 Exercises#Exercise 2.105|7S Exercise 2.105]]).

> Sources: 7 Sketches, Exercise 2.60 and Solution A.2.

## Solution 2.61

[[7S Chapter 2 Exercises#Exercise 2.61|Exercise 2.61]]

A set of points with, for each pair $(x, y)$, a value $\mathcal{X}(x,y) \in \{\mathsf{no}, \mathsf{maybe}, \mathsf{yes}\}$ — whether it is possible to get from $x$ to $y$ — such that $\mathcal{X}(x,x) = \mathsf{yes}$ and $\min(\mathcal{X}(x,y), \mathcal{X}(y,z)) \leq \mathcal{X}(x,z)$: it is at least as possible to go $x \to z$ directly as via $y$.

> Sources: 7 Sketches, Exercise 2.61 and Solution A.2.

## Solution 2.62

[[7S Chapter 2 Exercises#Exercise 2.62|Exercise 2.62]]

1. $A \xrightarrow{\{\mathsf{boat}\}} B$, $B \xrightarrow{\{\mathsf{boat}\}} D$, $C \xrightarrow{\{\mathsf{foot},\mathsf{boat}\}} A$, $C \xrightarrow{\{\mathsf{foot},\mathsf{car}\}} D$, $D \xrightarrow{\{\mathsf{foot},\mathsf{car}\}} C$ (say).
2. E.g. $\mathcal{X}(C, D)$: paths $C \to A \to B \to D$ (intersection $\{\mathsf{boat}\}$) and $C \to D$ ($\{\mathsf{foot},\mathsf{car}\}$); union $= M$. Diagonal entries are $M$. Taking the union over all paths guarantees $\mathcal{X}(x,y) \cap \mathcal{X}(y,z) \subseteq \mathcal{X}(x,z)$: it is a [[Weighted Graph|presented $\mathcal{V}$-category]].
3. Yes, the interpretation looks right.

> Sources: 7 Sketches, Exercise 2.62 and Solution A.2.

## Solution 2.63

[[7S Chapter 2 Exercises#Exercise 2.63|Exercise 2.63]]

Graph $A \xrightarrow{5} B$, $A \xrightarrow{10} C$, $B \xrightarrow{6} C$, $B \xrightarrow{10} A$, $C \xrightarrow{10} B$ gives

$$
\begin{pmatrix} \infty & 6 & 10 \\ 10 & \infty & 10 \\ 10 & 6 & \infty \end{pmatrix}.
$$

Diagonals equal the unit $\infty$ and $\min(M(x,y), M(y,z)) \leq M(x,z)$, so it is a $\mathbf{W}$-category. Interpretation: **weight limits** for trucking cargo — the hom-object is the maximum cargo weight allowed from $x$ to $y$; staying put has no limit; the limit $x \to z$ is at least $\min$ of the limits via $y$ (a "bottleneck" or max-min path problem).

> Sources: 7 Sketches, Exercise 2.63 and Solution A.2.

## Solution 2.67

[[7S Chapter 2 Exercises#Exercise 2.67|Exercise 2.67]]

$\mathrm{Boston} \leq \mathrm{US}$, and Spain is only related to itself: the "is a part of" relation, since $x \leq y$ iff $d(x,y) = 0$ iff every point of $x$ is (at distance $0$ from) a point of $y$.

> Sources: 7 Sketches, Exercise 2.67 and Solution A.2.

## Solution 2.68

[[7S Chapter 2 Exercises#Exercise 2.68|Exercise 2.68]]

1. $u(x) = [x < \infty]$ from [[7S Chapter 2 Exercises#Exercise 2.44|7S Exercise 2.44]].
2. Two points $A, B$ with $d(A,B) = d(B,A) = 5$: $\mathcal{X}_f$ is the [[Discrete Preorder]] on $\{A, B\}$, while $\mathcal{X}_u$ is the [[Codiscrete Preorder]] ($A \leq B \leq A$).

> Sources: 7 Sketches, Exercise 2.68 and Solution A.2.

## Solution 2.73

#proof — [[7S Chapter 2 Exercises#Exercise 2.73|Exercise 2.73]]

1. Dagger: the identity is a $\mathbf{Cost}$-functor $\mathcal{X} \to \mathcal{X}^{\mathrm{op}}$, so $d(x,y) \geq d(y,x)$ for all $x,y$, hence by symmetry of the quantifier $d(x,y) = d(y,x)$ — property (c). Skeletal: $0 \geq d(x,y)$ and $0 \geq d(y,x)$ imply $x = y$; given (c) this is property (b). So skeletal dagger $\mathbf{Cost}$-categories are exactly extended metric spaces.
2. By [[7S Chapter 1 Exercises#Exercise 1.73|7S Exercise 1.73]], skeletal dagger $\mathbf{Bool}$-categories (preorders) are sets. So in both cases "skeletal dagger" turns the enriched notion into the classical one.

> Sources: 7 Sketches, Exercise 2.73 and Solution A.2.

## Solution 2.75

#proof — [[7S Chapter 2 Exercises#Exercise 2.75|Exercise 2.75]]

1. $I = I \otimes I \leq \mathcal{X}(x,x) \otimes \mathcal{Y}(y,y) = (\mathcal{X} \times \mathcal{Y})((x,y),(x,y))$.
2. $\mathcal{X}(x_1,x_2) \otimes \mathcal{Y}(y_1,y_2) \otimes \mathcal{X}(x_2,x_3) \otimes \mathcal{Y}(y_2,y_3) \cong \mathcal{X}(x_1,x_2) \otimes \mathcal{X}(x_2,x_3) \otimes \mathcal{Y}(y_1,y_2) \otimes \mathcal{Y}(y_2,y_3) \leq \mathcal{X}(x_1,x_3) \otimes \mathcal{Y}(y_1,y_3)$ by monotonicity.
3. Symmetry is used to swap $\mathcal{Y}(y_1,y_2) \otimes \mathcal{X}(x_2,x_3) \cong \mathcal{X}(x_2,x_3) \otimes \mathcal{Y}(y_1,y_2)$.

> Sources: 7 Sketches, Exercise 2.75 and Solution A.2.

## Solution 2.78

[[7S Chapter 2 Exercises#Exercise 2.78|Exercise 2.78]]

$d((5,6),(-1,4)) = |{-1} - 5| + |4 - 6| = 6 + 2 = 8$ — the Manhattan distance, not $\sqrt{40}$. See [[Product of Enriched Categories]].

> Sources: 7 Sketches, Exercise 2.78 and Solution A.2.

## Solution 2.82

#proof — [[7S Chapter 2 Exercises#Exercise 2.82|Exercise 2.82]]

1. If $u \leq u'$ then $u \otimes v \leq u' \otimes v$ by monotonicity (a) with $v \leq v$.
2. Put $a := v \multimap w$ in (2.80): the right side $(v \multimap w) \leq (v \multimap w)$ holds by reflexivity, so $(v \multimap w) \otimes v \leq w$.
3. If $u \leq u'$ then $(v \multimap u) \otimes v \leq u \leq u'$, so by (2.80) $(v \multimap u) \leq (v \multimap u')$.
4. (2.80) is exactly the [[Galois Connection]] condition for $(- \otimes v) \dashv (v \multimap -)$, and 1, 3 supply the required monotonicity.

> Sources: 7 Sketches, Exercise 2.82 and Solution A.2.

## Solution 2.84

#proof — [[7S Chapter 2 Exercises#Exercise 2.84|Exercise 2.84]]

Define $v \Rightarrow w$ by: $\mathsf{false} \Rightarrow w = \mathsf{true}$, $\mathsf{true} \Rightarrow w = w$. Then $(a \wedge v) \leq w$ iff $a \leq (v \Rightarrow w)$: if $v = \mathsf{false}$ both sides are always true; if $v = \mathsf{true}$ both sides say $a \leq w$.

> Sources: 7 Sketches, Exercise 2.84 and Solution A.2.

## Solution 2.92

[[7S Chapter 2 Exercises#Exercise 2.92|Exercise 2.92]]

1a. $\mathsf{false}$, the least element. 1b. $\infty$: because [[Cost]] uses the reversed order $\geq$, $\infty$ is the least element — so the "$0$" of Definition 2.90 is $\infty$ here; beware.
2a. OR. 2b. $\min(x, y)$, the greatest number $\leq$ both under the usual order.

> Sources: 7 Sketches, Exercise 2.92 and Solution A.2.

## Solution 2.93

[[7S Chapter 2 Exercises#Exercise 2.93|Exercise 2.93]]

It is closed ([[7S Chapter 2 Exercises#Exercise 2.84|7S Exercise 2.84]]) and has all joins, given by OR ([[7S Chapter 1 Exercises#Exercise 1.7|7S Exercise 1.7]], Example 1.88; the empty join is $\mathsf{false}$).

> Sources: 7 Sketches, Exercise 2.93 and Solution A.2.

## Solution 2.94

#proof — [[7S Chapter 2 Exercises#Exercise 2.94|Exercise 2.94]]

Yes. Joins are unions. The hom-element is $B \multimap C = \overline{B} \cup C$: if $A \cap B \subseteq C$ then $A = (A \cap B) \cup (A \cap \overline{B}) \subseteq \overline{B} \cup C$; conversely if $A \subseteq \overline{B} \cup C$ then $A \cap B \subseteq (\overline{B} \cup C) \cap B = C \cap B \subseteq C$. (This is the [[Topos|Boolean/Heyting algebra]] structure of the power set.)

> Sources: 7 Sketches, Exercise 2.94 and Solution A.2.

## Solution 2.103

[[7S Chapter 2 Exercises#Exercise 2.103|Exercise 2.103]]

$\begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix}$, $\begin{pmatrix} \mathsf{true} & \mathsf{false} \\ \mathsf{false} & \mathsf{true} \end{pmatrix}$, $\begin{pmatrix} 0 & \infty \\ \infty & 0 \end{pmatrix}$ — unit on the diagonal, $\bigvee\varnothing$ elsewhere.

> Sources: 7 Sketches, Exercise 2.103 and Solution A.2.

## Solution 2.104

#proof — [[7S Chapter 2 Exercises#Exercise 2.104|Exercise 2.104]]

First, $0 \otimes v = v \otimes \bigvee \varnothing = \bigvee_{a \in \varnothing} v \otimes a = 0$ by Proposition 2.87(b) and symmetry.
1. $(I_X \ast M)(x, y) = \bigvee_{x'} I_X(x,x') \otimes M(x', y) = (I \otimes M(x,y)) \vee \bigvee_{x' \neq x} (0 \otimes M(x',y)) = M(x,y) \vee 0 = M(x,y)$.
2. $((M \ast N) \ast P)(w,z) = \bigvee_y \big(\bigvee_x M(w,x) \otimes N(x,y)\big) \otimes P(y,z) = \bigvee_{x,y} M(w,x) \otimes N(x,y) \otimes P(y,z) = \bigvee_x M(w,x) \otimes \big(\bigvee_y N(x,y) \otimes P(y,z)\big) = (M \ast (N \ast P))(w,z)$, using distributivity of $\otimes$ over joins and associativity of $\otimes$.

> Sources: 7 Sketches, Exercise 2.104 and Solution A.2.

## Solution 2.105

[[7S Chapter 2 Exercises#Exercise 2.105|Exercise 2.105]]

$$
M_X = \begin{pmatrix} 0 & \infty & 3 & \infty \\ 2 & 0 & \infty & 5 \\ \infty & 3 & 0 & \infty \\ \infty & \infty & 6 & 0 \end{pmatrix},\quad M_X^2 = \begin{pmatrix} 0 & 6 & 3 & \infty \\ 2 & 0 & 5 & 5 \\ 5 & 3 & 0 & 8 \\ \infty & 9 & 6 & 0 \end{pmatrix},\quad M_X^3 = M_X^4 = \begin{pmatrix} 0 & 6 & 3 & 11 \\ 2 & 0 & 5 & 5 \\ 5 & 3 & 0 & 8 \\ 11 & 9 & 6 & 0 \end{pmatrix} = d_X.
$$

The powers stabilize at the distance matrix ([[Matrix Multiplication in a Quantale]]).

> Sources: 7 Sketches, Exercise 2.105 and Solution A.2.
