#solution

Solutions to the exercises of Category Theory for Scientists, Chapter 3 (written for this wiki; the book has none): [[CTfS Chapter 3 Exercises]]. Index: [[Map of Content]].

## Solution 3.1.1.7

[[CTfS Chapter 3 Exercises#Exercise 3.1.1.7|Exercise 3.1.1.7]]

- **b.** first: a one-element set $\{e\}$ with $e \star e = e$. The empty set cannot be a monoid, since a monoid must contain its unit.
- **a.** Three elements. A monoid with one element is trivial. With two elements $\{e, a\}$, the only product not fixed by the unit laws is $a \star a$, and any choice is commutative. With three elements, take $\{e, a, b\}$ where $x \star y = y$ for $x, y \in \{a, b\}$ ("the last one wins"). This is associative, since $x \star y \star z = z$ whenever the last factor is not $e$, and $a \star b = b \neq a = b \star a$ ([[Monoid]]).

> Sources: CTfS, Exercise 3.1.1.7; see [[Map of Content]], [[Monoid]].

## Solution 3.1.1.13

[[CTfS Chapter 3 Exercises#Exercise 3.1.1.13|Exercise 3.1.1.13]]

- **a.** $\mathrm{List}(\{\ast\}) = \{[\,], [\ast], [\ast, \ast], \dots\}$; a list is determined by its length and concatenation adds lengths, so it is $(\mathbb N, 0, +)$.
- **b.** $\mathrm{List}(\varnothing) = \{[\,]\}$, the trivial monoid ([[Free Monoid]]).

> Sources: CTfS, Exercise 3.1.1.13; see [[Free Monoid]], [[Map of Content]].

## Solution 3.1.1.18

[[CTfS Chapter 3 Exercises#Exercise 3.1.1.18|Exercise 3.1.1.18]]

With size 3, the relations are $[x_1, x_2, x_3, y] \sim_1 [x_1, x_2, y]$ and $[x_1, x_2, x_3, y] \sim_2 [x_1, x_2, x_3]$, applied inside longer words (the congruence generated).

$\sim_1$: $[a, b, c, d, e, f] = [a, b, c, d] \cdot [e, f] \sim_1 [a, b, d] \cdot [e, f] = [a, b, d, e] \cdot [f] \sim_1 [a, b, e] \cdot [f] = [a, b, e, f] \sim_1 [a, b, f]$.

$\sim_2$: $[a, b, c, d, e, f] \sim_2 [a, b, c, e, f] \sim_2 [a, b, c, f] \sim_2 [a, b, c]$.

See [[Presentation of a Monoid]].

> Sources: CTfS, Exercise 3.1.1.18; see [[Map of Content]], [[Presentation of a Monoid]].

## Solution 3.1.1.23

[[CTfS Chapter 3 Exercises#Exercise 3.1.1.23|Exercise 3.1.1.23]]

Names: the symbol $\infty$, and pairs $(n, k)$ with $n \in \mathbb N$, $k \geq 1$.

- $\infty$ names $\mathbb N = \langle Q \mid \ \rangle$, where all powers $Q^i$ are distinct.
- $(n, k)$ names $C_{n,k} = \langle Q \mid Q^{n+k} = Q^n \rangle$. It has elements $Q^0, \dots, Q^{n+k-1}$, a "tail" of length $n$ running into a cycle of length $k$ (the shape of the letter ρ).

*Every cyclic monoid is named.* If the powers are distinct, the monoid is $\mathbb N$. Otherwise let $n$ be least such that $Q^n = Q^m$ for some $m > n$, and let $k$ be the least such $m - n$. Then $Q^i = Q^j$ iff $i = j$ or ($i, j \geq n$ and $i \equiv j \bmod k$), so the monoid is $C_{n,k}$.

*Names are distinct.* The cycle $\{Q^i \mid i \geq n\}$ is the smallest *ideal* of $C_{n,k}$, i.e. the smallest nonempty subset closed under multiplication by arbitrary elements. Being defined without reference to the generator, it is an isomorphism invariant. So $k$ is the size of the smallest ideal and $n + k$ is the size of the monoid, and both are recovered from the isomorphism class. $\mathbb N$ is the only infinite one.

The hint fails because both $n$ and $k$ vary: $C_{0,2} = \mathbb Z/2$ and $C_{1,1} = \{1, Q\}$ with $Q^2 = Q$ both have two elements but are not isomorphic. See [[Presentation of a Monoid]].

> Sources: CTfS, Exercise 3.1.1.23; see [[Map of Content]], [[Presentation of a Monoid]].

## Solution 3.1.2.4

[[CTfS Chapter 3 Exercises#Exercise 3.1.2.4|Exercise 3.1.2.4]]

- **a.** Take $\mathrm{id}, (+12) : \mathbb R \rightrightarrows \mathbb R$. The coequalizer identifies $y$ with $y + 12$, giving $q : \mathbb R \to \mathbb R/12\mathbb Z \cong [0, 12)$ with $q(y) = y \bmod 12$.
- **b.** The map $q \circ (x + -) : \mathbb R \to T$ coequalizes the pair: $q(x + y + 12) = q(x + y)$. So it factors uniquely through $q$ as a map $x \cdot - : T \to T$ with $x \cdot q(y) = q(x + y)$.
- **c.** $0 \cdot q(y) = q(y)$, and $x \cdot (x' \cdot q(y)) = q(x + x' + y) = (x + x') \cdot q(y)$. Uniqueness in the universal property makes these equations hold on the nose. So $(\mathbb R, 0, +)$ acts on $T$ ([[Monoid Action]], [[Coequalizer]]).

> Sources: CTfS, Exercise 3.1.2.4; see [[Map of Content]], [[Monoid Action]].

## Solution 3.1.2.13

[[CTfS Chapter 3 Exercises#Exercise 3.1.2.13|Exercise 3.1.2.13]]

Read words left to right: $\psi(f)([\,], s) = s$ and $\psi(f)([\sigma] \mathbin{+\!\!+} w, s) = \psi(f)(w, f(\sigma, s))$. The action law then reads $\psi(f)(u \mathbin{+\!\!+} v, s) = \psi(f)(v, \psi(f)(u, s))$, "first $u$, then $v$". This is a right action; CTfS's recursion from the other end gives the mirror-image convention, see [[Finite State Machine]].

- **a.** The unit law holds by definition. For the action law, induct on the length of $u$. For $u = [\,]$ both sides are $\psi(f)(v, s)$. For $u = [\sigma] \mathbin{+\!\!+} u'$, both sides unfold to $\psi(f)(u' \mathbin{+\!\!+} v, f(\sigma, s))$ and $\psi(f)(v, \psi(f)(u', f(\sigma, s)))$, which agree by the induction hypothesis.
- **b.** $\varphi(\psi(f))(\sigma, s) = \psi(f)([\sigma], s) = f(\sigma, s)$, so $\varphi \psi = \mathrm{id}$. Conversely, for an action $\alpha$, $\psi(\varphi(\alpha))$ and $\alpha$ agree on the empty word (unit law) and on one-letter words. By the action law both are determined by their values on letters, so an induction on word length shows they agree everywhere. This is the universal property of the [[Free Monoid]], composed with [[Currying]].

> Sources: CTfS, Exercise 3.1.2.13; see [[Finite State Machine]], [[Map of Content]].

## Solution 3.1.4.7

[[CTfS Chapter 3 Exercises#Exercise 3.1.4.7|Exercise 3.1.4.7]]

- $M \to N$: $n \mapsto c\,n$ for any $c > 0$, e.g. the inclusion.
- $M \to P$: $n \mapsto a^n$ for any $a > 0$, $a \neq 1$, e.g. $n \mapsto 2^n$.
- $N \to P$: $x \mapsto e^{x}$ (or $a^x$).
- $N \to M$: only the trivial homomorphism $x \mapsto 0$. For any $x$ and $k$, $h(x) = 2^k\, h(x / 2^k)$, so the natural number $h(x)$ is divisible by every power of 2, hence $h(x) = 0$.
- $P \to N$: $x \mapsto \log x$ fails, since $\log x < 0$ for $x < 1$. In fact only the trivial homomorphism exists: $h(x) + h(1/x) = h(1) = 0$ with both terms $\geq 0$ forces $h \equiv 0$.

(The $\log$ does give an isomorphism $P \cong (\mathbb R, 0, +)$ — you just need negative numbers.) See [[Monoid]].

> Sources: CTfS, Exercise 3.1.4.7; see [[Map of Content]], [[Monoid]].

## Solution 3.1.4.15

[[CTfS Chapter 3 Exercises#Exercise 3.1.4.15|Exercise 3.1.4.15]]

The action table of Example 3.1.3.1 is $a : 0 \mapsto 1, 1 \mapsto 2, 2 \mapsto 0$ and $b : 0 \mapsto 2, 1 \mapsto 1, 2 \mapsto 0$. The generator $1 \in \mathbb N$ acts as the word $[a, b, b]$:

| state | $1$ (= $[a,b,b]$) |
|---|---|
| State 0 | State 1 |
| State 1 | State 2 |
| State 2 | State 0 |

Reading left to right: $0 \xrightarrow{a} 1 \xrightarrow{b} 1 \xrightarrow{b} 1$, $1 \xrightarrow{a} 2 \xrightarrow{b} 0 \xrightarrow{b} 2$, $2 \xrightarrow{a} 0 \xrightarrow{b} 2 \xrightarrow{b} 0$. Reading right to left gives the same table. $n \in \mathbb N$ acts as the $n$-th power, a cyclic rotation ([[Finite State Machine]], [[Monoid Action]]).

> Sources: CTfS, Exercise 3.1.4.15; see [[Finite State Machine]], [[Map of Content]].

## Solution 3.2.1.8

[[CTfS Chapter 3 Exercises#Exercise 3.2.1.8|Exercise 3.2.1.8]]

Exactly the $C_{0,k} = \langle Q \mid Q^k = 1 \rangle \cong \mathbb Z/k$ for $k \geq 1$, including the trivial group $k = 1$. If the tail $n$ is positive, $Q$ has no inverse: $Q \cdot Q^j = Q^{j+1}$ never returns to $Q^0$. $\mathbb N$ is not a group either. The infinite cyclic group $\mathbb Z$ is not a cyclic *monoid*: as a monoid it needs two generators $Q, Q^{-1}$ ([[Presentation of a Monoid]], [[Group]]).

> Sources: CTfS, Exercise 3.2.1.8; see [[Map of Content]], [[Presentation of a Monoid]].

## Solution 3.2.1.14

[[CTfS Chapter 3 Exercises#Exercise 3.2.1.14|Exercise 3.2.1.14]]

- **a.** The orbit of $(x, y, z)$ is the horizontal circle of radius $\sqrt{x^2 + y^2}$ at height $z$ centred on the axis. Points on the $z$-axis are fixed, so their orbits are single points. The orbit set is in bijection with the half-plane $\{(r, z) \mid r \geq 0\}$ via $(x, y, z) \mapsto (\sqrt{x^2+y^2}, z)$.
- **b.** A single orbit $\{1, 2, 3\}$: any element can be moved to any other (the action is transitive). See [[Group Action]].

> Sources: CTfS, Exercise 3.2.1.14; see [[Group Action]], [[Map of Content]].

## Solution 3.2.1.15

[[CTfS Chapter 3 Exercises#Exercise 3.2.1.15|Exercise 3.2.1.15]]

Yes. Write $x \sim y$ iff $y = g \cdot x$ for some $g$. It is *reflexive* because $x = e \cdot x$. It is *symmetric*: if $y = g \cdot x$ then $x = g^{-1} \cdot y$ (this uses inverses). It is *transitive*: if $y = g \cdot x$ and $z = h \cdot y$ then $z = (hg) \cdot x$. The classes are the orbits, which therefore [[Partition|partition]] $X$. For a mere monoid action, "reachable from" is only a preorder ([[Group Action]], [[Equivalence Relation]]).

> Sources: CTfS, Exercise 3.2.1.15; see [[Equivalence Relation]], [[Group Action]], [[Map of Content]].

## Solution 3.3.2.4

[[CTfS Chapter 3 Exercises#Exercise 3.3.2.4|Exercise 3.3.2.4]]

In general no. Concatenation $p \mathbin{+\!\!+} q$ is only defined when the target of $p$ is the source of $q$. And there is no single identity: the trivial path at a vertex $v$ is a unit only for paths starting or ending at $v$. With two or more vertices there are several "identities". Composition is partial and the identities are indexed by vertices, which is exactly the structure of a *category* — the [[Free Category]] on $G$. When $G$ has one vertex, the paths do form a monoid: the free monoid on the arrows.

> Sources: CTfS, Exercise 3.3.2.4; see [[Free Category]], [[Map of Content]].

## Solution 3.3.3.5

[[CTfS Chapter 3 Exercises#Exercise 3.3.3.5|Exercise 3.3.3.5]]

- **a.** Lengths are preserved: a path $a_1 a_2 \cdots a_n$ goes to $f_1(a_1) \cdots f_1(a_n)$.
- **b.** Yes. Two paths with the same image have the same length and the same image arrow by arrow, so by injectivity of $f_1$ they are equal. For length-0 paths use injectivity of $f_0$.
- **c.** No. Let $G$ have two separate arrows $a : 1 \to 2$ and $b : 3 \to 4$, and let $G'$ be $x \xrightarrow{a'} y \xrightarrow{b'} z$, with $f$ sending $2, 3 \mapsto y$. Then $f$ is surjective on vertices and arrows, but the path $a' b'$ of length 2 is not the image of any path in $G$. See [[Graph Homomorphism]], [[Path in a Graph]].

> Sources: CTfS, Exercise 3.3.3.5; see [[Graph Homomorphism]], [[Map of Content]].

## Solution 3.3.3.6

[[CTfS Chapter 3 Exercises#Exercise 3.3.3.6|Exercise 3.3.3.6]]

Yes. Two maps into a product are equal iff their composites with both projections are equal. And $\pi_1 \circ i' \circ f_1 = \mathrm{src}' \circ f_1$ while $\pi_1 \circ (f_0 \times f_0) \circ i = f_0 \circ \pi_1 \circ i = f_0 \circ \mathrm{src}$, and likewise for $\pi_2$ and $\mathrm{tgt}$. So the single square commutes iff both squares do ([[Graph Homomorphism]], [[Product]]).

> Sources: CTfS, Exercise 3.3.3.6; see [[Graph Homomorphism]], [[Map of Content]].

## Solution 3.3.3.9

[[CTfS Chapter 3 Exercises#Exercise 3.3.3.9|Exercise 3.3.3.9]]

$R_\varepsilon = \{(x, y) \mid |x - y| \leq \varepsilon\}$ is the closed diagonal band between the lines $y = x - \varepsilon$ and $y = x + \varepsilon$. It contains the diagonal, so it is reflexive, and it is symmetric about the diagonal. It is not transitive: $(0, \varepsilon)$ and $(\varepsilon, 2\varepsilon)$ lie in the band but $(0, 2\varepsilon)$ does not. Approximate equality is a tolerance relation, not an [[Equivalence Relation]] ([[Relation]]).

> Sources: CTfS, Exercise 3.3.3.9; see [[Map of Content]], [[Relation]].

## Solution 3.4.1.8

[[CTfS Chapter 3 Exercises#Exercise 3.4.1.8|Exercise 3.4.1.8]]

- **a.** Four. Every preorder contains $(1,1), (2,2)$; the other pairs are optional: the discrete order (nothing else), $1 \leq 2$, $2 \leq 1$, and the indiscrete order (both).
- **b.** $n!$: a linear order is a ranking of the $n$ elements.
- **c.** Yes: the empty set has exactly one (empty) linear order, and $0! = 1$ ([[Preorder]], [[Total Order]]).

> Sources: CTfS, Exercise 3.4.1.8; see [[Map of Content]], [[Preorder]].

## Solution 3.4.1.12

[[CTfS Chapter 3 Exercises#Exercise 3.4.1.12|Exercise 3.4.1.12]]

False as stated: every single element $\{x\}$ is a clique (a set of mutually related elements), so no nonempty preorder has "no cliques". The nearby true statement: a partial order is a preorder in which every clique has at most one element. Antisymmetry says exactly that $x \leq y \leq x$ forces $x = y$ ([[Partial Order]]).

> Sources: CTfS, Exercise 3.4.1.12; see [[Map of Content]], [[Partial Order]].

## Solution 3.4.1.14

[[CTfS Chapter 3 Exercises#Exercise 3.4.1.14|Exercise 3.4.1.14]]

The reflexive–transitive closure: $x \leq y$ iff $x = y$ or $x$ is a descendant of $y$ (child, grandchild, …). Read the other way, $y$ is $x$ or an ancestor of $x$ ([[Reflexive Transitive Closure]], [[Preorder]]). It is in fact a partial order, since nobody is their own ancestor.

> Sources: CTfS, Exercise 3.4.1.14; see [[Map of Content]], [[Preorder]].

## Solution 3.4.4.3

[[CTfS Chapter 3 Exercises#Exercise 3.4.4.3|Exercise 3.4.4.3]]

Order the taxa (species, genera, families, …) by "is a kind of".

- **a.** No. The meet of two taxa would be the most general taxon that is a kind of both. Distinct species (or, say, a cat and a dog) have no common subkind, and taxa at the same level are disjoint.
- **b.** Essentially yes, provided there is a top element such as "life". The join of two taxa is their most specific common ancestor taxon, e.g. the join of *Homo sapiens* and *Pan troglodytes* is the tribe Hominini.
- **c.** The meet is a common refinement and the join is the least common generalization. See [[Join]], [[Meet]], [[Tree of Life]].

> Sources: CTfS, Exercise 3.4.4.3; see [[Join]], [[Map of Content]].

## Solution 3.4.4.7

[[CTfS Chapter 3 Exercises#Exercise 3.4.4.7|Exercise 3.4.4.7]]

- **a.** As posed, $P$ carries no order. The nearby preorder is $(S, \subseteq)$, or $\mathcal P(P)$ with inclusion. One can also preorder people by $p \leq q$ iff $q$ needs to know everything $p$ needs to know.
- **b.** $K(J_2) \subseteq K(J_1)$: needing to know more pieces is a stronger condition. $K$ is antitone.
- **c.** Yes: $K(J_1) \cap K(J_2) = K(J_1 \cup J_2) \in S$, and likewise for arbitrary families, so meets are intersections.
- **d.** Joins exist, since $S$ is closed under arbitrary intersections and contains $K(\varnothing) = P$. But they are not unions. The join of $K(J_1)$ and $K(J_2)$ is $K(J')$, where $J'$ is the set of pieces of information that *everyone* in $K(J_1) \cup K(J_2)$ needs to know. It contains the union $K(J_1) \cup K(J_2)$ and is generally bigger, a closure operation from a [[Galois Connection]]. See [[Join]], [[Meet]], [[Galois Connection]].

> Sources: CTfS, Exercise 3.4.4.7; see [[Join]], [[Map of Content]].

## Solution 3.4.4.11

[[CTfS Chapter 3 Exercises#Exercise 3.4.4.11|Exercise 3.4.4.11]]

- **a.** Yes: if $U \subseteq U'$ then the temperatures recorded in $U$ are among those recorded in $U'$, so $T(U) \subseteq T(U')$. $T$ is monotone.
- **b.** Joins are preserved. The join of two intervals is the smallest interval containing both, and $T(U \cup V)$ is exactly the hull of $T(U) \cup T(V)$: its low is the lower of the two lows and its high the higher of the two highs. Meets are not preserved. $T(U \cap V) \subseteq T(U) \cap T(V)$ always, but the inclusion can be strict: if $U$ ranges over $[0, 10]$ and $V$ over $[5, 20]$, the overlap $U \cap V$ may only see $[6, 7]$. See [[Join]], [[Monotone Map]].

> Sources: CTfS, Exercise 3.4.4.11; see [[Join]], [[Map of Content]].

## Solution 3.5.2.12

[[CTfS Chapter 3 Exercises#Exercise 3.5.2.12|Exercise 3.5.2.12]]

Yes: $f^4 = f^2$ (equivalently $f^{n+2} = f^n$ for all $n \geq 2$). Check it: every element reaches the fixed point $C$ or the 2-cycle $G \leftrightarrow H$ within two steps, and applying $f$ twice more returns the same place, e.g. $F \mapsto G \mapsto H \mapsto G \mapsto H$, so $f^2(F) = H = f^4(F)$. No shorter PED holds. $f^3 \neq f$ fails at $A$ ($f(A) = B$, $f^3(A) = C$). $f^3 \neq f^2$ fails at $G$. $f^2 \neq f$ fails at $A$. $f^2 = \mathrm{id}$ fails at $A$. So the classes are $\mathrm{id}, f, f^2, f^3$: four equivalence classes ([[Discrete Dynamical System]], [[Presentation of a Monoid]]: the cyclic monoid $C_{2,2}$).

> Sources: CTfS, Exercise 3.5.2.12; see [[Discrete Dynamical System]], [[Map of Content]].

## Solution 3.5.2.13

[[CTfS Chapter 3 Exercises#Exercise 3.5.2.13|Exercise 3.5.2.13]]

- **a.** Yes: the set of positions (with side to move) and the function "position ↦ position after $P$'s move" form a set with an endomorphism, i.e. an instance on $\mathrm{Loop}$. Here $P$ plays both sides.
- **b.** Terminal positions (checkmate, stalemate) have no legal move. To keep the function total, let $P$ send them to themselves, so game-ending positions are fixed points. Draws by repetition or by the fifty-move rule depend on the *history*, not just on the position. To model them the state must include the relevant history, such as a move counter and previous positions. Then these rules also become "absorbing" states ([[Discrete Dynamical System]]).

> Sources: CTfS, Exercise 3.5.2.13; see [[Discrete Dynamical System]], [[Map of Content]].

## Solution 3.5.2.18

[[CTfS Chapter 3 Exercises#Exercise 3.5.2.18|Exercise 3.5.2.18]]

$F.f.h \simeq F$: a father's first child's father is that father ($f \mathbin{;} h = \mathrm{id}_F$). No PED holds for $C.h.f$: a child's father's first child is that child only if the child is the firstborn. So $f$ is a section of $h$ ([[Section and Retraction]]). The presented category has five morphisms $\mathrm{id}_F, \mathrm{id}_C, f, h, h \mathbin{;} f$, the last an idempotent on $C$ ([[Database Schema]], [[Presentation of a Category]]).

> Sources: CTfS, Exercise 3.5.2.18; see [[Database Schema]], [[Map of Content]].

## Solution 3.5.3.2

[[CTfS Chapter 3 Exercises#Exercise 3.5.3.2|Exercise 3.5.3.2]]

- **a.** $\{\text{Em1206}, \dots, \text{Em1212}\}$, seven emails.
- **b.** $\{\text{Bob}, \text{Carl}, \text{Chris}, \text{Julia}, \text{Martha}, \text{Sue}\}$.
- **c.** Em1206 ↦ Bob, Em1207 ↦ Carl, Em1208 ↦ Sue, Em1209 ↦ Chris, Em1210 ↦ Chris, Em1211 ↦ Julia, Em1212 ↦ Martha.
- **d.** The PED is $\text{is} \mathbin{;} \text{is sent by} \simeq \text{is} \mathbin{;} \text{is sent to}$ as paths from "a self-email" to "a person". It holds: SEm1207 ↦ Carl both ways, SEm1210 ↦ Chris, SEm1211 ↦ Julia. The self-email table is in fact the [[Equalizer]] of sender and recipient ([[C-Set]], [[Database Schema]]).

> Sources: CTfS, Exercise 3.5.3.2; see [[C-Set]], [[Map of Content]].

## Solution 3.5.3.5

[[CTfS Chapter 3 Exercises#Exercise 3.5.3.5|Exercise 3.5.3.5]]

In a group action every element acts by a bijection, since $g^{-1}$ undoes $g$. So every column of the action table (one per generator) must be a *permutation* of the rows: no repeated entries, and every row ID appears. A column with a repeated entry shows that the action does not factor through a group. Conversely, all-permutation columns are only evidence: the action factors through the permutation group of the rows, but $M$ itself might not be a group (e.g. $\mathbb N$ acting on a finite cycle). See [[Monoid Action]], [[Group Action]].

> Sources: CTfS, Exercise 3.5.3.5; see [[Map of Content]], [[Monoid Action]].
