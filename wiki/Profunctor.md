#definition #example #theorem

Let $\mathcal{V}$ be a (unital commutative, skeletal) [[Quantale]] and $\mathcal{X}, \mathcal{Y}$ be $\mathcal{V}$-[[Enriched Category|categories]]. A **$\mathcal{V}$-profunctor** from $\mathcal{X}$ to $\mathcal{Y}$, written $\Phi : \mathcal{X} \nrightarrow \mathcal{Y}$, is a $\mathcal{V}$-[[Enriched Functor|functor]]

$$
\Phi : \mathcal{X}^{\mathrm{op}} \times \mathcal{Y} \to \mathcal{V},
$$

where $\mathcal{V}$ is regarded as [[Monoidal Closed Preorder|enriched in itself]]. Concretely ([[7S Chapter 4 Exercises#Exercise 4.9|7S Exercise 4.9]]): a function $\Phi : \mathrm{Ob}(\mathcal{X}) \times \mathrm{Ob}(\mathcal{Y}) \to V$ such that

$$
\mathcal{X}(x', x) \otimes \Phi(x, y) \otimes \mathcal{Y}(y, y') \leq \Phi(x', y').
$$

In ordinary category theory (DaoFP §8.3, §17.1), a profunctor is a [[Functor]] $P : \mathcal{C}^{\mathrm{op}} \times \mathcal{D} \to \mathbf{Set}$: it maps a pair of objects to a set $P\langle a, b\rangle$ and a pair of arrows $\langle f : s \to a, g : b \to t \rangle$ to a function $P\langle a, b \rangle \to P\langle s, t \rangle$ — "simultaneously a producer and a consumer".

> Sources: 7 Sketches §4.2 (Definition 4.8, Examples 4.11, 4.13, Remark 4.16, Exercises 4.9, 4.10, 4.12, 4.15, 4.17), §4.3, §4.5; DaoFP §8.3 ("Profunctors"), §8.4 (the [[Hom Functor]] is a profunctor), §17.1 ("Profunctors", "Collages", "Profunctors as relations", "Profunctor composition in Haskell"), §17.2, §17.8 ([[Bicategory of Profunctors]]), §18 ([[Tambara Module|Tambara modules]], profunctor optics), §20.2 (enriched profunctors); Kittenlab Lecture 14 ($\mathbb{B}$-relations).

## Examples

- **$\mathbf{Bool}$-profunctors** are [[Feasibility Relation|feasibility relations]] between preorders — *bridges between cities* (Example 4.11): $\Phi(x, y) = \mathsf{true}$ iff there is a path from $x$ through $\mathcal{X}$, across a bridge, and through $\mathcal{Y}$ to $y$.
- **[[Cost]]-profunctors** between [[Lawvere Metric Space|Lawvere metric spaces]] are bridges *labelled by length*: $\Phi(x, y)$ is the shortest path from $x$ through $\mathcal{X}$, over a bridge, through $\mathcal{Y}$ to $y$ (Example 4.13: $\Phi(B, x) = 11$, $\Phi(A, z) = 20$, $\Phi(C, y) = 17$; [[7S Chapter 4 Exercises#Exercise 4.15|7S Exercise 4.15]]). Remark 4.16: with $M_\Phi$ the matrix of bridge lengths ($\infty$ where there is none), $\Phi = d_{\mathcal{X}} \ast M_\Phi \ast d_{\mathcal{Y}} = M_{\mathcal{X}}^3 \ast M_\Phi \ast M_{\mathcal{Y}}^2$ by [[Matrix Multiplication in a Quantale|min-plus matrix multiplication]] ([[7S Chapter 4 Exercises#Exercise 4.17|7S Exercise 4.17]]).
- **$\mathbf{Set}$-profunctors** (DaoFP): the [[Hom Functor]] $\mathcal{C}(-, -)$ is the model — "a profunctor provides additional bridges between objects, on top of the hom-sets already there"; a profunctor is a **proof-relevant relation**: each element of $P\langle a, b\rangle$ is a proof that $b$ is related to $a$, compatible with the structure of the categories (if $\mathcal{C}(s, a)$ and $\mathcal{D}(b, t)$ are nonempty then relatedness transfers). In Haskell `class Profunctor p where dimap :: (s -> a) -> (b -> t) -> p a b -> p s t`, with `(->)` as the prime instance (`dimap f g h = g . h . f`): "in programming, all non-trivial profunctors are variations on the function type". The [[Exponential Object]] $b^a$ is functorial as a profunctor.
- [[Companion and Conjoint|Companions and conjoints]] of functors: $\hat F(p, q) = \mathcal{Q}(F p, q)$, $\check F(q, p) = \mathcal{Q}(q, F p)$; the unit profunctor $U_{\mathcal{X}} = \mathcal{X}(-, -)$.
- Enriched profunctors $\mathcal{C}^{\mathrm{op}} \otimes \mathcal{D} \to \mathcal{V}$ for a monoidal closed $\mathcal{V}$ (DaoFP §20.2).

## Composition

Profunctors compose by a *sum over a middle object* — "in general, we say two objects are related by the composite relation if there exists an object in the middle related to both": for a quantale,

$$
(\Phi \mathbin{;} \Psi)(p, r) = \bigvee_{q \in \mathcal{Q}} \Phi(p, q) \otimes \Psi(q, r)
$$

(Definition 4.21, matrix multiplication), giving the [[Category of Profunctors]] $\mathbf{Prof}_{\mathcal{V}}$. For $\mathbf{Set}$-profunctors the naive sum $\sum_x P\langle a, x\rangle \times Q\langle x, b\rangle$ over-counts when middle objects are connected by morphisms; the correct composite is the [[Coend]] $\int^x P\langle a, x\rangle \times Q\langle x, b \rangle$ (DaoFP §17.2). In Haskell, `data Procompose p q a b where Procompose :: q a x -> p x b -> Procompose p q a b` works thanks to parametricity: "the two arguments are a pair of proofs, one that $x$ is related to $a$, and one that $b$ is related to $x$" — like charging your phone through a friend who owns a charger. Composition is associative only up to isomorphism in general, making $\mathbf{Prof}$ a [[Bicategory of Profunctors|bicategory]] whose monads are [[Prearrow|prearrows]]; for skeletal quantales one gets an honest category.

## Collages

Any profunctor $\Phi : \mathcal{X} \nrightarrow \mathcal{Y}$ glues its two categories into one: the [[Collage]] $\mathrm{Col}(\Phi)$ (DaoFP: *cograph*), with objects $\mathrm{Ob}\,\mathcal{X} \sqcup \mathrm{Ob}\,\mathcal{Y}$ and $\Phi(x, y)$ as the "heteromorphisms" from $\mathcal{X}$ to $\mathcal{Y}$. Conversely a category with a functor to the [[Walking Arrow]] splits as a collage ([[DaoFP Chapter 17 Exercises#Exercise 17.1.2|DaoFP Exercise 17.1.2]]).

## Further

- $\mathbf{Prof}_{\mathcal{V}}$ is a [[Compact Closed Category]] with $\mathcal{X}^* = \mathcal{X}^{\mathrm{op}}$ (Theorem 4.63): profunctors are exactly what is needed to interpret feedback [[Wiring Diagram|wiring diagrams]] in [[Co-design]]. The Kittenlab preamble writes $\mathbb{P}\mathrm{rof}$ with $\pto$.
- [[Kan Extension|Kan extensions]] and [[Day Convolution]] are computed by (co)ends of profunctors; [[Tambara Module|Tambara modules]] are profunctors with extra structure that classify [[Lens|optics]] (DaoFP Ch. 18).
- 7 Sketches §4.6: profunctors generalize binary relations; a "delightful exposition" of profunctors, equipments, companions and conjoints is [Shu08; Shu10].

````tabs
tab: Julia
```julia
# a V-profunctor between finite V-categories as a matrix; composition by quantale matrix multiplication
struct VProfunctor{T}
  X::VCategory; Y::VCategory; Φ::Matrix{T}     # Φ[i, j] = Φ(X.objects[i], Y.objects[j])
end
function is_profunctor(P::VProfunctor)
  V = P.X.base
  all(leq(V, otimes(V, otimes(V, P.X.hom[i′, i], P.Φ[i, j]), P.Y.hom[j, j′]), P.Φ[i′, j′])
      for i in axes(P.Φ, 1), i′ in axes(P.Φ, 1), j in axes(P.Φ, 2), j′ in axes(P.Φ, 2))
end
compose(P::VProfunctor, Q::VProfunctor) = VProfunctor(P.X, Q.Y, qmul(P.X.base, P.Φ, Q.Φ))   # (Φ;Ψ)(p,r) = ⋁_q Φ(p,q) ⊗ Ψ(q,r)

# Example 4.13 (Cost): Φ = d_X * M_Φ * d_Y via min-plus products
MX = [0.0 Inf 3 Inf; 2 0 Inf 5; Inf 3 0 Inf; Inf Inf 4 0]
MΦ = [Inf Inf Inf; 11 Inf Inf; Inf Inf Inf; Inf 9 Inf]
MY = [0.0 4 3; 3 0 Inf; Inf 4 0]
Φ = qmul(CostPre(), qmul(CostPre(), distances(MX), MΦ), distances(MY))   # Φ[2,1] == 11, Φ[1,3] == 20, Φ[3,2] == 17
```
tab: Lean
```lean
-- a profunctor C ⇸ D is a functor Dᵒᵖ × C ⥤ Type (Mathlib's convention puts the contravariant variable first)
example (C D : Type) [CategoryTheory.Category C] [CategoryTheory.Category D] : Type _ := Dᵒᵖ × C ⥤ Type
#check CategoryTheory.Functor.hom      -- the hom-functor Cᵒᵖ × C ⥤ Type as the prototypical profunctor
-- monotone Bool-profunctors between preorders:
def BoolProf (X Y : Type) [Preorder X] [Preorder Y] := (Xᵒᵈ × Y) →o Bool
```
tab: Haskell
```haskell
{-# LANGUAGE GADTs, RankNTypes #-}
-- DaoFP §8.3: profunctors
class Profunctor p where
  dimap :: (s -> a) -> (b -> t) -> (p a b -> p s t)

instance Profunctor (->) where
  dimap f g h = g . h . f            -- pre-compose with f, post-compose with g

-- DaoFP §17.1: composition via an existential middle object (a pair of "proofs")
data Procompose p q a b where
  Procompose :: q a x -> p x b -> Procompose p q a b

instance (Profunctor p, Profunctor q) => Profunctor (Procompose p q) where
  dimap l r (Procompose qax pxb) = Procompose (dimap l id qax) (dimap id r pxb)

mapOut :: Procompose p q a b -> (forall x. q a x -> p x b -> c) -> c
mapOut (Procompose qax pxb) f = f qax pxb
```
````
