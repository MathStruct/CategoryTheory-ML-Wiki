#theorem #proof #program

Writing the set of natural transformations as an [[End]], the [[Yoneda Lemma]] $[\mathcal{C}, \mathbf{Set}](\mathcal{C}(a, -), F) \cong F a$ becomes the **ninja Yoneda lemma** and its dual with [[Coend|coends]] the **ninja co-Yoneda lemma**:

| covariant $F$ | contravariant $G$ |
|---|---|
| $\int_{x} \mathbf{Set}(\mathcal{C}(a, x), F x) \cong F a$ | $\int_{x} \mathbf{Set}(\mathcal{C}(x, a), G x) \cong G a$ |
| $\int^{x} \mathcal{C}(x, a) \times F x \cong F a$ | $\int^{x} \mathcal{C}(a, x) \times G x \cong G a$ |

Like integrals against a Dirac delta, "integrating over $x$" replaces $x$ by $a$ in the integrand — hence *distributors* ("distributors are to functors as distributions are to functions"). On a discrete category the co-Yoneda lemma is $\sum_j \delta^j_i v_j = v_i$ with the hom-functor as the Kronecker delta; much of linear algebra transfers to $\mathbf{Set}$-valued functors (vectors), profunctors (matrices, "bimodules"), coends (traces and matrix products).

> Sources: DaoFP §17.6 ("Ninja Yoneda Lemma", "Yoneda lemma in Haskell"), §17.11, Exercise 17.6.1; §17.7–17.8 (uses: unit of [[Day Convolution]], identity of the [[Bicategory of Profunctors]]); 7 Sketches §4.3 ($\mathrm{Bool}$-profunctors and the unit matrix).

## Proof of the co-Yoneda lemma

Compare mappings out to an arbitrary set $S$ (the Yoneda trick): $\mathbf{Set}(\int^x \mathcal{C}(x, a) \times F x,\ S) \cong \int_x \mathbf{Set}(\mathcal{C}(x, a) \times F x, S)$ (co-continuity of hom) $\cong \int_x \mathbf{Set}(\mathcal{C}(x, a), S^{F x})$ (currying) $\cong S^{F a}$ (contravariant ninja Yoneda) $\cong \mathbf{Set}(F a, S)$. Since $S$ is arbitrary, $\int^x \mathcal{C}(x, a) \times F x \cong F a$. $\blacksquare$ The contravariant version is [[DaoFP Exercise 17.6.1]].

````tabs
tab: Julia
```julia
using Catlab
# co-Yoneda for C-sets: every graph is a coend of representables weighted by its own elements —
# concretely, G ≅ colim over its category of elements (cf. Yoneda Lemma note)
G = path_graph(Graph, 3)
yV = representable(Graph, :V); yE = representable(Graph, :E)
length(homomorphisms(yV, G)) == nv(G), length(homomorphisms(yE, G)) == ne(G)   # ninja Yoneda: Nat(y(c), G) ≅ G(c)
```
tab: Lean
```lean
import Mathlib
open CategoryTheory
#check @CategoryTheory.yonedaEquiv        -- (yoneda.obj X ⟶ F) ≃ F.obj (op X)
#check @CategoryTheory.coyonedaEquiv      -- (coyoneda.obj (op X) ⟶ F) ≃ F.obj X
```
tab: Haskell
```haskell
{-# LANGUAGE GADTs, RankNTypes #-}
data Yo f a x y = Yo ((a -> x) -> f y)             -- the profunctor under the end
yoneda :: Functor f => (forall x. Yo f a x x) -> f a
yoneda (Yo g) = g id
yoneda_1 :: Functor f => f a -> (forall x. Yo f a x x)
yoneda_1 fa = Yo (\h -> fmap h fa)

data CoY f a x y = CoY (x -> a) (f y)              -- the profunctor under the coend
data Coend p where Coend :: p x x -> Coend p
coyoneda :: Functor f => Coend (CoY f a) -> f a
coyoneda (Coend (CoY g fx)) = fmap g fx           -- needs nothing about the hidden x
coyoneda_1 :: Functor f => f a -> Coend (CoY f a)
coyoneda_1 fa = Coend (CoY id fa)
```
````
