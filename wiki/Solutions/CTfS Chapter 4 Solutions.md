#solution

Solutions to the exercises of Category Theory for Scientists, Chapter 4 (written for this wiki; the book has none): [[CTfS Chapter 4 Exercises]]. Index: [[Map of Content]].

## Solution 4.1.1.8

[[CTfS Chapter 4 Exercises#Exercise 4.1.1.8|Exercise 4.1.1.8]]

A monotone map $[m] \to [n]$ is a non-decreasing sequence $f(0) \leq \cdots \leq f(m)$ in $\{0, \dots, n\}$, i.e. a multiset of size $m + 1$ from $n + 1$ values.

- **a.** $4$ (any point).
- **b.** $1$ ($[0]$ is terminal).
- **c.** $\binom{6}{3} = 20$.
- **d.** $\binom{n+2}{2} = \frac{(n+1)(n+2)}{2}$ (pairs $i \leq j$).
- **e.** $\binom{m + n + 1}{m + 1}$ by "stars and bars".

See [[Simplex Category]], [[Monotone Map]].

> Sources: CTfS, Exercise 4.1.1.8; see [[Map of Content]], [[Monotone Map]], [[Simplex Category]].

## Solution 4.1.2.29

[[CTfS Chapter 4 Exercises#Exercise 4.1.2.29|Exercise 4.1.2.29]]

*Itineraries*. A morphism from city $A$ to city $B$ is a finite sequence of connecting flights (each landing where the next departs), composition is concatenation of trips, and the identity at $A$ is "stay at $A$". Different itineraries between the same cities are different morphisms, since no equations are imposed ([[Free Category]], [[Path in a Graph]]). Real itineraries also need compatible *times*; modelling that requires a richer graph, e.g. with (city, time) vertices.

> Sources: CTfS, Exercise 4.1.2.29; see [[Free Category]], [[Map of Content]].

## Solution 4.2.1.10

[[CTfS Chapter 4 Exercises#Exercise 4.2.1.10|Exercise 4.2.1.10]]

- **a.** The symmetric group $\Sigma_4$ of permutations, $4! = 24$ elements.
- **b.** All functions $S \to S$ under composition, $4^4 = 256$ elements.
- **c.** No. $U(\mathrm{Aut}(S))$ is a proper submonoid (24 of 256) of $\mathrm{End}(S)$; non-bijective endomorphisms such as constant maps have no inverse ([[Endomorphism Monoid]]).

> Sources: CTfS, Exercise 4.2.1.10; see [[Endomorphism Monoid]], [[Map of Content]].

## Solution 4.2.1.11

[[CTfS Chapter 4 Exercises#Exercise 4.2.1.11|Exercise 4.2.1.11]]

Since there is at most one arrow between any ordered pair of vertices, an automorphism is determined by its vertex permutation. That permutation must preserve adjacency in the 4-cycle $1 - 2 - 4 - 3 - 1$. These are the symmetries of a square: 4 rotations (e.g. $1 \mapsto 2 \mapsto 4 \mapsto 3 \mapsto 1$) and 4 reflections (e.g. swap $2 \leftrightarrow 3$ fixing $1, 4$). So the automorphism group is the dihedral group $D_4$ of order 8 ([[Endomorphism Monoid]], [[Group Action]]).

> Sources: CTfS, Exercise 4.2.1.11; see [[Endomorphism Monoid]], [[Map of Content]].

## Solution 4.2.1.13

[[CTfS Chapter 4 Exercises#Exercise 4.2.1.13|Exercise 4.2.1.13]]

In a preorder, $x \cong y$ iff $x \leq y$ and $y \leq x$ (the unique arrows compose to identities automatically). Antisymmetry says this forces $x = y$. So a preorder is a partial order iff *the only isomorphisms in $P$ are identities*, i.e. $P$ is a skeletal category ([[Partial Order]], [[Skeleton]]).

> Sources: CTfS, Exercise 4.2.1.13; see [[Map of Content]], [[Partial Order]].

## Solution 4.2.1.22

[[CTfS Chapter 4 Exercises#Exercise 4.2.1.22|Exercise 4.2.1.22]]

Test: a graph admits a symmetric structure iff for all vertices $x, y$ the number of arrows $x \to y$ equals the number $y \to x$. Loops can be fixed by $\rho$.

- (b) No. In (3.3), $f : v \to w$ has no reverse, nor do $g, h : w \to x$. The part $y \rightleftarrows z$ with the loop $i$ would be symmetric on its own.
- (e) Yes, with $\rho = \mathrm{id}$.
- (f) Yes: $\rho(ij) = ji$.
- (a), (c), (d): apply the same count to the pictures. Any arrow without a partner in the opposite direction rules symmetry out.

Note that symmetry is *structure*, not just a property. A graph with two loops at a vertex admits several $\rho$ (swap them, or fix both). See [[Symmetric Graph]].

> Sources: CTfS, Exercise 4.2.1.22; see [[Map of Content]], [[Symmetric Graph]].

## Solution 4.2.1.23

[[CTfS Chapter 4 Exercises#Exercise 4.2.1.23|Exercise 4.2.1.23]]

The hom-sets of $\mathcal D$ are $\mathcal D(A, A) = \{\mathrm{id}, \rho\}$, $\mathcal D(A, V) = \{\mathrm{src}, \mathrm{tgt}\}$, $\mathcal D(V, V) = \{\mathrm{id}\}$, and $\mathcal D(V, A) = \varnothing$.

- **a.** $\mathcal C$ is free on the two arrows, so a functor is an object assignment plus two parallel morphisms. $(A, V) \mapsto (A, V)$: $2 \cdot 2 = 4$. $(A, V) \mapsto (A, A)$: $2 \cdot 2 = 4$. $(A, V) \mapsto (V, V)$: $1$. $(A, V) \mapsto (V, A)$: $0$. Total $9$.
- **b.** Yes: $\mathrm{src} \mapsto \mathrm{src}$, $\mathrm{tgt} \mapsto \mathrm{tgt}$, the inclusion.
- **c.** $S \circ i$ is the underlying graph: the same vertices and arrows, where every arrow appears together with its reverse, but $\rho$ is forgotten. The graph no longer "knows" which arrows are reverses of each other ([[Symmetric Graph]], [[Data Migration Functor]]: this is $\Delta_i$).

> Sources: CTfS, Exercise 4.2.1.23; see [[Map of Content]], [[Symmetric Graph]].

## Solution 4.2.2.3

[[CTfS Chapter 4 Exercises#Exercise 4.2.2.3|Exercise 4.2.2.3]]

Five. Paths alternate $c$ and $f$. Every occurrence of $c \mathbin{;} f$ reduces to $\mathrm{id}_F$, so the normal forms are $\mathrm{id}_F$, $\mathrm{id}_C$, $c$, $f$ and $f \mathbin{;} c$ ("a child's father's first child"). The last is an idempotent on $C$: $(f \mathbin{;} c) \mathbin{;} (f \mathbin{;} c) = f \mathbin{;} (c \mathbin{;} f) \mathbin{;} c = f \mathbin{;} c$. Longer paths reduce to these ([[Presentation of a Category]], [[Categories and Schemas are Equivalent]]).

> Sources: CTfS, Exercise 4.2.2.3; see [[Map of Content]], [[Presentation of a Category]].

## Solution 4.2.3.12

[[CTfS Chapter 4 Exercises#Exercise 4.2.3.12|Exercise 4.2.3.12]]

The set of paths from $p$ to $q$ up to homotopy (continuous deformation keeping the endpoints fixed): the "essentially different ways" of getting from $p$ to $q$ on the donut's surface. It is a torsor for $\pi_1(T, p) \cong \mathbb Z^2$, recording how many times the path winds around each of the two circles. Choosing one path identifies $\mathrm{Hom}(p, q) \cong \mathbb Z^2$ ([[Groupoid]]).

> Sources: CTfS, Exercise 4.2.3.12; see [[Groupoid]], [[Map of Content]].

## Solution 4.2.4.4

[[CTfS Chapter 4 Exercises#Exercise 4.2.4.4|Exercise 4.2.4.4]]

- **a.** Not covariantly. If $U \subseteq V$, a law respected by everyone in $V$ is respected by everyone in $U$, so $R(V) \subseteq R(U)$. We get maps $R(V) \to R(U)$, i.e. a functor $J^{\mathrm{op}} \to \mathbf{Set}$ (a presheaf), not $J \to \mathbf{Set}$. (There is no natural map $R(U) \to R(V)$ in general.)
- **b.** Yes. Order propositions by implication. If $U \subseteq V$, then $R(V) \subseteq R(U)$, so the conjunction over the larger set $R(U)$ implies the conjunction over $R(V)$: $\bigwedge R(U) \Rightarrow \bigwedge R(V)$. Hence $U \leq V$ gives $\bigwedge R(U) \leq \bigwedge R(V)$, a covariant functor $J \to \mathbf{Prop}$. See [[Contravariant Functor]], [[Presheaf]].

> Sources: CTfS, Exercise 4.2.4.4; see [[Contravariant Functor]], [[Map of Content]].

## Solution 4.3.1.10

[[CTfS Chapter 4 Exercises#Exercise 4.3.1.10|Exercise 4.3.1.10]]

- **a.** and b. Take every component equal to $f$: $\alpha_G := f : C_X(G) = X \to Y = C_Y(G)$. The naturality square for $h : G \to G'$ reads $\mathrm{id}_Y \circ f = f \circ \mathrm{id}_X$, which holds trivially. In fact every natural transformation $C_X \Rightarrow C_Y$ has this form, and $f \mapsto \alpha$ embeds $\mathbf{Set}$ into $\mathbf{Set}^{\mathbf{Grph}}$ ([[Constant Functor]], [[Natural Transformation]]).

> Sources: CTfS, Exercise 4.3.1.10; see [[Constant Functor]], [[Map of Content]].

## Solution 4.3.1.11

[[CTfS Chapter 4 Exercises#Exercise 4.3.1.11|Exercise 4.3.1.11]]

- **a.** and b. $\mathrm{src} : \mathrm{Ar} \Rightarrow \mathrm{Ve}$, whose component at a graph $G$ is its source function $\mathrm{src}_G : A_G \to V_G$. Naturality for a graph homomorphism $h = (h_0, h_1)$ is $\mathrm{src}_{G'} \circ h_1 = h_0 \circ \mathrm{src}_G$, exactly the defining condition of a graph homomorphism.
- **c.** Yes, by the other half of the definition: $\mathrm{tgt} : \mathrm{Ar} \Rightarrow \mathrm{Ve}$. In fact, by the [[Yoneda Lemma]] these are the only two natural transformations $\mathrm{Ar} \Rightarrow \mathrm{Ve}$. $\mathrm{Ar}$ and $\mathrm{Ve}$ are represented by the one-arrow graph and the one-vertex graph, and there are exactly two maps from the one-vertex graph to the one-arrow graph ([[Natural Transformation]], [[Representable Functor]]).

> Sources: CTfS, Exercise 4.3.1.11; see [[Map of Content]], [[Natural Transformation]].

## Solution 4.3.2.13

[[CTfS Chapter 4 Exercises#Exercise 4.3.2.13|Exercise 4.3.2.13]]

Renamings of states. A natural isomorphism $\alpha : X \cong Y$ is a bijection between the state sets that commutes with every input letter, $\alpha(\sigma \cdot s) = \sigma \cdot \alpha(s)$. So $Y$ is $X$ with its states relabelled, and the transition tables are the same up to the relabelling. Nothing observable about the machine's behaviour changes ([[Finite State Machine]], [[Natural Transformation]]).

> Sources: CTfS, Exercise 4.3.2.13; see [[Finite State Machine]], [[Map of Content]], [[Natural Transformation]].

## Solution 4.3.3.3

[[CTfS Chapter 4 Exercises#Exercise 4.3.3.3|Exercise 4.3.3.3]]

Take $\mathcal A = \mathrm{Disc}(A)$, the discrete schema with one object per $a \in A$ and no arrows. An instance assigns a set $S_a$ to each $a$, i.e. an $A$-indexed set. A natural transformation is a family of functions $S_a \to T_a$, with no naturality conditions because there are no non-identity arrows. That is exactly a mapping of $A$-indexed sets, so the notions align ([[Indexed Set]], [[C-Set]], [[Discrete Category]]).

> Sources: CTfS, Exercise 4.3.3.3; see [[C-Set]], [[Map of Content]].

## Solution 4.3.3.6

[[CTfS Chapter 4 Exercises#Exercise 4.3.3.6|Exercise 4.3.3.6]]

- **a.** $3$: a graph homomorphism out of $Y_A$ is determined by where the arrow $a$ goes (its endpoints are then forced), and $I$ has 3 arrows.
- **b.** $4$, the arrows $i, j, k, l$.
- **c.** $\mathrm{Hom}(Y_A, X) \cong \mathrm{Arrow}(X)$ naturally in $X$. $Y_A$ is the [[Representable Functor|representable]] graph $\mathcal C(A, -)$ on the "arrow" object, and this is the [[Yoneda Lemma]]. Likewise maps from the one-vertex graph pick out vertices ([[C-Set]]).

> Sources: CTfS, Exercise 4.3.3.6; see [[C-Set]], [[Map of Content]], [[Representable Functor]].

## Solution 4.3.4.5

[[CTfS Chapter 4 Exercises#Exercise 4.3.4.5|Exercise 4.3.4.5]]

Choose $p_X$ for each $X$ (with $p_{\underline n} = \mathrm{id}$). Define $F : \mathbf{Fin} \to \mathcal S$ by $F(X) = \underline{|X|}$ and $F(f : X \to Y) = p_Y \circ f \circ p_X^{-1}$. It is a functor: $F(g \circ f) = p_Z g p_Y^{-1} p_Y f p_X^{-1} = F(g) F(f)$. Let $G : \mathcal S \hookrightarrow \mathbf{Fin}$ be the inclusion. Then $F G = \mathrm{id}_{\mathcal S}$, and $p : \mathrm{id}_{\mathbf{Fin}} \Rightarrow G F$ with components $p_X$ is a natural isomorphism, since naturality is $p_Y \circ f = F(f) \circ p_X$ by definition. So $F$ and $G$ form an equivalence, and $\mathcal S$ is a [[Skeleton]] of $\mathbf{Fin}$ ([[Category of Finite Sets]], [[Equivalence of Categories]]).

> Sources: CTfS, Exercise 4.3.4.5; see [[Category of Finite Sets]], [[Map of Content]].

## Solution 4.3.4.13

[[CTfS Chapter 4 Exercises#Exercise 4.3.4.13|Exercise 4.3.4.13]]

- **a.** Not full: $\mathrm{Hom}_{\mathbf 2}(a, b) = \varnothing$ cannot map onto $\mathrm{Hom}_{\mathbf 1}(\ast, \ast) = \{\mathrm{id}\}$.
- **b.** Faithful: each hom-set of $\mathbf 2$ has at most one element, so each map on hom-sets is injective ([[Full and Faithful Functor]]).

> Sources: CTfS, Exercise 4.3.4.13; see [[Full and Faithful Functor]], [[Map of Content]].

## Solution 4.3.4.14

[[CTfS Chapter 4 Exercises#Exercise 4.3.4.14|Exercise 4.3.4.14]]

- **a.** and b. Yes, vacuously: there are no pairs of objects in $\mathbf 0$ to check.
- **c.** Only if $\mathcal C$ is empty. An equivalence is essentially surjective, and no object of a nonempty $\mathcal C$ is isomorphic to an object in the image. This shows that "fully faithful" alone does not give an equivalence ([[Full and Faithful Functor]], [[Equivalence of Categories]]).

> Sources: CTfS, Exercise 4.3.4.14; see [[Full and Faithful Functor]], [[Map of Content]].

## Solution 4.3.4.16

[[CTfS Chapter 4 Exercises#Exercise 4.3.4.16|Exercise 4.3.4.16]]

No. The unique functor sends both morphisms $\mathrm{id}, \sigma$ to $\mathrm{id}_\ast$, so it is full but not faithful. Hence $\mathbb Z_2 \not\simeq \mathbf 1$, although both have one object ([[Full and Faithful Functor]]).

> Sources: CTfS, Exercise 4.3.4.16; see [[Full and Faithful Functor]], [[Map of Content]].

## Solution 4.4.1.5

[[CTfS Chapter 4 Exercises#Exercise 4.4.1.5|Exercise 4.4.1.5]]

- **a.** $8$. Choose where $1$ goes and a path for $f_1$, then where $2$ goes and a path for $f_2$. The paths from $a$ are $\mathrm{id}_a$, $g$, $gh$, $i$; from $b$ they are $\mathrm{id}_b$, $h$; from $c$ only $\mathrm{id}_c$.
- $1 \mapsto a$ ($f_1 \mapsto \mathrm{id}_a$): then $f_2$ is any of the 4 paths from $a$. That gives 4.
- $1 \mapsto b$ ($f_1 \mapsto g$): then $f_2 \mapsto \mathrm{id}_b$ or $h$. That gives 2.
- $1 \mapsto c$ ($f_1 \mapsto gh$ or $i$): then $f_2 \mapsto \mathrm{id}_c$. That gives 2.

- **b.** $6$. $[2]$ is a preorder, so a morphism is determined by the images $\beta, \gamma$ of $b, c$ with $0 \leq \beta \leq \gamma$. The pairs are $(0,0), (0,1), (0,2), (1,1), (1,2), (2,2)$, and $i$ is forced to the unique path $0 \to \gamma$.

See [[Categories and Schemas are Equivalent]].

> Sources: CTfS, Exercise 4.4.1.5; see [[Categories and Schemas are Equivalent]], [[Map of Content]].

## Solution 4.4.1.6

[[CTfS Chapter 4 Exercises#Exercise 4.4.1.6|Exercise 4.4.1.6]]

- **a.** No. $L_1$ presents the monoid $\{\mathrm{id}, f\}$ with $f^2 = f$, which has two morphisms, while $\mathbf 1$ presents the terminal category with one.
- **b.** Yes, $L_0$: its PED is $f \simeq \mathrm{id}$, so it presents the terminal category, and the schema morphisms $f \mapsto$ (empty path) and back are mutually inverse up to path equivalence. For $n \geq 1$, $L_n$ has $n + 1$ morphisms ([[Categories and Schemas are Equivalent]]).

> Sources: CTfS, Exercise 4.4.1.6; see [[Categories and Schemas are Equivalent]], [[Map of Content]].

## Solution 4.4.1.7

[[CTfS Chapter 4 Exercises#Exercise 4.4.1.7|Exercise 4.4.1.7]]

A schema morphism $L_m \to L_n$ sends $f$ to a path $f^k$, taken up to path equivalence in $L_n$, where $f^a \simeq f^b$ iff $a = b$ or $a, b \geq n$. So $k \in \{0, \dots, n\}$. It must send the PED to a PED: $f^{k(m+1)} \simeq f^{km}$ in $L_n$. This holds iff $k = 0$ or $km \geq n$.

- **a.** $m = 3$, $n = 5$: $k = 0$ or $3k \geq 5$, i.e. $k \in \{0, 2, 3, 4, 5\}$, giving $5$.
- **b.** $m = 5$, $n = 3$: $k = 0$ or $5k \geq 3$, i.e. $k \in \{0, 1, 2, 3\}$, giving $4$.

Check against the hint: $m = 4$, $n = 9$ gives $k \in \{0\} \cup \{3, \dots, 9\}$, which is 8 ✓. See [[Categories and Schemas are Equivalent]].

> Sources: CTfS, Exercise 4.4.1.7; see [[Categories and Schemas are Equivalent]], [[Map of Content]], [[Start Here]].

## Solution 4.5.1.4

[[CTfS Chapter 4 Exercises#Exercise 4.5.1.4|Exercise 4.5.1.4]]

In the product order both coordinates must be related.
- $(2, 4) \preceq (3, 4)$: true ($2 \leq 3$ and $4 \mid 4$).
- $(2, 4) \preceq (3, 5)$: false ($4 \nmid 5$).
- $(2, 4) \preceq (8, 0)$: true ($2 \leq 8$, and $4 \mid 0$ since $4 \cdot 0 = 0$).
- $(2, 4) \preceq (0, 0)$: false ($2 \not\leq 0$).

See [[Divisibility Order]], [[Product Preorder]].

> Sources: CTfS, Exercise 4.5.1.4; see [[Divisibility Order]], [[Map of Content]].

## Solution 4.5.1.15

[[CTfS Chapter 4 Exercises#Exercise 4.5.1.15|Exercise 4.5.1.15]]

- **a.** $F(x) = (x, f(x))$, e.g. $(x, x + 7)$.
- **b.** $F = \langle \mathrm{id}_{\mathbb R}, f \rangle$ is the unique map with $\pi_1 \circ F = \mathrm{id}$ and $\pi_2 \circ F = f$ ([[Product]]).

> Sources: CTfS, Exercise 4.5.1.15; see [[Map of Content]], [[Product]].

## Solution 4.5.2.5

[[CTfS Chapter 4 Exercises#Exercise 4.5.2.5|Exercise 4.5.2.5]]

- **a.** No. The chain is indexed by the linear order $[\mathbb N] = (0 \to 1 \to 2 \to \cdots)$, with infinitely many objects and at most one arrow between any two. The loop is indexed by the monoid $\mathbb N$ (the schema $\mathrm{Loop}$), with one object and arrows $f^n$.
- **c.** The coincidence is that both diagrams send every generating arrow to the same $f$ and every object to $A$. There is a functor $[\mathbb N] \to \mathbb N$ (all objects ↦ the one object, $i \to j$ ↦ $f^{j-i}$), and the chain diagram factors through it. Their limits and colimits differ: the colimit of the chain is a sequential colimit ("where the iteration of $f$ ends up"), while the colimit of the loop is the coequalizer of $f$ and $\mathrm{id}$, the orbit set. See [[Diagram]].

> Sources: CTfS, Exercise 4.5.2.5; see [[Diagram]], [[Map of Content]].

## Solution 4.5.2.9

[[CTfS Chapter 4 Exercises#Exercise 4.5.2.9|Exercise 4.5.2.9]]

Each cone adds a new initial object below everything. $\mathcal C_1 = [0]$ (a point), $\mathcal C_2 = [1]$, $\mathcal C_3 = [2]$, and $\mathcal C_4 = [3]$: the linear order $\bullet \to \bullet \to \bullet \to \bullet$ with all composites, i.e. 4 objects and $\binom{4}{2} = 6$ non-identity arrows. The newest cone point is the bottom $0$ ([[Cone Category]], [[Simplex Category]]).

> Sources: CTfS, Exercise 4.5.2.9; see [[Cone Category]], [[Map of Content]], [[Simplex Category]].

## Solution 4.5.2.10

[[CTfS Chapter 4 Exercises#Exercise 4.5.2.10|Exercise 4.5.2.10]]

$\mathcal C^{\triangleleft}$ has objects $-\infty, A, V$ and arrows $\mathrm{src}, \mathrm{tgt} : A \to V$, $a : -\infty \to A$ and $v : -\infty \to V$ (unique), with the forced equations $a \mathbin{;} \mathrm{src} = v = a \mathbin{;} \mathrm{tgt}$. This is the shape of an *equalizer cone*: a diagram of this shape is an object $Z$ with a map to $A$ that equalizes $\mathrm{src}$ and $\mathrm{tgt}$. It is (4.18) plus the path equation, so limits of graphs are equalizers, i.e. sets of loops ([[Cone Category]], [[Equalizer]]).

> Sources: CTfS, Exercise 4.5.2.10; see [[Cone Category]], [[Map of Content]].

## Solution 4.5.3.12

[[CTfS Chapter 4 Exercises#Exercise 4.5.3.12|Exercise 4.5.3.12]]

Neither. The only object $\bullet$ would be initial iff $\mathrm{Hom}(\bullet, \bullet) = M$ had exactly one element, but $M$ is infinite. The same argument applies to terminal objects. A one-object category has an initial or terminal object only if it is the trivial monoid ([[Initial Object]], [[Terminal Object]]).

> Sources: CTfS, Exercise 4.5.3.12; see [[Map of Content]], [[Terminal Object]].

## Solution 4.5.3.13

[[CTfS Chapter 4 Exercises#Exercise 4.5.3.13|Exercise 4.5.3.13]]

If $S \neq \varnothing$, every object is both initial and terminal: from any $s$ to any $s'$ there is exactly one morphism. If $S = \varnothing$ there are none. That all objects are initial reflects that they are all uniquely isomorphic, and $K_S \simeq \mathbf 1$ ([[Codiscrete Category]]).

> Sources: CTfS, Exercise 4.5.3.13; see [[Codiscrete Category]], [[Map of Content]].

## Solution 4.5.3.19

[[CTfS Chapter 4 Exercises#Exercise 4.5.3.19|Exercise 4.5.3.19]]

- **a.** See Exercise 4.5.2.10: $-\infty \to A \rightrightarrows V$ with $a \mathbin{;} \mathrm{src} = a \mathbin{;} \mathrm{tgt}$.
- **b.** A cone is a set $Z$ with $p : Z \to A$ and $q : Z \to V$ such that $\mathrm{src} \circ p = q = \mathrm{tgt} \circ p$. For the graph of Example 3.3.1.2 ($f : v \to w$, $g, h : w \to x$, $i : y \to y$, $j : y \to z$, $k : z \to y$), take $Z = \{\ast, \ast'\}$ with both points mapped to the loop $i$ and to $y$.
- **c.** The limit is the set of *loops* of $G$, here $\{i\}$: the [[Equalizer]] of $\mathrm{src}$ and $\mathrm{tgt}$. The colimit is the set of connected components ([[Cone Category]], [[Limit]]).

> Sources: CTfS, Exercise 4.5.3.19; see [[Cone Category]], [[Map of Content]].

## Solution 4.6.2.5

[[CTfS Chapter 4 Exercises#Exercise 4.6.2.5|Exercise 4.6.2.5]]

Its objects are the states $0, 1, 2$ (pairs $(\bullet, s)$). A morphism $s \to s'$ is a word $w$ with $w \cdot s = s'$, and composition is concatenation. It is generated by the arrows $s \xrightarrow{a} a \cdot s$ and $s \xrightarrow{b} b \cdot s$, which are exactly the arrows drawn in Figure 3.1. So $\int F$ is the category presented by the state diagram, with the equations that hold in the action. The projection $\int F \to \mathcal M$ forgets the states and remembers the letters ([[Category of Elements]], [[Finite State Machine]]).

> Sources: CTfS, Exercise 4.6.2.5; see [[Category of Elements]], [[Finite State Machine]], [[Map of Content]].

## Solution 4.6.4.3

[[CTfS Chapter 4 Exercises#Exercise 4.6.4.3|Exercise 4.6.4.3]]

Its objects are triples $(\ast, \ast, f : c \to c')$, i.e. the morphisms $f \in \mathcal C(c, c')$. A morphism $(\ast, \ast, f) \to (\ast, \ast, g)$ consists of identity morphisms in $\mathbf 1$ making the square commute, which forces $f = g$. So $(c \downarrow c')$ is the discrete category on the hom-set $\mathcal C(c, c')$ ([[Comma Category]], [[Discrete Category]]).

> Sources: CTfS, Exercise 4.6.4.3; see [[Comma Category]], [[Map of Content]].
