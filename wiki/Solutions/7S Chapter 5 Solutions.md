#solution

Solutions to the exercises of 7 Sketches, Chapter 5: [[7S Chapter 5 Exercises]]. Index: [[Map of Content]].

## Solution 5.5

[[7S Chapter 5 Exercises#Exercise 5.5|Exercise 5.5]]

1–2. Any two functions; $f + g : 5 \to 6$ acts as $f$ on the first three elements and as $g$, shifted by $2$, on the last two (Eq. 5.4). 3. $(f \mathbin{;} g)(i) = g(f(i))$. 4. $\mathrm{id}_m(i) = i$: $m$ parallel wires. 5. $\sigma_{m,n}(i) = i + n$ for $i \leq m$ and $i - m$ for $i > m$: the block swap (e.g. $\sigma_{3,5} : 8 \to 8$).

> Sources: 7 Sketches, Exercise 5.5 and Solution A.5.

## Solution 5.9

[[7S Chapter 5 Exercises#Exercise 5.9|Exercise 5.9]]

The discrete order ($m \preceq n$ iff $m = n$); the usual order; the reverse of the usual order. Quasi-example: the codiscrete preorder (not a poset). Non-example: [[Divisibility Order|divisibility]] — $2 \mid 4$ and $3 \mid 3$ but $5 \nmid 7$, so $+$ is not monotone.

> Sources: 7 Sketches, Exercise 5.9 and Solution A.5.

## Solution 5.10

[[7S Chapter 5 Exercises#Exercise 5.10|Exercise 5.10]]

$\mathbf{Bij}$: $\mathbf{Bij}(m,n)$ is empty unless $m = n$ (then $n!$ elements); identity $i \mapsto i$; $\sigma_{m,n}(i) = i + n$ or $i - m$; composition of bijections; $(f + g)(i) = f(i)$ for $i \leq m$, $g(i - m)$ otherwise. $\mathbf{Corel}$: equivalence relations on $\underline{m} \sqcup \underline{n}$; identity the pairing $i \sim i'$; symmetry pairs corresponding elements; composition "travel within classes" ([[Corelation]]); $\sim + \sim'$ acts on each half with no interaction. $\mathbf{Rel}$: subsets $R \subseteq \underline{m} \times \underline{n}$; identity the diagonal; symmetry the swap relation; relational composition; $R_1 + R_2 = R_1 \sqcup R_2$ ([[Category of Relations]]).

> Sources: 7 Sketches, Exercise 5.10 and Solution A.5.

## Solution 5.16

[[7S Chapter 5 Exercises#Exercise 5.16|Exercise 5.16]]

Stick the two port graphs end to end, connecting the $n$ outer outputs of the first to the $n$ outer inputs of the second in order, remove the two outer boxes and draw a new outer box around everything. E.g. composing a graph with boxes $a, b, c$ and one with boxes $d, e$ yields one with boxes $a, b, c, d, e$ wired in sequence.

> Sources: 7 Sketches, Exercise 5.16 and Solution A.5.

## Solution 5.18

[[7S Chapter 5 Exercises#Exercise 5.18|Exercise 5.18]]

Stack the picture on top of itself: a $(4, 6)$-port graph with boxes $a, b, c, a', b', c'$ and no wires between the two copies. See [[Port Graph]].

> Sources: 7 Sketches, Exercise 5.18 and Solution A.5.

## Solution 5.20

#proof — [[7S Chapter 5 Exercises#Exercise 5.20|Exercise 5.20]]

1. $x \leq_P y$ iff there is a chain $x = x_0, \dots, x_n = y$ with $R(x_i, x_{i+1})$ ($n = 0$ gives reflexivity); by assumption $f(x_i) \leq f(x_{i+1})$, so by induction and transitivity $f(x) \leq f(y)$.
2. $R(x,y)$ implies $x \leq_P y$ (the closure contains $R$), hence $f(x) \leq f(y)$.
This is the universal property of the [[Reflexive Transitive Closure|free preorder]] — about maps *out* ([[Free Prop]]).

> Sources: 7 Sketches, Exercise 5.20 and Solution A.5.

## Solution 5.21

[[7S Chapter 5 Exercises#Exercise 5.21|Exercise 5.21]]

1. Yes, since $R(x,y)$ implies $x \leq_P y$. 2. No: $Q = \{1\}$, $P = \{1\}$ with $R = \varnothing$; $g$ is monotone ($\leq_P$ is reflexive) but $(g(1), g(1)) \notin R$. "Maps between structured objects preserve constraints, so the domain must be more constrained than the codomain: fewest constraints = most maps out."

> Sources: 7 Sketches, Exercise 5.21 and Solution A.5.

## Solution 5.23

#proof — [[7S Chapter 5 Exercises#Exercise 5.23|Exercise 5.23]]

1. Each morphism $q : y \to z$ has a domain $y$ and codomain $z$. 2. A functor restricts to $(f, g)$ on vertices and length-1 paths; conversely $(f, g)$ extends to paths by $F(v_0, a_1, \dots, a_n) := \mathrm{id}_{f(v_0)} \mathbin{;} g(a_1) \mathbin{;} \cdots \mathbin{;} g(a_n)$, and the two constructions are inverse (functoriality forces the action on all paths). 3. Yes, it is the underlying graph $U(\mathcal{C})$; part 2 says $\mathrm{Free} \dashv U : \mathbf{Grph} \rightleftarrows \mathbf{Cat}$ is an [[Adjunction]] ([[Free Category]]).

> Sources: 7 Sketches, Exercise 5.23 and Solution A.5.

## Solution 5.24

[[7S Chapter 5 Exercises#Exercise 5.24|Exercise 5.24]]

1. $a^0, a^1, a^2, \dots$ with $a^i \ast a^j = a^{i+j}$. 2. $(\mathbb{N}, +, 0)$ via $a^i \mapsto i$. 3. Words in $a$ and $b$: $[\,], [a], [b], [a,a], [a,b], \dots$

> Sources: 7 Sketches, Exercise 5.24 and Solution A.5.

## Solution 5.28

#proof — [[7S Chapter 5 Exercises#Exercise 5.28|Exercise 5.28]]

Both have objects $\mathbb{N}$. A morphism of $\mathrm{Free}(G)$ is a $G$-labeled port graph; since $G$ has exactly one generator of each arity, the labeling $\ell$ is forced ($\ell(v) = \rho_{\mathrm{in}(v), \mathrm{out}(v)}$) and contributes nothing, so morphisms are exactly port graphs; composition and monoidal product are by definition those of $\mathbf{PG}$.

> Sources: 7 Sketches, Exercise 5.28 and Solution A.5.

## Solution 5.32

[[7S Chapter 5 Exercises#Exercise 5.32|Exercise 5.32]]

Three input wires; the top passes through box $f$; then wires 1 and 2 cross; then wires 2 and 3 enter $h$; the two remaining wires cross; finally both enter $g$, giving two outputs. See [[Free Prop]] (prop expressions).

> Sources: 7 Sketches, Exercise 5.32 and Solution A.5.

## Solution 5.35

[[7S Chapter 5 Exercises#Exercise 5.35|Exercise 5.35]]

For all intents and purposes yes; the only "subtle difference" is that between a set and its quotient by the trivial equivalence relation (elements vs. singleton classes), which are naturally isomorphic — "category-theoretically the difference will never make a difference".

> Sources: 7 Sketches, Exercise 5.35 and Solution A.5.

## Solution 5.41

[[7S Chapter 5 Exercises#Exercise 5.41|Exercise 5.41]]

1. The identity matrix ($1$ on the diagonal, $0$ elsewhere). 2. $n = 2$: $A = \begin{pmatrix}0&1\\0&0\end{pmatrix}$, $B = \begin{pmatrix}0&1\\1&0\end{pmatrix}$ give $(AB)(1,1) = 1$ but $(BA)(1,1) = 0$.

> Sources: 7 Sketches, Exercise 5.41 and Solution A.5.

## Solution 5.43

[[7S Chapter 5 Exercises#Exercise 5.43|Exercise 5.43]]

$(16x + 4y,\ x + 4y)$ — by tracing signals or summing over paths.

> Sources: 7 Sketches, Exercise 5.43 and Solution A.5.

## Solution 5.51

[[7S Chapter 5 Exercises#Exercise 5.51|Exercise 5.51]]

$$
A + B = \begin{pmatrix} 3&3&1&0&0&0&0 \\ 2&0&4&0&0&0&0 \\ 0&0&0&2&5&6&1 \end{pmatrix}.
$$

See [[Prop of Matrices]].

> Sources: 7 Sketches, Exercise 5.51 and Solution A.5.

## Solution 5.55

[[7S Chapter 5 Exercises#Exercise 5.55|Exercise 5.55]]

Both represent $(1\ 1\ 1) : 1 \to 3$; yes, equal — coassociativity of copy ([[Graphical Linear Algebra]]).

> Sources: 7 Sketches, Exercise 5.55 and Solution A.5.

## Solution 5.58

[[7S Chapter 5 Exercises#Exercise 5.58|Exercise 5.58]]

1. Three inputs: discard the first, pass the second, amplify the third by 2, add all into one output (or, minimally: discard input 1, amplify input 3 by 2, add). 2. Discard both inputs; two zero outputs. 3. Copy each of the two inputs three times, amplify by $1, 2, 3$ resp. $4, 5, 6$, permute, and add pairwise into three outputs (the four-layer normal form of [[Prop of Matrices|Proposition 5.56]]).

> Sources: 7 Sketches, Exercise 5.58 and Solution A.5.

## Solution 5.59

#proof — [[7S Chapter 5 Exercises#Exercise 5.59|Exercise 5.59]]

Layer 1: $g_1 := c_n + \cdots + c_n : m \to mn$ where $c_n : 1 \to n$ makes $n$ copies (composite of copies with identities). Layer 2: $g_2 := \sum_{i,j} s_{M(i,j)} : mn \to mn$, scalars in row-major order. Layer 3: $g_3$, a permutation of swaps and identities sending the $(i-1)n + j$-th wire to the $(j-1)m + i$-th. Layer 4: $g_4 := a_m + \cdots + a_m : mn \to n$ where $a_m : m \to 1$ adds $m$ inputs. By Proposition 5.54 there is exactly one path from input $i$ to output $j$, carrying scalar $M(i,j)$, so $S(g_1 \mathbin{;} g_2 \mathbin{;} g_3 \mathbin{;} g_4) = M$.

> Sources: 7 Sketches, Exercise 5.59 and Solution A.5.

## Solution 5.62

[[7S Chapter 5 Exercises#Exercise 5.62|Exercise 5.62]]

E.g. for $\binom{0}{1}{2}$: "discard input 1, add inputs 2 and (input 3 amplified by 2)" vs. the normal form with a zero scalar; rewrite using "$0$ = discard-then-zero" and "zero into add = identity". For the zero matrix: two discards and two zeros vs. scalars $0$: use $0$ = discard-then-zero and the bialgebra laws. For the $2 \times 3$ matrix: two normal forms differing by the order of copying and adding, related by coassociativity/associativity and the bialgebra law. See [[Graphical Linear Algebra]].

> Sources: 7 Sketches, Exercise 5.62 and Solution A.5.

## Solution 5.63

#proof — [[7S Chapter 5 Exercises#Exercise 5.63|Exercise 5.63]]

1. One graph has a path from an input to an output that the other lacks. The only equation of Theorem 5.60 that breaks a left-to-right path is "$0$ = discard-then-zero", which requires a $0$ scalar; no $0$ appears and products/sums of nonzero naturals are nonzero, so the path cannot be removed. 2. Replacing $3$ by $0$, the scalars become $0$ = discard-then-zero, and the diagram simplifies (using the bialgebra and unit laws) to a graph with the surviving $5$-amplification only.

> Sources: 7 Sketches, Exercise 5.63 and Solution A.5.

## Solution 5.67

[[7S Chapter 5 Exercises#Exercise 5.67|Exercise 5.67]]

Check (a) $(a \ast b) \ast c = a \ast (b \ast c)$, (b) $1 \ast a = a = a \ast 1$, (c) $\sigma \mathbin{;} \mu = \mu$ i.e. $b \ast a = a \ast b$; identically for $+$ with $0$. Diagrammatically, both paths around each square agree.

> Sources: 7 Sketches, Exercise 5.67 and Solution A.5.

## Solution 5.69

[[7S Chapter 5 Exercises#Exercise 5.69|Exercise 5.69]]

1. $R^0 \cong \{1\}$ and $R^m \times R^n \cong R^{m+n}$ canonically. 2. A [[Monoidal Functor]] preserves the monoid diagrams: $U(\eta)(1) = (0, \dots, 0)$, $U(\mu)(a, b) = a + b$ componentwise. 3. The additive one: $(5, 3) \mapsto 8$.

> Sources: 7 Sketches, Exercise 5.69 and Solution A.5.

## Solution 5.77

[[7S Chapter 5 Exercises#Exercise 5.77|Exercise 5.77]]

$B(\text{add}^{\mathrm{op}}) = \{(x, (y, z)) \mid x = y + z\}$; $B(\text{copy}^{\mathrm{op}}) = \{((y, z), x) \mid x = y = z\}$. See [[Behavior of a Signal Flow Graph]].

> Sources: 7 Sketches, Exercise 5.77 and Solution A.5.

## Solution 5.80

[[7S Chapter 5 Exercises#Exercise 5.80|Exercise 5.80]]

$B + C := \{(w, y, x, z) \in R^{m+p} \times R^{n+q} \mid (w, x) \in B,\ (y, z) \in C\}$. See [[Category of Relations]].

> Sources: 7 Sketches, Exercise 5.80 and Solution A.5.

## Solution 5.82

#proof — [[7S Chapter 5 Exercises#Exercise 5.82|Exercise 5.82]]

$B(g) = \{(x, z) \mid S(g)x = z\}$, $B(h^{\mathrm{op}}) = \{(z, y) \mid z = S(h)y\}$; their composite is $\{(x,y) \mid \exists z.\ S(g)x = z = S(h)y\}$, and since $S(g), S(h)$ are functions this is $\{(x,y) \mid S(g)x = S(h)y\}$.

> Sources: 7 Sketches, Exercise 5.82 and Solution A.5.

## Solution 5.83

#proof — [[7S Chapter 5 Exercises#Exercise 5.83|Exercise 5.83]]

$B(g^{\mathrm{op}}) = \{(y, x) \mid y = S(g)x\}$, $B(h) = \{(x, z) \mid S(h)x = z\}$; composing over the middle $x$ gives $\{(S(g)x, S(h)x)\}$.

> Sources: 7 Sketches, Exercise 5.83 and Solution A.5.

## Solution 5.84

#proof — [[7S Chapter 5 Exercises#Exercise 5.84|Exercise 5.84]]

1. The reversed zero has behaviour $\{y \mid y = 0\}$; its $n$-fold sum is $\{0\} \subseteq R^n$; composing with $B(g)$ gives $\{x \mid S(g)x = 0\}$. 2. Reversed discard has behaviour all of $R$; composing $R^m$ with $B(g)$ gives $\{y \mid \exists x.\ S(g)x = y\}$. 3. $S(g)$ is linear, so $B(g)$ is closed under scalars and sums; likewise $B(g^{\mathrm{op}})$; and composites of linear relations are linear ([[7S Chapter 5 Exercises#Exercise 5.85|7S Exercise 5.85]]). See [[Graphical Linear Algebra]].

> Sources: 7 Sketches, Exercise 5.84 and Solution A.5.

## Solution 5.85

#proof — [[7S Chapter 5 Exercises#Exercise 5.85|Exercise 5.85]]

If $(x, z) \in B \mathbin{;} C$ via $y$, then $(rx, ry) \in B$ and $(ry, rz) \in C$, so $(rx, rz) \in B \mathbin{;} C$; if also $(x', z') \in B \mathbin{;} C$ via $y'$, then $(x + x', y + y') \in B$ and $(y + y', z + z') \in C$, so $(x + x', z + z') \in B \mathbin{;} C$. Hence linear relations form the sub-prop $\mathbf{LinRel}_R$.

> Sources: 7 Sketches, Exercise 5.85 and Solution A.5.
