#definition #example

**$\mathbf{Rel}$** is the [[Category]] whose objects are [[Set|sets]] and whose morphisms $X \to Y$ are [[Relation|relations]] $R \subseteq X \times Y$; composition is

$$
S \circ R := \{(x, z) \mid \exists y \in Y.\ (x, y) \in R \wedge (y, z) \in S\}
$$

and the identity on $X$ is $1_X = \{(x, x)\}$ (Kittenlab Lecture 14, 7 Sketches Example 5.8). Restricted to $\underline{m}, \underline{n}$ it is a [[Prop]] with monoidal product $R_1 + R_2 := R_1 \sqcup R_2 \subseteq (m_1 \sqcup m_2) \times (n_1 \sqcup n_2)$ ("no interaction", [[7S Chapter 5 Exercises#Exercise 5.10|7S Exercise 5.10]]).

> Sources: Kittenlab Lecture 14 ("Relations", "Category of relations"), 15 (relations as [[Span|spans]]); 7 Sketches Example 5.8, Definition 5.79 ($\mathbf{Rel}_R$), Theorem 5.87, §2.6 (relations form a noncommutative quantale), Exercise 5.10; DaoFP §17.1 (profunctors as proof-relevant relations); CTfS Definition 3.3.3.8, Exercises 3.3.3.10–3.3.3.12, 5.3.3.5–5.3.3.7

- Composition is [[Matrix Multiplication in a Quantale|matrix multiplication]] of Boolean matrices ("replace `any` with `sum` and `&&` with `*`"); $\mathbf{Rel}$ is the category of $\mathbf{Bool}$-[[Profunctor|profunctors]] between discrete $\mathbf{Bool}$-categories, and $\mathbf{Bool}$-matrices ([[Category of Profunctors]]).
- $\mathbf{FinSet} \to \mathbf{Rel}$, $f \mapsto \mathrm{graph}(f)$, is a faithful [[Prop]] functor (Example 5.12); a relation is a [[Span]] $X \leftarrow R \to Y$ (Kittenlab Lecture 15), and spans compose by [[Pullback]].
- $\mathbf{Rel}$ is [[Compact Closed Category|compact closed]] with every object self-dual (both under $\sqcup$ and $\times$); $\mathbf{Rel}_R$ (relations $B \subseteq R^m \times R^n$ over a rig, with $B + C := \{(w,y,x,z) \mid (w,x) \in B, (y,z) \in C\}$, [[7S Chapter 5 Exercises#Exercise 5.80|7S Exercise 5.80]]) is compact closed with cup $\{(0, (x,x))\}$ and cap $\{((x,x), 0)\}$ (Theorem 5.87), and contains $\mathbf{Mat}(R)$ via behaviours ([[Graphical Linear Algebra]]).
- Relations on a fixed set form a noncommutative [[Quantale]] under composition (7 Sketches §2.6). In a [[Topos]], relations are subobjects of products and [[Dagger Category|dagger]] structure is transposition $R^\dagger = \{(y,x)\}$.
- **Relations are the Kleisli arrows of the power set monad** (CTfS Exercise 5.3.3.5): a function $A \to \mathcal{P}(B)$ is the same as a relation $R \subseteq A \times B$, and Kleisli composition (apply, then take the union) is relational composition; so $\mathbf{Rel} \cong \mathrm{Kl}(\mathcal{P})$ ([[Power Set Monad]]). In $\mathbf{Rel}$ the disjoint union $A \sqcup B$ is both the product and the coproduct of $A$ and $B$ (CTfS Exercises 5.3.3.6–5.3.3.7): e.g. for $A = \{1,2,3\}$, $B = \{a, b\}$ both are $\{1,2,3,a,b\}$.
- **Relations vs. graphs** (CTfS Exercises 3.3.3.10–3.3.3.12): a relation $R \subseteq S \times S$ is a graph with vertices $S$ and arrows $R$ (source and target the two projections); a graph gives a relation by taking the image of $(\mathrm{src}, \mathrm{tgt}) : A \to V \times V$. Relation → graph → relation is the identity, graph → relation → graph collapses parallel arrows. E.g. $\leq$ on $\{0,1,2,3\}$ draws a tetrahedron with a loop at every vertex, while "$|n - m| \leq 1$" is not transitive and "$n = 5m$" is not reflexive — neither is a preorder.
- Kittenlab's behavioural view: a relation is a *joint constraint*; a mathematical model "selects a subset of a universum of possibilities" (Willems).

````tabs
tab: Julia
**Docs:** [FinRelations](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.FinRelations) — Kittenlab [Lecture 14](https://algebraicjulia.github.io/Kittenlab.jl/lecture14.html), [Lecture 15](https://algebraicjulia.github.io/Kittenlab.jl/lecture15.html)
```julia
# Kittenlab Lecture 14
const BoolRel = BitMatrix                      # R[i, j] = "i is related to j"
relcompose(R::BoolRel, S::BoolRel) =
  BoolRel([any(R[i, j] && S[j, k] for j in axes(R, 2)) for i in axes(R, 1), k in axes(S, 2)])
idrel(n) = BoolRel([i == j for i in 1:n, j in 1:n])
graph_of(f::Vector{Int}, n) = BoolRel([f[i] == j for i in eachindex(f), j in 1:n])   # FinSet → Rel
relcompose(graph_of([2, 3, 3], 3), graph_of([1, 1, 2], 2)) == graph_of([1, 2, 2], 2)  # functorial: true
```
Catlab version (run in a fresh Julia session — Catlab exports its own `compose`, `id`, `FinFunction`, …):
```julia
# Catlab (checked with v0.16): the category FinRel
using Catlab, Catlab.CategoricalAlgebra.FinRelations
R = FinRelation((x, y) -> x < y, 3, 3); S = compose(R, R); S(1, 3)   # true: 1 < 2 < 3
```
tab: Lean
```lean
#check CategoryTheory.RelCat          -- the category of types and relations
#check @Rel.comp
example (α : Type) : Rel α α := fun a b => a = b     -- identity relation
```
tab: Haskell
```haskell
type Rel a b = a -> b -> Bool
compRel :: [b] -> Rel a b -> Rel b c -> Rel a c
compRel ys r s x z = any (\y -> r x y && s y z) ys
idRel :: Eq a => Rel a a
idRel = (==)
plusRel :: Rel a b -> Rel c d -> Rel (Either a c) (Either b d)   -- monoidal product in the prop Rel
plusRel r _ (Left a)  (Left b)  = r a b
plusRel _ s (Right c) (Right d) = s c d
plusRel _ _ _ _ = False
```
````
