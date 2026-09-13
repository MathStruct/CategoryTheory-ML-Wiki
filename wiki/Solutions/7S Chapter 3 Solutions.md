#solution

Solutions to the exercises of 7 Sketches, Chapter 3: [[7S Chapter 3 Exercises]]. Index: [[Map of Content]].

## Solution 3.3

[[7S Chapter 3 Exercises#Exercise 3.3|Exercise 3.3]]

Five and five; not a coincidence — in a [[Database Schema]] there is exactly one arrow per non-ID column.

> Sources: 7 Sketches, Exercise 3.3 and Solution A.3.

## Solution 3.9

#proof — [[7S Chapter 3 Exercises#Exercise 3.9|Exercise 3.9]]

Define a path as $(v, a_1, \dots, a_n)$ with $s(a_1) = v$, $t(a_i) = s(a_{i+1})$; source $v$, target $t(a_n)$ (or $v$ if $n = 0$); concatenation appends the arrow lists. Concatenating with a length-0 path $(v)$ returns the same tuple, and both bracketings of a triple concatenation give $(v, a_1..a_m, b_1..b_n, c_1..c_o)$. See [[Free Category]].

> Sources: 7 Sketches, Exercise 3.9 and Solution A.3.

## Solution 3.10

[[7S Chapter 3 Exercises#Exercise 3.10|Exercise 3.10]]

Morphisms $v_1, v_2, v_3$ (identities), $f_1, f_2, f_1 \mathbin{;} f_2$. Composites exist only when target meets source: $v_1 \mathbin{;} f_1 = f_1$, $f_1 \mathbin{;} f_2 = f_1 \mathbin{;} f_2$, $f_1 \mathbin{;} v_2 = f_1$, $v_2 \mathbin{;} f_2 = f_2$, $f_2 \mathbin{;} v_3 = f_2$, $(f_1 \mathbin{;} f_2) \mathbin{;} v_3 = f_1 \mathbin{;} f_2$, and identities compose with themselves; the identities sit on the diagonal.

> Sources: 7 Sketches, Exercise 3.10 and Solution A.3.

## Solution 3.12

[[7S Chapter 3 Exercises#Exercise 3.12|Exercise 3.12]]

$\underline{\mathbf{1}}$: one object, one (identity) morphism — the [[Terminal Object]] of $\mathbf{Cat}$. $\underline{\mathbf{0}}$: empty. $\underline{\mathbf{n}}$ has $1 + 2 + \cdots + n = n(n+1)/2$ morphisms (triangle numbers): $n - i$ paths of length $i$.

> Sources: 7 Sketches, Exercise 3.12 and Solution A.3.

## Solution 3.15

[[7S Chapter 3 Exercises#Exercise 3.15|Exercise 3.15]]

Addition: a path of length $m$ followed by one of length $n$ has length $m + n$. So $\mathrm{Free}(\circlearrowleft) \cong (\mathbb{N}, +, 0)$; see [[Free Category]], [[Monoid]].

> Sources: 7 Sketches, Exercise 3.15 and Solution A.3.

## Solution 3.16

[[7S Chapter 3 Exercises#Exercise 3.16|Exercise 3.16]]

$A, A\mathbin{;}f, A\mathbin{;}g, A\mathbin{;}f\mathbin{;}h, A\mathbin{;}g\mathbin{;}i, B, B\mathbin{;}h, C, C\mathbin{;}i, D$. Parallel: $f \mathbin{;} h$ and $g \mathbin{;} i$ (both $A \to D$). Non-parallel: $A$ and any other. See [[Presentation of a Category]].

> Sources: 7 Sketches, Exercise 3.16 and Solution A.3.

## Solution 3.17

[[7S Chapter 3 Exercises#Exercise 3.17|Exercise 3.17]]

$A, A\mathbin{;}f, A\mathbin{;}g, A\mathbin{;}j, B, B\mathbin{;}h, C, C\mathbin{;}i, D$ — nine, since $f \mathbin{;} h = j = g \mathbin{;} i$.

> Sources: 7 Sketches, Exercise 3.17 and Solution A.3.

## Solution 3.19

[[7S Chapter 3 Exercises#Exercise 3.19|Exercise 3.19]]

Four: $z = \mathrm{id}$, $s$, $s \mathbin{;} s$, $s \mathbin{;} s \mathbin{;} s$; the equation $s^4 = s^2$ collapses all longer paths.

> Sources: 7 Sketches, Exercise 3.19 and Solution A.3.

## Solution 3.21

[[7S Chapter 3 Exercises#Exercise 3.21|Exercise 3.21]]

$G_1$: $f = g$. $G_2$: $f = \mathrm{id}$ ($f = a$ where $a$ is the identity path). $G_3$: $f \mathbin{;} h = g \mathbin{;} i$. $G_4$: no equations — there are no parallel paths. A [[Hasse Diagram]] presents a preorder by equating all parallel paths.

> Sources: 7 Sketches, Exercise 3.21 and Solution A.3.

## Solution 3.22

[[7S Chapter 3 Exercises#Exercise 3.22|Exercise 3.22]]

$\underline{1}$: one object with its identity, since there are morphisms from the object to itself.

> Sources: 7 Sketches, Exercise 3.22 and Solution A.3.

## Solution 3.25

[[7S Chapter 3 Exercises#Exercise 3.25|Exercise 3.25]]

Functions are pairs $(f(1), f(2))$: $(1,1), (1,2), (1,3), (2,1), (2,2), (2,3), (3,1), (3,2), (3,3)$ — a $3 \times 3$ grid, $|3^2| = 9$ (the [[Exponential Object]]).

> Sources: 7 Sketches, Exercise 3.25 and Solution A.3.

## Solution 3.30

[[7S Chapter 3 Exercises#Exercise 3.30|Exercise 3.30]]

$f^{-1}(1) = b$, $f^{-1}(2) = a$, $f^{-1}(3) = c$. There are $3! = 6$ isomorphisms; in general $n!$ between two $n$-element sets. See [[Isomorphism]].

> Sources: 7 Sketches, Exercise 3.30 and Solution A.3.

## Solution 3.31

#proof — [[7S Chapter 3 Exercises#Exercise 3.31|Exercise 3.31]]

Take $f = \mathrm{id}_c$: $\mathrm{id}_c \mathbin{;} \mathrm{id}_c = \mathrm{id}_c$ in both orders, so it is its own inverse.

> Sources: 7 Sketches, Exercise 3.31 and Solution A.3.

## Solution 3.32

[[7S Chapter 3 Exercises#Exercise 3.32|Exercise 3.32]]

$\mathbb{N}$ is not: $s$ has no inverse, since $s^n \mathbin{;} s = s^{n+1} \neq s^0$. $\mathcal{C}$ is: $s$ is its own inverse; it is $\mathbb{Z}/2\mathbb{Z}$.

> Sources: 7 Sketches, Exercise 3.32 and Solution A.3.

## Solution 3.33

#proof — [[7S Chapter 3 Exercises#Exercise 3.33|Exercise 3.33]]

Yes. Lengths add under composition and identities are the length-0 paths; if $p \mathbin{;} q = \mathrm{id}$ then $p$ and $q$ have length 0.

> Sources: 7 Sketches, Exercise 3.33 and Solution A.3.

## Solution 3.37

[[7S Chapter 3 Exercises#Exercise 3.37|Exercise 3.37]]

A functor is determined by the images of $m_0 \leq m_1$, which must satisfy $F(m_0) \leq F(m_1)$ in $n_0 \leq n_1 \leq n_2$: six choices in total, so the remaining three are $(n_1, n_1)$, $(n_1, n_2)$, $(n_2, n_2)$ — with $f_1$ sent to the unique path. See [[Functor]], [[Walking Arrow]].

> Sources: 7 Sketches, Exercise 3.37 and Solution A.3.

## Solution 3.39

[[7S Chapter 3 Exercises#Exercise 3.39|Exercise 3.39]]

$A' \mapsto A$, $B' \mapsto B$, $C' \mapsto C$, $D' \mapsto D$, $f' \mapsto f$, $g' \mapsto g$, $h' \mapsto h$, $i' \mapsto i$, $f' \mathbin{;} h' \mapsto f \mathbin{;} h$, $g' \mathbin{;} i' \mapsto f \mathbin{;} h$ — which is also $g \mathbin{;} i$, so the last is not an outlier. See [[Presentation of a Category]].

> Sources: 7 Sketches, Exercise 3.39 and Solution A.3.

## Solution 3.40

[[7S Chapter 3 Exercises#Exercise 3.40|Exercise 3.40]]

$F(a) = G(a) = a'$, $F(b) = G(b) = b'$, $F(f) = f_1$, $G(f) = f_2$. Functors are not in general determined by their action on objects.

> Sources: 7 Sketches, Exercise 3.40 and Solution A.3.

## Solution 3.43

#proof — [[7S Chapter 3 Exercises#Exercise 3.43|Exercise 3.43]]

1. $\mathrm{id}_{\mathcal{C}}$ fixing every object and morphism preserves identities and composites trivially.
2. For $F : \mathcal{C} \to \mathcal{D}$, $G : \mathcal{D} \to \mathcal{E}$, $(F \mathbin{;} G)(\mathrm{id}_c) = G(F(\mathrm{id}_c)) = G(\mathrm{id}_{Fc}) = \mathrm{id}_{GFc}$ and $(F \mathbin{;} G)(f \mathbin{;} g) = G(Ff \mathbin{;} Fg) = GFf \mathbin{;} GFg$.
3. Unitality and associativity hold because they hold for the underlying functions on objects and hom-sets: $((F \mathbin{;} G) \mathbin{;} H)(x) = H(G(F(x))) = (F \mathbin{;} (G \mathbin{;} H))(x)$. See [[Category of Categories]].

> Sources: 7 Sketches, Exercise 3.43 and Solution A.3.

## Solution 3.45

[[7S Chapter 3 Exercises#Exercise 3.45|Exercise 3.45]]

$F_S(1) := S$, $F_S(\mathrm{id}_1) := \mathrm{id}_S$; the only composite in $\underline{\mathbf{1}}$ is $\mathrm{id}_1 \mathbin{;} \mathrm{id}_1$, which is preserved. So sets are [[C-Set|instances]] on the schema $\underline{\mathbf{1}}$.

> Sources: 7 Sketches, Exercise 3.45 and Solution A.3.

## Solution 3.48

[[7S Chapter 3 Exercises#Exercise 3.48|Exercise 3.48]]

1. A set with an **involution**: everyone picks a partner (possibly themselves) and swaps — a "do-si-do"; e.g. pixels of a photo with the mirror-image map.
2. A **secret-Santa** party: $D(c)$ the people, $D(b)$ the gifts, $g$ the giver and $h$ the receiver of each gift, $D(a)$ the gifts given to oneself with $f$ the inclusion. See [[C-Set]].

> Sources: 7 Sketches, Exercise 3.48 and Solution A.3.

## Solution 3.55

#proof — [[7S Chapter 3 Exercises#Exercise 3.55|Exercise 3.55]]

1. "For each object $c$, compose the $c$-components": $(\alpha \mathbin{;} \beta)_c := \alpha_c \mathbin{;} \beta_c$. Naturality: the outer rectangle of two pasted naturality squares commutes. "Most beginners think of a natural transformation via its squares, but the main thing is its components; the squares are a check that comes later."
2. $(\mathrm{id}_F)_c := \mathrm{id}_{F(c)}$; its naturality square commutes trivially, and $(\mathrm{id}_F \mathbin{;} \beta)_c = \mathrm{id}_{F(c)} \mathbin{;} \beta_c = \beta_c$.

> Sources: 7 Sketches, Exercise 3.55 and Solution A.3.

## Solution 3.58

[[7S Chapter 3 Exercises#Exercise 3.58|Exercise 3.58]]

1. True: each component $\alpha_c : F(c) \to G(c)$ lives in a hom-set of $\mathcal{P}$ with at most one element.
2. False: $\mathcal{P} = \underline{1}$, $\mathcal{C} = a \rightrightarrows b$ with $f_1, f_2$, $F(1) = a$, $G(1) = b$: both $\alpha_1 = f_1$ and $\beta_1 = f_2$ are natural transformations. See [[Natural Transformation]].

> Sources: 7 Sketches, Exercise 3.58 and Solution A.3.

## Solution 3.62

[[7S Chapter 3 Exercises#Exercise 3.62|Exercise 3.62]]

Vertex table: Employee, Department, string. Arrow table (source, target): Mngr (Employee, Employee), WorksIn (Employee, Department), Secr (Department, Employee), FName (Employee, string), DName (Department, string). See [[Category of Graphs]].

> Sources: 7 Sketches, Exercise 3.62 and Solution A.3.

## Solution 3.64

[[7S Chapter 3 Exercises#Exercise 3.64|Exercise 3.64]]

$\alpha_{\mathrm{Arrow}}(b) = e$; $\alpha_{\mathrm{Vertex}}(1) = 4$, $\alpha_{\mathrm{Vertex}}(2) = 5$, $\alpha_{\mathrm{Vertex}}(3) = 5$. Check: $\mathrm{source}(\alpha(a)) = \mathrm{source}(d) = 4 = \alpha(1) = \alpha(\mathrm{source}(a))$, and both paths from $a$ through target end at $5$; similarly for $b$.

> Sources: 7 Sketches, Exercise 3.64 and Solution A.3.

## Solution 3.67

[[7S Chapter 3 Exercises#Exercise 3.67|Exercise 3.67]]

Arrow table: arrow $s$ has source $\mathsf{next}(s)$ and target $s$: $(1: 4 \to 1), (2: 4 \to 2), (3: 5 \to 3), (4: 5 \to 4), (5: 5 \to 5), (6: 7 \to 6), (7: 6 \to 7)$ — the graph of Eq. (3.66) with all arrows reversed. See [[Discrete Dynamical System]], [[Data Migration Functor]].

> Sources: 7 Sketches, Exercise 3.67 and Solution A.3.

## Solution 3.73

[[7S Chapter 3 Exercises#Exercise 3.73|Exercise 3.73]]

1. $f \times B : (x, b) \mapsto (f(x), b)$ — functorial. 2. $f^B : g \mapsto g \mathbin{;} f$ for $g : B \to X$ — functorial since $(f_1 \mathbin{;} f_2)^B = f_1^B \mathbin{;} f_2^B$. 3. $p(3) : \mathbb{N} \to \mathbb{N}$ is $n \mapsto n + 3$, "the function that adds three".

> Sources: 7 Sketches, Exercise 3.73 and Solution A.3.

## Solution 3.76

[[7S Chapter 3 Exercises#Exercise 3.76|Exercise 3.76]]

Every object goes to the unique object $1$ and every morphism to $\mathrm{id}_1$. Hence $\underline{\mathbf{1}}$ is the [[Terminal Object]] of $\mathbf{Cat}$; migration along $!$ gives [[Data Migration Functor|single-set summaries]].

> Sources: 7 Sketches, Exercise 3.76 and Solution A.3.

## Solution 3.78

[[7S Chapter 3 Exercises#Exercise 3.78|Exercise 3.78]]

Vertices Bob, Doug, Emmy, Grace, Pat, Sue; arrows $\mathsf{Em}_1 : B \to G$, $\mathsf{Em}_2 : G \to P$, $\mathsf{Em}_3 : B \to E$, $\mathsf{Em}_4 : S \to D$, $\mathsf{Em}_5 : D \to S$, $\mathsf{Em}_6 : B \to B$ (a loop). Two connected components — the two values of $\Sigma_!(I)$; one loop — the single element of $\Pi_!(I)$ ([[Data Migration Functor]]).

> Sources: 7 Sketches, Exercise 3.78 and Solution A.3.

## Solution 3.81

#proof — [[7S Chapter 3 Exercises#Exercise 3.81|Exercise 3.81]]

In a preorder there is at most one morphism between two objects, so "a unique morphism $c \to z$ for every $c$" reduces to "a morphism $c \to z$ for every $c$", i.e. $c \leq z$ for all $c$.

> Sources: 7 Sketches, Exercise 3.81 and Solution A.3.

## Solution 3.82

[[7S Chapter 3 Exercises#Exercise 3.82|Exercise 3.82]]

$\underline{\mathbf{1}}$, by [[7S Chapter 3 Exercises#Exercise 3.76|7S Exercise 3.76]]: exactly one functor $\mathcal{C} \to \underline{\mathbf{1}}$ for every $\mathcal{C}$.

> Sources: 7 Sketches, Exercise 3.82 and Solution A.3.

## Solution 3.83

[[7S Chapter 3 Exercises#Exercise 3.83|Exercise 3.83]]

The [[Discrete Category]] on two objects ($\mathrm{Free}$ of two vertices, no arrows): there are no morphisms from one object to the other. Another: $(\mathbb{N}, \leq)$ has no top element.

> Sources: 7 Sketches, Exercise 3.83 and Solution A.3.

## Solution 3.88

#proof — [[7S Chapter 3 Exercises#Exercise 3.88|Exercise 3.88]]

A product is $z$ with $z \leq x$, $z \leq y$ such that any $z'$ with $z' \leq x$, $z' \leq y$ has $z' \leq z$; uniqueness of the mediating map and commutativity of the triangles are automatic in a preorder. This is exactly the definition of the meet.

> Sources: 7 Sketches, Exercise 3.88 and Solution A.3.

## Solution 3.90

[[7S Chapter 3 Exercises#Exercise 3.90|Exercise 3.90]]

1. $(\mathrm{id}_c, \mathrm{id}_d)$. 2. Composition is componentwise, and each component is associative. 3. Two objects $(1,1), (1,2)$ and one non-identity morphism: it is (isomorphic to) $\underline{\mathbf{2}}$; in general $\underline{\mathbf{1}} \times \mathcal{C} \cong \mathcal{C}$. 4. The category of the [[Product Preorder]] $P \times Q$.

> Sources: 7 Sketches, Exercise 3.90 and Solution A.3.

## Solution 3.91

#proof — [[7S Chapter 3 Exercises#Exercise 3.91|Exercise 3.91]]

An object of $\mathrm{Cone}(X, Y)$ is an object $Z$ with maps to $X$ and $Y$ (a [[Span]]); a morphism $Z' \to Z$ is a map making the two triangles commute. The product's universal property says exactly that every such $Z'$ has a unique morphism to $X \times Y$ in $\mathrm{Cone}(X, Y)$: terminality.

> Sources: 7 Sketches, Exercise 3.91 and Solution A.3.

## Solution 3.97

[[7S Chapter 3 Exercises#Exercise 3.97|Exercise 3.97]]

For two vertices $v_1, v_2$ with $D(v_1) = A$, $D(v_2) = B$ and no arrows, the formula gives $\{(d_1, d_2) \mid d_1 \in A, d_2 \in B\} = A \times B$ with the projections. See [[Finite Limits in Set]].

> Sources: 7 Sketches, Exercise 3.97 and Solution A.3.

## Solution 3.98

[[7S Chapter 3 Exercises#Exercise 3.98|Exercise 3.98]]

One vertex, no arrows: $\lim D = \{(d) \mid d \in D(1)\} \cong D(1)$. Directly: a cone is a set $C$ with a map $C \to D(1)$, and the terminal one is $\mathrm{id} : D(1) \to D(1)$.

> Sources: 7 Sketches, Exercise 3.98 and Solution A.3.

## Solution 3.101

[[7S Chapter 3 Exercises#Exercise 3.101|Exercise 3.101]]

$F^{\mathrm{op}}(c) := F(c)$ on objects; a morphism $f : c_1 \to c_2$ in $\mathcal{C}^{\mathrm{op}}$ is $f' : c_2 \to c_1$ in $\mathcal{C}$, so set $F^{\mathrm{op}}(f) := F(f')^{\mathrm{op}} : F(c_1) \to F(c_2)$ in $\mathcal{D}^{\mathrm{op}}$. It preserves identities and composites. See [[Opposite Category]].

> Sources: 7 Sketches, Exercise 3.101 and Solution A.3.
