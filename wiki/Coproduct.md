#definition #example #theorem #proof

The **coproduct** (**sum**) of objects $a, b$ in a [[Category]] is an object $a + b$ with **injections** $\iota_a : a \to a + b$, $\iota_b : b \to a + b$ (DaoFP: $\mathsf{Left}, \mathsf{Right}$) such that for every $x$ with $f : a \to x$, $g : b \to x$ there is a unique $[f, g] : a + b \to x$ (Kittenlab: $\langle f, g \rangle$; DaoFP: $h$) with $\iota_a \mathbin{;} [f, g] = f$ and $\iota_b \mathbin{;} [f, g] = g$. Dual to [[Product]]: the product in $\mathcal{C}^{\mathrm{op}}$.

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}
X \arrow[r, "\iota_X"] \arrow[dr, "f"'] & X + Y \arrow[d, "{[f, g]}", dashed] & Y \arrow[l, "\iota_Y"'] \arrow[dl, "g"] \\
 & Z &
\end{tikzcd}
\end{document}
```

> Sources: Kittenlab Lecture 8 ("Representatives of functors", coproducts as representing objects, the "traditional" definition), 9, 11 ("Coproducts of types in Julia"); DaoFP Chapter 4 ("Sum Types", "Cocartesian Categories"), §9.4 ("Sum as a universal cospan"), §10.2 ("The sum adjunction"); 7 Sketches §6.2.2 (Definition 6.6, Examples 6.7–6.9, Exercises 6.10–6.11), §3.4.3 ($\Sigma$ "unions data").

## Examples

- $\mathbf{Set}$: the disjoint union $X \sqcup Y = \{(x, 1)\} \cup \{(y, 2)\}$ (7 Sketches §1.2.1); in $\mathbf{FinSet}$, $\{1..n\} + \{1..m\} = \{1..n+m\}$.
- [[Preorder]]: the [[Join]] $a \vee b$; in the [[Preorder of Partitions]], the join of systems ([[Generative Effect]]).
- [[Category of Graphs|Graphs]] and all [[C-Set|C-sets]]: pointwise, $(G + H)(V) = G(V) + H(V)$, $(G + H)(E) = G(E) + H(E)$ (Kittenlab Lecture 8).
- **Sum types** in programming: Julia `Union{Left{S}, Right{T}}` — a function out of it is written by dispatch on `Left`/`Right`, i.e. by two functions, one per summand; the tagged union `TaggedUnion{S,T}` is another representing object with "precisely the same external interface" but different performance (Kittenlab Lecture 11). Haskell `Either a b` with `either :: (a -> x) -> (b -> x) -> Either a b -> x`; `Bool = 1 + 1`, `Maybe a = 1 + a`, enumerations (DaoFP Chapter 4). In logic: disjunction $A \vee B$; a proof is a proof of one side.
- $\mathbb{N}$-ary coproducts are [[Colimit|colimits]] over a [[Discrete Category]] with $n$ objects; the empty coproduct is the [[Initial Object]] (Kittenlab Lecture 9). Coproducts of monoids, groups, categories, and props exist but are *not* disjoint unions (7 Sketches §5.2.3).

## Three descriptions (Kittenlab Lecture 8, DaoFP §9.4, §10.2)

1. **Representability**: $a + b$ is a representing object of the functor $\mathrm{Hom}(a, -) \times \mathrm{Hom}(b, -) : \mathcal{C} \to \mathbf{Set}$; "a map out of the coproduct is equivalent to a map out of each summand". By the [[Yoneda Lemma]] representing objects are unique up to isomorphism, justifying "*the* coproduct". Feeding $\mathrm{id}_{a+b}$ into the isomorphism $\mathrm{Hom}(a + b, -) \cong \mathrm{Hom}(a, -) \times \mathrm{Hom}(b, -)$ recovers the injections $\iota_a, \iota_b$, and universality (1 above) follows.
2. **Universal cocone**: $[\mathbf{2}, \mathcal{C}](D, \Delta_x) \cong \mathcal{C}(a + b, x)$ where $D : \mathbf{2} \to \mathcal{C}$ picks $a, b$ — "sum as a universal [[Cospan]]": a [[Colimit]] of a two-object discrete diagram; an [[Initial Object]] in the category of cocones.
3. **Adjunction**: $(+) \dashv \Delta$ with unit the pair of injections ([[Diagonal Functor]]).

## Properties (DaoFP Chapter 4)

- **Functoriality**: $f + g := [f \mathbin{;} \iota, g \mathbin{;} \iota]$ makes $+$ a [[Bifunctor]] (`bimap` for `Either`).
- $a + 0 \cong a$ ("something plus zero"), $1 + 0 \cong 1$, commutativity $a + b \cong b + a$ (via $[\iota_b, \iota_a]$), associativity — a category with finite coproducts is **cocartesian** and $(\mathcal{C}, +, 0)$ is a [[Symmetric Monoidal Category]]. **Bicartesian** categories have both; in a [[Bicartesian Closed Category]] products distribute over sums.
- $\mathcal{C}(a + b, x) \cong \mathcal{C}(a, x) \times \mathcal{C}(b, x)$: the contravariant hom-functor turns sums into products (DaoFP §10.7: [[Hom Functor|hom preserves colimits]]).
- A [[Pushout]] is a coproduct with part of the two objects "equalized by fiat" (Kittenlab Lecture 9); [[Coequalizer|coequalizers]] "squish" (Lecture 11: "if coproducts allow you to *add* objects, coequalizers allow you to *squish* them").

````tabs
tab: Julia
```julia
# Kittenlab Lecture 11: coproducts of Julia types are tagged unions; functions out are by dispatch
struct Left{T};  val::T end
struct Right{T}; val::T end
const Coproduct{S,T} = Union{Left{S}, Right{T}}
foo(l::Left{Int}) = l.val^2                 # one function per summand: the universal property
foo(r::Right{String}) = length(r.val)
(foo(Left(4)), foo(Right("hello")))         # (16, 5)

# Catlab
using Catlab
C = coproduct(FinSet(2), FinSet(3))
apex(C)                                     # FinSet(5)
ι1, ι2 = legs(C)
f = FinFunction([1, 2], 2); g = FinFunction([2, 2, 1], 2)
h = copair(C, f, g)                         # [f, g] : FinSet(5) → FinSet(2)
compose(ι1, h) == f && compose(ι2, h) == g  # true
```
tab: Lean
```lean
#check CategoryTheory.Limits.coprod         -- X ⨿ Y with coprod.inl, coprod.inr, coprod.desc
#check CategoryTheory.Limits.coprod.desc    -- [f, g]
#check CategoryTheory.Limits.Types.binaryCoproductIso   -- X ⨿ Y ≅ X ⊕ Y in Type
example (A B X : Type) (f : A → X) (g : B → X) : A ⊕ B → X := Sum.elim f g
```
tab: Haskell
```haskell
-- DaoFP Chapter 4: sum types
data Either a b = Left a | Right b

either' :: (a -> x) -> (b -> x) -> (Either a b -> x)     -- the universal [f, g]
either' f _ (Left a)  = f a
either' _ g (Right b) = g b

-- functoriality
bimapE :: (a -> a') -> (b -> b') -> Either a b -> Either a' b'
bimapE f _ (Left a)  = Left (f a)
bimapE _ g (Right b) = Right (g b)

-- Bool = 1 + 1, Maybe a = 1 + a
data Maybe' a = Nothing' | Just' a
```
````
