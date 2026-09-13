#theorem #proof #example #program

**Tannakian reconstruction** recovers a category from its category of $\mathbf{Set}$-valued representations. For a small category $\mathcal{C}$ and objects $a, b$,

$$
\int_{F : [\mathcal{C}, \mathbf{Set}]} \mathbf{Set}(F a, F b) \cong \mathcal{C}(a, b).
$$

For a one-object category (a [[Monoid]] $\mathcal{M}$) this says $\int_F \mathbf{Set}(F *, F *) \cong \mathcal{M}(*, *)$: a monoid is determined by all its representations (functors $\mathcal{M} \to \mathbf{Set}$, i.e. $M$-sets) together with the equivariant maps between them — even though a single representation may be very lossy.

> Sources: DaoFP §18.1 ("Tannakian Reconstruction": "Monoids and their Representations", "Cayley's theorem", "Tannakian reconstruction of a monoid", "Proof of Tannakian reconstruction", "Tannakian reconstruction in Haskell", "Tannakian reconstruction with adjunction"), Exercise 18.1.1; §17.3 ([[End|ends]]), §17.6.

*Proof.* By the [[Yoneda Lemma]], $F a \cong [\mathcal{C}, \mathbf{Set}](\mathcal{C}(a, -), F)$ and likewise for $b$. The end becomes $\int_F \mathbf{Set}([\mathcal{C}, \mathbf{Set}](\mathcal{C}(a,-), F), [\mathcal{C}, \mathbf{Set}](\mathcal{C}(b,-), F))$, which by the Yoneda corollary $\int_z \mathbf{Set}(\mathcal{D}(x, z), \mathcal{D}(y, z)) \cong \mathcal{D}(y, x)$ applied in $\mathcal{D} = [\mathcal{C}, \mathbf{Set}]$ equals $[\mathcal{C}, \mathbf{Set}](\mathcal{C}(b, -), \mathcal{C}(a, -)) \cong \mathcal{C}(a, b)$ by Yoneda again. $\blacksquare$ The wedge condition is where the structure enters: for each natural transformation $\alpha : G \to H$ (equivariant map) the tuple's components must satisfy $\alpha \circ g = h \circ \alpha$. Interpretation: an element of the left side proves that for every structure-compatible (proof-relevant) subset containing $a$, $b$ is in it too — only possible if there is an arrow $a \to b$.

- **Cayley's theorem** is built in: the representation $F * = \mathcal{M}(*, *)$ with action by post-composition is full and faithful — every monoid is a monoid of endofunctions. Programming use: **difference lists** `type DList a = [a] -> [a]`, `rep as = (as ++)`, `unRep f = f []`; `rep [] = id`, `rep (xs ++ ys) = rep xs . rep ys`, so `fastReverse = unRep . rev` with `rev (a : as) = rev as . rep [a]` runs in $O(N)$ instead of $O(N^2)$ — prepends are queued and executed FIFO; `foldl` reverses lists the same way.
- **In Haskell**: `forall f. Functor f => f a -> f b ≅ a -> b`; `toTannaka g = fmap g`, `fromTannaka g a = runIdentity (g (Identity a))`. The type `Getter a b = forall f. Functor f => f a -> f b` is "the precursor of all optics", and such representations compose by plain function composition.
- **With an adjunction**: for a free/forgetful adjunction $F \dashv U$ between a functor category $\mathcal{T}$ (e.g. [[Tambara Module|Tambara modules]]) and $[\mathcal{C}, \mathbf{Set}]$, the same manipulations give $\int_{P : \mathcal{T}} \mathbf{Set}((U P) a, (U P) s) \cong (\Phi\, \mathcal{C}(a, -))\, s$ with $\Phi = U F$ the induced [[Monad]] — the foundation of [[Profunctor Optics]]. The *fiber functor* $P \mapsto (U P) a$ probes the "infinitesimal neighbourhood" of $a$ (compare stalks of [[Sheaf|sheaves]]).

````tabs
tab: Julia
```julia
# Cayley / difference lists: represent lists as prepending closures; reversal in O(N)
rep(as) = xs -> vcat(as, xs)
unrep(f) = f(Int[])
rev(as) = isempty(as) ? rep(Int[]) : rev(as[2:end]) ∘ rep([as[1]])
unrep(rev([1, 2, 3]))                  # [3, 2, 1]
```
tab: Lean
```lean
import Mathlib
open CategoryTheory
-- the Yoneda corollary behind the proof: maps between representables are arrows the other way
#check @CategoryTheory.yonedaEquiv
#check @CategoryTheory.Yoneda.fullyFaithful
#check @MulAction                          -- representations of a monoid as M-sets
```
tab: Haskell
```haskell
{-# LANGUAGE RankNTypes #-}
newtype Identity a = Identity { runIdentity :: a }
instance Functor Identity where fmap g (Identity a) = Identity (g a)

toTannaka :: (a -> b) -> (forall f. Functor f => f a -> f b)
toTannaka g fa = fmap g fa
fromTannaka :: (forall f. Functor f => f a -> f b) -> (a -> b)
fromTannaka g a = runIdentity (g (Identity a))

type DList a = [a] -> [a]                 -- Cayley representation of the list monoid
rep :: [a] -> DList a
rep as = (as ++)
unRep :: DList a -> [a]
unRep f = f []
rev :: [a] -> DList a
rev [] = rep []
rev (a : as) = rev as . rep [a]
fastReverse :: [a] -> [a]
fastReverse = unRep . rev
```
````
