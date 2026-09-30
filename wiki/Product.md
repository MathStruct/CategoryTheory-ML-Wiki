#definition #example #theorem #proof

Let $X, Y$ be objects of a [[Category]] $\mathcal{C}$. A **product** of $X$ and $Y$ is an object $X \times Y$ with **projections** $p_X : X \times Y \to X$, $p_Y : X \times Y \to Y$ such that for every object $C$ with $f : C \to X$, $g : C \to Y$ there is a *unique* morphism $\langle f, g \rangle : C \to X \times Y$ with $\langle f, g \rangle \mathbin{;} p_X = f$ and $\langle f, g \rangle \mathbin{;} p_Y = g$. A category **has products** if every pair has one.

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}
 & C \arrow[dl, "f"'] \arrow[dr, "g"] \arrow[d, "{\langle f, g \rangle}", dashed] & \\
X & X \times Y \arrow[l, "p_X"] \arrow[r, "p_Y"'] & Y
\end{tikzcd}
\end{document}
```

"$X \times Y$ is the best object-equipped-with-morphisms-to-$X$-and-$Y$: any other such object maps to it uniquely" — a **universal property**; dotted arrows denote the unique morphism.

> Sources: 7 Sketches Definition 3.86, Examples 3.87, 3.89, 3.94, Exercises 3.88, 3.90, 3.91, 3.97; Kittenlab Lecture 13 ("Products and typed products"); DaoFP Chapter 5 ("Product Types", "Cartesian Category", "Tuple Arithmetic"), §9.4 ("Product as a universal span"), §10.2 ("The product adjunction"), §10.5; CTfS §2.4.1 (Definition 2.4.1.1, Lemma 2.4.1.10, Examples 2.4.1.2–2.4.1.18), Definition 4.5.1.8, Examples 4.5.1.2–4.5.1.17

## Examples

- $\mathbf{Set}$: the cartesian product $\{(x, y)\}$ with $p_X(x,y) = x$, $\langle f, g \rangle(c) = (f(c), g(c))$ (Example 3.87; picture of $\underline{6} \times \underline{4}$ as a grid). In $\mathbf{FinSet}$ with $A = \{1..n\}$, $B = \{1..m\}$: $A \times B = \{1..nm\}$ with $k \mapsto (\mathrm{div}(k,m) + 1, \mathrm{rem}(k,m) + 1)$ and $(i, j) \mapsto (i-1)m + j$ — "the same problem as storing a matrix in linear memory" (Kittenlab).
- [[Preorder]]: the product $x \times y$ is the [[Meet]] $x \wedge y$ ([[7S Chapter 3 Exercises#Exercise 3.88|7S Exercise 3.88]]); in $\mathcal{P}(X)$ the intersection (Kittenlab).
- [[Category of Graphs|Graphs]]: $(G \times H)(V) = G(V) \times H(V)$, $(G \times H)(E) = G(E) \times H(E)$ with componentwise sources and targets (Kittenlab Lecture 13); all [[C-Set|C-sets]] likewise, pointwise.
- [[Category of Categories|$\mathbf{Cat}$]]: the [[Product Category]] (Example 3.89); $\mathbf{Preord}$: the [[Product Preorder]].
- Julia: `Tuple{A,B}`; Haskell: `(a, b)` with `fst`, `snd` and `(&&&)` (DaoFP: the **cartesian category** of types).
- In a [[Slice Category]] $\mathcal{C}/T$: the [[Pullback]] $A \times_T B$ ("typed product", Kittenlab).

## Examples from Category Theory for Scientists

- **A grid of dots** (CTfS Example 2.4.1.2): $\underline 6 \times \{\clubsuit, \diamondsuit, \heartsuit, \spadesuit\}$ is a $6 \times 4$ grid, and the projections read off the column and the row. "Suppose each person in a classroom picks an element of $X$ and an element of $Y$ — isn't picking a column and a row the same as picking a point of the grid?" That is the universal property, which we "need to see as completely intuitive" (CTfS Example 2.4.1.11). Punnett squares in genetics are products of the parents' possible genotypes; a tensile test produces points of extension × force.
- **Olog labels** (CTfS §2.4.1.17): the product of boxes $c$ and $d$ is "a pair $(x, y)$ where $x$ is $c$ and $y$ is $d$", with projections "yields, as $x$" and "yields, as $y$". A car owner has as primary car a car; pairing "is a person" with "owns, as primary, a car" gives a map into "a pair (person, car)", which "has as associated utility" a dollar value (CTfS Example 2.4.1.18).
- **Equations** (CTfS Exercise 2.4.1.8): in $\mathbb Z$, the square $(a,b,c) \mapsto (ab, ac) \mapsto ab + ac$ vs. $(a,b,c) \mapsto (a+b, c) \mapsto (a+b)c$ does *not* commute (distributivity is $a(b+c)$, not $(a+b)c$); $x \mapsto (x, 1) \mapsto x \cdot 1$ is the identity but $x \mapsto (x,0) \mapsto x \cdot 0$ is not. The swap $X \times Y \to Y \times X$ is $\langle \pi_2, \pi_1 \rangle$, its own inverse ([[CTfS Chapter 2 Exercises#Exercise 2.4.1.15|CTfS Exercise 2.4.1.15]]); the graph of $f : \mathbb R \to \mathbb R$ is $\langle \mathrm{id}, f \rangle : \mathbb R \to \mathbb R^2$ ([[CTfS Chapter 4 Exercises#Exercise 4.5.1.15|CTfS Exercise 4.5.1.15]]).
- **Preferences** (CTfS Exercise 4.5.1.3): the product of a partial order on songs and one on artworks orders pairs "componentwise" — a reasonable but cautious guess, as it never trades a better song for a worse painting. In $(\mathbb N, \mid)$ the product of $9$ and $12$ is $\gcd = 3$. Products need not exist (two rays through the origin have no meet) and need not be unique (all points of a circle can be products), but are always unique up to unique isomorphism (CTfS Examples 4.5.1.11–4.5.1.12).
- **In $\mathbf{Cat}$**: $[1] \times [1]$ is the commutative square with 9 morphisms — "it is a minor miracle that the categorical product somehow knows that this square should commute" (CTfS Example 4.5.1.17).

## Three descriptions

1. **Universal cone**: a product is a [[Terminal Object]] in the category $\mathrm{Cone}(X, Y)$ of spans $X \leftarrow C \to Y$ ([[7S Chapter 3 Exercises#Exercise 3.91|7S Exercise 3.91]]); hence a [[Limit]] of the [[Diagram]] $\bullet\ \bullet$ indexed by the discrete two-object category (Example 3.94), and unique up to unique isomorphism.
2. **Representability**: $\mathcal{C}(-, X \times Y) \cong \mathcal{C}(-, X) \times \mathcal{C}(-, Y)$, i.e. $[\mathbf{2}, \mathcal{C}](\Delta_x, D) \cong \mathcal{C}(x, a \times b)$ — "product as a universal span" (DaoFP §9.4); naturality of the isomorphism encodes the commuting triangles.
3. **Adjunction**: $\Delta \dashv (\times)$ with the [[Diagonal Functor]]; the counit is $\langle \mathsf{fst}, \mathsf{snd} \rangle$ (DaoFP §10.2, 10.5).

## Properties (DaoFP Chapter 5)

- **Functoriality**: $f \times g := \langle p_1 \mathbin{;} f, p_2 \mathbin{;} g \rangle$ makes $\times$ a [[Bifunctor]] (`bimap`).
- **Tuple arithmetic**: $a \times b \cong b \times a$ (symmetry), $(a \times b) \times c \cong a \times (b \times c)$ (associativity), $1 \times a \cong a$ (unit) — a category with all finite products is a [[Cartesian Category]], a special [[Symmetric Monoidal Category]]; $0 \times a \cong 0$ in a [[Cartesian Closed Category|CCC]].
- **Logic**: $a \times b$ is conjunction $A \wedge B$ (Curry–Howard: a proof of both). **Records** are named products.
- **Duality**: the [[Coproduct]] is the product in $\mathcal{C}^{\mathrm{op}}$ ("we can flip all arrows in the definition"); [[Exponential Object|exponentials]] are right adjoint to $(- \times a)$ ([[Currying]]); the product distributes over sums in a [[Bicartesian Closed Category|bicartesian closed category]].

````tabs
tab: Julia
**Docs:** [FinSets](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.FinSets) · [Limits & colimits](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.Limits) — Kittenlab [Lecture 13](https://algebraicjulia.github.io/Kittenlab.jl/lecture13.html)
```julia
# Kittenlab Lecture 13: products in skeletal FinSet by index arithmetic
struct FinSet′; n::Int end
product_set(A::FinSet′, B::FinSet′) = FinSet′(A.n * B.n)
proj1(A, B) = k -> div(k - 1, B.n) + 1
proj2(A, B) = k -> rem(k - 1, B.n) + 1
pair_index(A, B) = (i, j) -> (i - 1) * B.n + j
```
Catlab version (run in a fresh Julia session — Catlab exports its own `compose`, `id`, `FinFunction`, …):
```julia
# Catlab
using Catlab
P = product(FinSet(2), FinSet(3))
apex(P)                      # FinSet(6)
π1, π2 = legs(P)             # projections
f = FinFunction([1, 2, 1], 2); g = FinFunction([3, 3, 2], 3)
h = pair(P, f, g)            # ⟨f, g⟩ : FinSet(3) → FinSet(6)
force(compose(h, π1)) == f && force(compose(h, π2)) == g     # true
```
tab: Lean
```lean
#check CategoryTheory.Limits.prod          -- X ⨯ Y (notation), with prod.fst, prod.snd, prod.lift
#check CategoryTheory.Limits.prod.lift     -- ⟨f, g⟩
#check CategoryTheory.Limits.prod.lift_fst
#check CategoryTheory.Limits.BinaryFan     -- cones over a pair
-- in Type: X ⨯ Y ≅ X × Y
#check CategoryTheory.Limits.Types.binaryProductIso
```
tab: Haskell
```haskell
-- DaoFP Chapter 5: the product type with projections and the universal pairing
fst' :: (a, b) -> a
fst' (a, _) = a
snd' :: (a, b) -> b
snd' (_, b) = b

pairing :: (c -> a) -> (c -> b) -> (c -> (a, b))       -- ⟨f, g⟩, Control.Arrow's (&&&)
pairing f g c = (f c, g c)

bimapP :: (a -> a') -> (b -> b') -> (a, b) -> (a', b')   -- functoriality
bimapP f g (a, b) = (f a, g b)
```
````
