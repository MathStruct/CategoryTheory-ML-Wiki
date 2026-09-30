#solution

Solutions to the exercises of Category Theory for Scientists, Chapter 2 (written for this wiki; the book has none): [[CTfS Chapter 2 Exercises]]. Index: [[Map of Content]].

## Solution 2.1.2.2

[[CTfS Chapter 2 Exercises#Exercise 2.1.2.2|Exercise 2.1.2.2]]

- **a.** A function $PR \to RG$: every photoreceptor connects to exactly one ganglion cell (so the assignment is total and single-valued), while a ganglion cell may receive many photoreceptors (so the reverse assignment is not single-valued). It is typically far from injective — this is *convergence*, information compression.
- **b.** Generally not. Neurons in the cortex have many outgoing and incoming connections, so a connection pattern between two areas is a [[Relation]] (a span $A \leftarrow C \to B$ of connections), not a function. The retina's many-to-one wiring is special.

> Sources: CTfS, Exercise 2.1.2.2; see [[Function]], [[Map of Content]].

## Solution 2.1.2.5

[[CTfS Chapter 2 Exercises#Exercise 2.1.2.5|Exercise 2.1.2.5]]

A function is an independent choice of output for each input, so $|\mathrm{Hom}(A, B)| = |B|^{|A|}$.

- **a.** $2^5 = 32$.
- **b.** $5^2 = 25$.

> Sources: CTfS, Exercise 2.1.2.5; see [[Function]], [[Map of Content]].

## Solution 2.1.2.10

[[CTfS Chapter 2 Exercises#Exercise 2.1.2.10|Exercise 2.1.2.10]]

- **a.** $n!$: an isomorphism is a bijection (a permutation); the image of the first element has $n$ choices, the second $n - 1$, and so on.
- **b.** Yes. For $X = \varnothing$ there is exactly one function $\varnothing \to \varnothing$, the empty function, and it is the identity, hence an isomorphism; and $0! = 1$.

> Sources: CTfS, Exercise 2.1.2.10; see [[Cardinality]], [[Map of Content]].

## Solution 2.1.2.13

[[CTfS Chapter 2 Exercises#Exercise 2.1.2.13|Exercise 2.1.2.13]]

$A = \{\ast\}$, any one-element set. A function $\{\ast\} \to X$ is determined by the single element it picks out, so $x \mapsto (\ast \mapsto x)$ is a bijection $X \cong \mathrm{Hom}(\{\ast\}, X)$. These functions are the [[Global Element|global elements]] of $X$. No other $A$ works: $A = \varnothing$ gives $\mathrm{Hom}(\varnothing, X) \cong \{\ast\}$ for every $X$, and $|A| = k \geq 2$ gives $|X|^k \neq |X|$ for $|X| = 2$.

> Sources: CTfS, Exercise 2.1.2.13; see [[Global Element]], [[Map of Content]].

## Solution 2.4.1.15

[[CTfS Chapter 2 Exercises#Exercise 2.4.1.15|Exercise 2.4.1.15]]

- **a.** To map into $Y \times X$ we need a map to $Y$ and a map to $X$; take $\pi_2 : X \times Y \to Y$ and $\pi_1 : X \times Y \to X$. The universal property gives $s = \langle \pi_2, \pi_1 \rangle$, i.e. $s(x, y) = (y, x)$.
- **b.** Yes. Let $s' = \langle \pi'_2, \pi'_1 \rangle : Y \times X \to X \times Y$. Then $\pi_1 \circ s' \circ s = \pi'_2 \circ s = \pi_1$ and $\pi_2 \circ s' \circ s = \pi'_1 \circ s = \pi_2$. The identity $\mathrm{id}_{X \times Y}$ also satisfies these two equations, and by the *uniqueness* part of the universal property there is only one map with given components; hence $s' \circ s = \mathrm{id}$. Symmetrically $s \circ s' = \mathrm{id}$. See [[Product]], [[Universal Property]].

> Sources: CTfS, Exercise 2.4.1.15; see [[Map of Content]], [[Product]].

## Solution 2.4.2.13

[[CTfS Chapter 2 Exercises#Exercise 2.4.2.13|Exercise 2.4.2.13]]

Olog: $\langle$a particle$\rangle \xrightarrow{\text{is}} \langle$a particle or a wave$\rangle \xleftarrow{\text{is}} \langle$a wave$\rangle$, where the middle box is the [[Coproduct]] $P \sqcup W$. A photon, being both a particle and a wave, appears in $P \sqcup W$ *twice* — once as "a photon viewed as a particle" and once as "a photon viewed as a wave" — because the coproduct is a disjoint union. If one wants each physical object once, the right construction is the [[Pushout]] $P \sqcup_{B} W$ over $B = \langle$a thing that is both a particle and a wave$\rangle$ (with $B \to P$, $B \to W$ labelled "is"), which glues the two copies of each photon together. The exercise illustrates that the coproduct is the right notion exactly when the two types are *disjoint* or when one wants to keep track of the viewpoint.

> Sources: CTfS, Exercise 2.4.2.13; see [[Coproduct]], [[Map of Content]].

## Solution 2.5.1.3

[[CTfS Chapter 2 Exercises#Exercise 2.5.1.3|Exercise 2.5.1.3]]

Take $X = \{x_1, \dots, x_5\}$ coloured $r, r, b, y, b$ and $Y = \{y_1, y_2, y_3\}$ coloured $b, r, b$.

- **a.** $X \times_C Y = \{(x, y) \mid \mathrm{col}(x) = \mathrm{col}(y)\}$: the red pairs $(x_1, y_2), (x_2, y_2)$ and the blue pairs $(x_3, y_1), (x_3, y_3), (x_5, y_1), (x_5, y_3)$ — six elements, coloured by their common colour. Yellow contributes nothing because $Y$ has no yellow element.
- **b.** In the grid (rows $x_i$, columns $y_j$) mark the cells where row colour equals column colour:

| | $y_1$ (b) | $y_2$ (r) | $y_3$ (b) |
|---|---|---|---|
| $x_1$ (r) | | ● | |
| $x_2$ (r) | | ● | |
| $x_3$ (b) | ● | | ● |
| $x_4$ (y) | | | |
| $x_5$ (b) | ● | | ● |

In general $|X \times_C Y| = \sum_{c \in C} |X_c| \cdot |Y_c|$, a sum of products of fiber sizes ([[Pullback]]).

> Sources: CTfS, Exercise 2.5.1.3; see [[Map of Content]], [[Pullback]].

## Solution 2.5.1.5

[[CTfS Chapter 2 Exercises#Exercise 2.5.1.5|Exercise 2.5.1.5]]

- **a.** It is empty: its elements are pairs $(x, y)$ with $y \in Y = \varnothing$.
- **b.** It is (isomorphic to) the product $X \times Y$: every pair satisfies $f(x) = g(y)$ since $Z$ has only one element. So the product is the pullback over the [[Terminal Object]] ([[Finite Limits in Set]]).

> Sources: CTfS, Exercise 2.5.1.5; see [[Finite Limits in Set]], [[Map of Content]].

## Solution 2.5.1.6

[[CTfS Chapter 2 Exercises#Exercise 2.5.1.6|Exercise 2.5.1.6]]

- **a.** $W_1 = \{(\ast, (s, t)) \mid s = 0\} \cong \{0\} \times T \cong \mathbb R$ and $W_2 = \{(\ast, (s, t)) \mid t = 0\} \cong S \times \{0\} \cong \mathbb R^3$.
- **b.** $W_1$ is the set of all space-time points *at the place* of MIT's founding centre of mass, at every time — a world-line (in Aristotelian, absolute space). $W_2$ is all of space *at the instant* of the founding — a time-slice. See [[Finite Limits in Set]], [[Fiber]].

> Sources: CTfS, Exercise 2.5.1.6; see [[Finite Limits in Set]], [[Map of Content]].

## Solution 2.5.1.10

[[CTfS Chapter 2 Exercises#Exercise 2.5.1.10|Exercise 2.5.1.10]]

- **a.** Reasonable: the pullback consists of pairs (person, blue) with the person's favourite colour equal to blue, i.e. persons whose favourite colour is blue.
- **b.** Reasonable, for the same reason: pairs (dog, woman) with the dog's owner equal to that woman, i.e. dogs whose owner is a woman.
- **c.** Misleading. The pullback is the set of pairs (space, piece of furniture) *of equal width*. Equal width is neither necessary nor sufficient for a good fit (depth and height matter, and a piece narrower than the space may fit fine). A correct label is "a space in our house and a piece of furniture of the same width". Labels of limits must describe exactly the set the construction produces ([[Pullback]], [[Olog]]).

> Sources: CTfS, Exercise 2.5.1.10; see [[Map of Content]], [[Pullback]].

## Solution 2.5.3.3

[[CTfS Chapter 2 Exercises#Exercise 2.5.3.3|Exercise 2.5.3.3]]

Let $A$ be "a published author" and $P$ "a paper", with $f : A \to P$ "has as first paper" and $g : A \to P$ "has as most recent paper". The [[Equalizer]] $\{a \in A \mid f(a) = g(a)\}$ is "an author whose first paper is their most recent paper", i.e. an author with exactly one paper (assuming papers are totally ordered in time with no ties). Another example: for an experiment, "an input" with $f$ "yields as predicted output" and $g$ "yields as measured output"; the equalizer is "an input on which theory and experiment agree".

> Sources: CTfS, Exercise 2.5.3.3; see [[Equalizer]], [[Map of Content]].

## Solution 2.6.1.3

[[CTfS Chapter 2 Exercises#Exercise 2.6.1.3|Exercise 2.6.1.3]]

None of the three in general. (a) Not everyone thinks a lot about themselves. (b) Unrequited attention is common: a fan thinks about a celebrity, not vice versa. (c) If $x$ thinks about $y$ and $y$ thinks about $z$, $x$ need not think about $z$. So $R$ is merely a [[Relation]], far from an [[Equivalence Relation]].

> Sources: CTfS, Exercise 2.6.1.3; see [[Equivalence Relation]], [[Map of Content]].

## Solution 2.6.1.5

[[CTfS Chapter 2 Exercises#Exercise 2.6.1.5|Exercise 2.6.1.5]]

Yes: $f(x) = f(x)$; $f(x) = f(y) \Rightarrow f(y) = f(x)$; and equality is transitive. Its classes are the nonempty [[Fiber|fibers]] of $f$.

- **a.** Yes. Given an equivalence relation $\sim$, take $f : X \to X/\!\sim$, the quotient map $x \mapsto [x]$; then $f(x) = f(y)$ iff $x \sim y$.
- **b.** Yes: the fibers of $f$ form a [[Partition]] of $X$. Equivalence relations, partitions and surjections out of $X$ (up to isomorphism of the codomain) are three presentations of the same data ([[Quotient Set]]).

> Sources: CTfS, Exercise 2.6.1.5; see [[Equivalence Relation]], [[Map of Content]].

## Solution 2.6.1.10

[[CTfS Chapter 2 Exercises#Exercise 2.6.1.10|Exercise 2.6.1.10]]

- **a.** $x \sim y$ iff $x$ and $y$ are joined by a (possibly empty) path of edges, traversed in either direction: "being in the same connected component".
- **b.** The set of connected components of the network ([[Equivalence Relation]], [[Coequalizer]]).

> Sources: CTfS, Exercise 2.6.1.10; see [[Equivalence Relation]], [[Map of Content]].

## Solution 2.6.2.6

[[CTfS Chapter 2 Exercises#Exercise 2.6.2.6|Exercise 2.6.2.6]]

$f$ is a bijection from $\mathbb N$ onto the negative integers $\{-1, -2, \dots\}$. The pushout takes $\mathbb Z \sqcup \{\ast\}$ and identifies each $f(w)$ with $g(w) = \ast$, so all negative integers are glued to the single point $\ast$. Result: $\{\ast, 0, 1, 2, \dots\} \cong \mathbb N$ — "the non-negative integers with one extra point standing for all the negatives" ([[Pushout]], [[Finite Colimits in Set]]).

> Sources: CTfS, Exercise 2.6.2.6; see [[Finite Colimits in Set]], [[Map of Content]].

## Solution 2.6.2.7

[[CTfS Chapter 2 Exercises#Exercise 2.6.2.7|Exercise 2.6.2.7]]

- **a.** The pushout is $(X \sqcup X)/\!\approx$ where $\approx$ is generated by identifying $x$ in the left copy with $y$ in the right copy whenever $(x, y) \in R$. Since $R$ is reflexive, every $x_{\text{left}}$ is identified with $x_{\text{right}}$, so the two copies collapse into one; then the remaining identifications are exactly $\sim$. The pushout is $X/\!\sim$.
- **b.** In general the pushout is $(X \sqcup X)/\!\approx$ with $x_\text{left} \approx y_\text{right}$ for $(x, y) \in R$; it need not glue the two copies together (e.g. $R = \varnothing$ gives $X \sqcup X$). Its relationship to the generated equivalence relation $\overline R$: the *coequalizer* of the two maps $R \rightrightarrows X$ is exactly $X / \overline R$, and if $R$ is reflexive the pushout equals this coequalizer. See [[Equivalence Relation]], [[Pushout]].

> Sources: CTfS, Exercise 2.6.2.7; see [[Equivalence Relation]], [[Map of Content]], [[Pushout]].

## Solution 2.6.3.2

[[CTfS Chapter 2 Exercises#Exercise 2.6.3.2|Exercise 2.6.3.2]]

The quotient $\mathbb R / (x \sim x + 1)$, i.e. $\mathbb R/\mathbb Z$, which can be represented by $[0, 1)$ with wrap-around: a circle. Every real number is identified with its fractional part ([[Coequalizer]], [[Finite Colimits in Set]]).

> Sources: CTfS, Exercise 2.6.3.2; see [[Finite Colimits in Set]], [[Map of Content]].

## Solution 2.7.1.2

[[CTfS Chapter 2 Exercises#Exercise 2.7.1.2|Exercise 2.7.1.2]]

$X$ = "a US state", $Y$ = "a US city", $f$: "has as capital", $g$: "is located in". Then $g \circ f = \mathrm{id}_X$: the capital of a state lies in that state. But $f \circ g \neq \mathrm{id}_Y$: Boston ↦ Massachusetts ↦ Boston is fine, but Cambridge ↦ Massachusetts ↦ Boston $\neq$ Cambridge. So $f$ is a section (a [[Monomorphism]]) and $g$ a retraction (an [[Epimorphism]]), neither an isomorphism ([[Section and Retraction]]).

> Sources: CTfS, Exercise 2.7.1.2; see [[Map of Content]], [[Section and Retraction]].

## Solution 2.7.2.2

[[CTfS Chapter 2 Exercises#Exercise 2.7.2.2|Exercise 2.7.2.2]]

Yes. A function $A \to B$ is an independent choice of an element of $B$ for each of the $|A|$ elements of $A$. Edge cases: $A = \varnothing$ gives exactly one function (the empty function), and $|B|^0 = 1$; $A \neq \varnothing$, $B = \varnothing$ gives no functions, and $0^{|A|} = 0$; $A = B = \varnothing$ gives one function and $0^0 = 1$ ([[Arithmetic of Sets]], [[Exponential Object]]).

> Sources: CTfS, Exercise 2.7.2.2; see [[Arithmetic of Sets]], [[Map of Content]].

## Solution 2.7.2.5

[[CTfS Chapter 2 Exercises#Exercise 2.7.2.5|Exercise 2.7.2.5]]

- **a.** Uncurrying a function $h : X \to B^A$ gives $(x, a) \mapsto h(x)(a)$. For $h = \mathrm{id}$ this is $\mathrm{ev}(f, a) = f(a)$.
- **b.** It *evaluates* a function at an argument. It is the counit of the adjunction $- \times A \dashv (-)^A$ and the universal arrow defining the [[Exponential Object]]: every $X \times A \to B$ factors uniquely as $\mathrm{ev} \circ (\tilde h \times \mathrm{id}_A)$ ([[Currying]]).

> Sources: CTfS, Exercise 2.7.2.5; see [[Currying]], [[Map of Content]].

## Solution 2.7.2.6

[[CTfS Chapter 2 Exercises#Exercise 2.7.2.6|Exercise 2.7.2.6]]

$\underline 2 = \{1, 2\} \cong \underline 1 \sqcup \underline 1$. Maps out of a coproduct are pairs of maps: $\mathrm{Hom}(\underline 1 \sqcup \underline 1, \mathbb R) \cong \mathrm{Hom}(\underline 1, \mathbb R) \times \mathrm{Hom}(\underline 1, \mathbb R)$. By Exercise 2.1.2.13, $\mathrm{Hom}(\underline 1, \mathbb R) \cong \mathbb R$. Hence $\mathbb R^{\underline 2} \cong \mathbb R^{\underline 1 \sqcup \underline 1} \cong \mathbb R^{\underline 1} \times \mathbb R^{\underline 1} \cong \mathbb R \times \mathbb R$. Concretely, $v \mapsto (v(1), v(2))$ ([[Arithmetic of Sets]], [[Currying]]).

> Sources: CTfS, Exercise 2.7.2.6; see [[Arithmetic of Sets]], [[Currying]], [[Map of Content]].

## Solution 2.7.3.2

[[CTfS Chapter 2 Exercises#Exercise 2.7.3.2|Exercise 2.7.3.2]]

$0^0 = \mathrm{Hom}(\varnothing, \varnothing)$, which has exactly one element, the empty function (= $\mathrm{id}_\varnothing$). So $0^0 \cong 1$. The rule $0^A \cong 0$ holds only for $A \neq \varnothing$: if $A$ has an element, there is nowhere to send it ([[Arithmetic of Sets]], [[Cardinality]]).

> Sources: CTfS, Exercise 2.7.3.2; see [[Arithmetic of Sets]], [[Cardinality]], [[Map of Content]].

## Solution 2.7.3.3

[[CTfS Chapter 2 Exercises#Exercise 2.7.3.3|Exercise 2.7.3.3]]

Yes. If $A \times B \cong \varnothing$ but $A$ and $B$ were both nonempty, pick $a \in A$, $b \in B$; then $(a, b) \in A \times B$, a contradiction. So $A \cong \varnothing$ or $B \cong \varnothing$ ([[Arithmetic of Sets]]). (In other categories this can fail: in $\mathbf{Vect}$ the product of nonzero spaces is nonzero, but "zero divisors" appear e.g. for the tensor product of abelian groups, $\mathbb Z/2 \otimes \mathbb Z/3 = 0$.)

> Sources: CTfS, Exercise 2.7.3.3; see [[Arithmetic of Sets]], [[Map of Content]].

## Solution 2.7.4.7

[[CTfS Chapter 2 Exercises#Exercise 2.7.4.7|Exercise 2.7.4.7]]

$X_0 = \{\{1\}, \{2\}, \{3\}\}$, $X_1 = \{\{1, 2\}, \{1, 3\}, \{2, 3\}\}$, and $X_n = \varnothing$ for $n \geq 2$ (the 2-simplex $\{1,2,3\}$ is omitted: the triangle is not filled in). This family is downward closed and contains all atoms ([[Simplicial Complex]]).

> Sources: CTfS, Exercise 2.7.4.7; see [[Map of Content]], [[Simplicial Complex]].

## Solution 2.7.4.12

[[CTfS Chapter 2 Exercises#Exercise 2.7.4.12|Exercise 2.7.4.12]]

- **a.** $\chi_{A_2}(a) = \text{true}$ iff $f(a) = \text{false}$.
- **b.** $\chi_{A_2} = \neg \circ f$, where $\neg : \Omega \to \Omega$ swaps true and false. Complement of subsets corresponds to negation on the [[Subobject Classifier]] ([[Booleans]], [[Power Set]]).

> Sources: CTfS, Exercise 2.7.4.12; see [[Booleans]], [[Map of Content]].

## Solution 2.7.5.6

[[CTfS Chapter 2 Exercises#Exercise 2.7.5.6|Exercise 2.7.5.6]]

Let $f : A \to B$ be an epimorphism, $g : A \to C$ any map, and form the pushout $D = B \sqcup_A C$ with $i : B \to D$, $f' : C \to D$ (so $f' g = i f$). Claim: $f'$ is an epimorphism. Suppose $h, k : D \to E$ with $h f' = k f'$. Then $h i f = h f' g = k f' g = k i f$, and since $f$ is epi, $h i = k i$. Now $h$ and $k$ agree after composing with both pushout injections $i$ and $f'$, so by the uniqueness in the universal property of the pushout, $h = k$. $\blacksquare$ (This is the formal dual of the pullback argument; in $\mathbf{Set}$ it also follows from epi = surjective, [[Epimorphism]], [[Pushout]].)

> Sources: CTfS, Exercise 2.7.5.6; see [[Epimorphism]], [[Map of Content]].

## Solution 2.7.6.4

[[CTfS Chapter 2 Exercises#Exercise 2.7.6.4|Exercise 2.7.6.4]]

Example: $E = \{e\}$, $B = \{x, y\}$, $\pi(e) = x$ — the name $y$ has no instance.

- **a.** In a multiset every name has multiplicity $\geq 1$; a pseudo-multiset allows multiplicity $0$.
- **b.** Pseudo-multisets are usually more useful: they are exactly the relative sets over $B$, i.e. objects of the [[Slice Category]] $\mathbf{Set}_{/B}$, which has good categorical properties (limits, colimits; it is a [[Topos]]), and they model "bags over a fixed vocabulary", like word counts, where most words occur 0 times ([[Multiset]]).

> Sources: CTfS, Exercise 2.7.6.4; see [[Map of Content]], [[Multiset]].

## Solution 2.7.6.5

[[CTfS Chapter 2 Exercises#Exercise 2.7.6.5|Exercise 2.7.6.5]]

- **a.** $X$: $E = \{e_1, e_2, e_3, e_4\}$, $B = \{1, 2, 3\}$, $\pi = (e_1, e_2 \mapsto 1, e_3 \mapsto 2, e_4 \mapsto 3)$. $Y$: $E' = \{d_1, d_2, d_3, d_4\}$, $B' = \{a, b\}$, $\pi' = (d_1 \mapsto a;\ d_2, d_3, d_4 \mapsto b)$.
- **b.** Choose $f_0 : \{1, 2, 3\} \to \{a, b\}$ ($8$ ways); then each instance $e$ must go into the fiber of $f_0(\pi(e))$, which has 1 element over $a$ and 3 over $b$. Name $1$ has two instances, so the count is $\sum_{f_0} m(f_0(1))^2 \, m(f_0(2)) \, m(f_0(3)) = (1 + 9)(1 + 3)(1 + 3) = 160$ mappings.
- **c.** Any $f_1 : E \to E'$ and any $f_0 : B \to B'$: $4^4 \cdot 2^3 = 2048$.

See [[Multiset]].

> Sources: CTfS, Exercise 2.7.6.5; see [[Map of Content]], [[Multiset]].

## Solution 2.7.6.8

[[CTfS Chapter 2 Exercises#Exercise 2.7.6.8|Exercise 2.7.6.8]]

Yes: the composite function $g \circ f : X \to Z$. It is a mapping over $B$ because $r \circ (g \circ f) = (r \circ g) \circ f = q \circ f = p$ — paste the two commuting triangles. Identities are identity functions. This makes relative sets over $B$ a category, the [[Slice Category]] $\mathbf{Set}_{/B}$ ([[Multiset]]).

> Sources: CTfS, Exercise 2.7.6.8; see [[Map of Content]], [[Multiset]].

## Solution 2.7.6.9

[[CTfS Chapter 2 Exercises#Exercise 2.7.6.9|Exercise 2.7.6.9]]

- **a.** None, essentially: every set has exactly one function to $\{\ast\}$, and every function commutes with these maps, so $\mathbf{Set}_{/\{\ast\}} \cong \mathbf{Set}$ ([[Terminal Object]]).
- **b.** A function $E \to \varnothing$ exists only if $E = \varnothing$, so there is exactly one set over $\varnothing$, namely $(\varnothing, \mathrm{id})$, and $\mathbf{Set}_{/\varnothing}$ is the terminal category ([[Multiset]]).

> Sources: CTfS, Exercise 2.7.6.9; see [[Map of Content]], [[Multiset]].

## Solution 2.7.6.14

[[CTfS Chapter 2 Exercises#Exercise 2.7.6.14|Exercise 2.7.6.14]]

They are equivalent: $\mathbf{Set}^A \simeq \mathbf{Set}_{/A}$. An indexed set $\{S_a\}$ gives the relative set $\coprod_a S_a \to A$, $(a, s) \mapsto a$; a relative set $\pi : E \to A$ gives the indexed set of [[Fiber|fibers]] $\{\pi^{-1}(a)\}$. Mappings correspond (families of fiberwise maps ↔ maps over $A$), and the two constructions are mutually inverse up to isomorphism. See [[Indexed Set]], [[Dependent Type]] ($\Sigma$-types vs type families).

> Sources: CTfS, Exercise 2.7.6.14; see [[Dependent Type]], [[Indexed Set]], [[Map of Content]].
