#definition #example

**$\mathbf{Rel}$** is the [[Category]] whose objects are [[Set|sets]] and whose morphisms $X \to Y$ are [[Relation|relations]] $R \subseteq X \times Y$; composition is
$$S \circ R := \{(x, z) \mid \exists y \in Y.\ (x, y) \in R \wedge (y, z) \in S\}$$
and the identity on $X$ is $1_X = \{(x, x)\}$ (Kittenlab Lecture 14, 7 Sketches Example 5.8). Restricted to $\underline{m}, \underline{n}$ it is a [[Prop]] with monoidal product $R_1 + R_2 := R_1 \sqcup R_2 \subseteq (m_1 \sqcup m_2) \times (n_1 \sqcup n_2)$ ("no interaction", [[7S Exercise 5.10]]).

> Sources: Kittenlab Lecture 14 ("Relations", "Category of relations"), 15 (relations as [[Span|spans]]); 7 Sketches Example 5.8, Definition 5.79 ($\mathbf{Rel}_R$), Theorem 5.87, §2.6 (relations form a noncommutative quantale), Exercise 5.10; DaoFP §17.1 (profunctors as proof-relevant relations).

- Composition is [[Matrix Multiplication in a Quantale|matrix multiplication]] of Boolean matrices ("replace `any` with `sum` and `&&` with `*`"); $\mathbf{Rel}$ is the category of $\mathbf{Bool}$-[[Profunctor|profunctors]] between discrete $\mathbf{Bool}$-categories, and $\mathbf{Bool}$-matrices ([[Category of Profunctors]]).
- $\mathbf{FinSet} \to \mathbf{Rel}$, $f \mapsto \mathrm{graph}(f)$, is a faithful [[Prop]] functor (Example 5.12); a relation is a [[Span]] $X \leftarrow R \to Y$ (Kittenlab Lecture 15), and spans compose by [[Pullback]].
- $\mathbf{Rel}$ is [[Compact Closed Category|compact closed]] with every object self-dual (both under $\sqcup$ and $\times$); $\mathbf{Rel}_R$ (relations $B \subseteq R^m \times R^n$ over a rig, with $B + C := \{(w,y,x,z) \mid (w,x) \in B, (y,z) \in C\}$, [[7S Exercise 5.80]]) is compact closed with cup $\{(0, (x,x))\}$ and cap $\{((x,x), 0)\}$ (Theorem 5.87), and contains $\mathbf{Mat}(R)$ via behaviours ([[Graphical Linear Algebra]]).
- Relations on a fixed set form a noncommutative [[Quantale]] under composition (7 Sketches §2.6). In a [[Topos]], relations are subobjects of products and [[Dagger Category|dagger]] structure is transposition $R^\dagger = \{(y,x)\}$.
- Kittenlab's behavioural view: a relation is a *joint constraint*; a mathematical model "selects a subset of a universum of possibilities" (Willems).

````tabs
tab: Julia
```julia
# Kittenlab Lecture 14
const FinRelation = BitMatrix
function compose(R::FinRelation, S::FinRelation)
  n, m, _, l = (size(R)..., size(S)...)
  FinRelation([any(R[i, j] && S[j, k] for j in 1:m) for i in 1:n, k in 1:l])
end
idrel(n) = FinRelation([i == j for i in 1:n, j in 1:n])
graph_of(f::Vector{Int}, n) = FinRelation([f[i] == j for i in eachindex(f), j in 1:n])   # FinSet → Rel

# Catlab: FinRel
using Catlab, Catlab.CategoricalAlgebra.FinRelations
R = FinRelation((x, y) -> x < y, 3, 3); S = compose(R, R); S(1, 3)
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
