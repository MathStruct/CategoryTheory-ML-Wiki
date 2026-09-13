#definition #theorem #example #program

The **end** of a functor $P : \mathcal{C}^{\mathrm{op}} \times \mathcal{C} \to \mathcal{D}$ is the dual of the [[Coend]]: the "product of the diagonal entries". A **wedge** is an object $d$ with projections $\pi_x : d \to P\langle x, x\rangle$ such that for every $f : x \to y$
$$P\langle f, \mathrm{id}_y \rangle \circ \pi_y = P\langle \mathrm{id}_x, f \rangle \circ \pi_x : d \to P\langle x, y \rangle,$$
and the end $\int_{x : \mathcal{C}} P\langle x, x \rangle$ is the universal wedge. In $\mathbf{Set}$: form the giant product of all $P\langle x, x\rangle$ and keep only the tuples satisfying the wedge condition. In Haskell, parametricity makes the wedge condition automatic: `type End p = forall x. p x x` — to build an end one must supply a *polymorphic formula*, whereas to build a coend one picks a single type (`exists x. p x x`).

> Sources: DaoFP §17.3 ("Ends": "Natural transformations as an end", "Limits as ends"), §17.4 ("Continuity of the Hom-Functor"), §17.5 ("Fubini Rule"), §17.6, §17.11, Exercise 17.3.1; §20.3 (ends as [[Weighted Limit|weighted limits]]).

- **Natural transformations as an end**: $\langle a, b\rangle \mapsto \mathcal{D}(F a, G b)$ is a profunctor, and the wedge condition for its diagonal is exactly naturality $G f \circ \alpha_a = \alpha_b \circ F f$; hence
$$[\mathcal{C}, \mathcal{D}](F, G) \cong \int_{x : \mathcal{C}} \mathcal{D}(F x, G x), \qquad \texttt{type Natural f g = forall x. f x -> g x}.$$
- **Limits as ends**: for $P\langle x, y\rangle = F y$ a wedge is a [[Cone]], so $\int_x F x = \lim F$; a [[Product]] is the end over the discrete two-object category ([[DaoFP Exercise 17.3.1]]).
- **Continuity of the hom-functor**: $\mathcal{C}(x, -)$ preserves limits ($\mathcal{C}(x, a \times b) \cong \mathcal{C}(x, a) \times \mathcal{C}(x, b)$) and $\mathcal{C}(-, x)$ turns colimits into limits ($\mathcal{C}(a + b, x) \cong \mathcal{C}(a, x) \times \mathcal{C}(b, x)$); hence the integral sign can be pulled out of a hom-set: $\mathcal{C}(d, \int_a P\langle a, a\rangle) \cong \int_a \mathcal{C}(d, P\langle a, a\rangle)$ and $\mathcal{C}(\int^a P\langle a, a\rangle, d) \cong \int_a \mathcal{C}(P\langle a, a\rangle, d)$.
- **Fubini**: $\int_c \int_d P \cong \int_d \int_c P \cong \int_{\langle c, d\rangle} P$ whenever the ends exist; likewise for coends.
- The [[Ninja Yoneda Lemma]] $\int_x \mathbf{Set}(\mathcal{C}(a, x), F x) \cong F a$ is the Yoneda lemma with the set of natural transformations written as an end. In calculus one rarely sees "product integrals" because logarithms turn them into sums; category theory has no logarithm, so ends and coends are equally important.

````tabs
tab: Julia
```julia
using Catlab
# ends over a finite discrete category are products of the diagonal sets
Pdiag = [FinSet(2), FinSet(3)]
ob(product(Pdiag))                             # FinSet(6) = ∫_x P⟨x,x⟩ = P⟨1,1⟩ × P⟨2,2⟩
# natural transformations as an end: all homs between two graphs, checked for naturality
G = path_graph(Graph, 2); H = cycle_graph(Graph, 2)
length(homomorphisms(G, H))                    # the (finite) end ∫_x Set(F x, G x) for C-sets
```
tab: Lean
```lean
import Mathlib
open CategoryTheory
-- natural transformations as an end: NatTrans F G is a family with the wedge (naturality) condition
#check @CategoryTheory.NatTrans
#check @CategoryTheory.NatTrans.naturality
#check @CategoryTheory.Limits.limit               -- ends are limits over the twisted arrow category
```
tab: Haskell
```haskell
{-# LANGUAGE RankNTypes #-}
type End p = forall x. p x x
type Natural f g = forall x. f x -> g x          -- ∫_x Hom(f x, g x)

newtype HomFG f g x y = HomFG (f x -> g y)       -- the profunctor ⟨x,y⟩ ↦ Hom(F x, G y)
-- End (HomFG f g) ≅ Natural f g
```
````
