#definition #theorem #example #program

The **codensity monad** of a functor $F : \mathcal{D} \to \mathcal{C}$ is the right [[Kan Extension]] of $F$ along itself, $T^F := \mathrm{Ran}_F F$ — the "fraction" $F/F$. Even when $F$ has no left adjoint this is a [[Monad]]: the unit comes from universality applied to $(\mathrm{Id}, \mathrm{id}_F)$ and the multiplication from $(T^F \circ T^F,\ \varepsilon \cdot (T^F \circ \varepsilon))$. If $F$ *does* have a left adjoint $L$, then $T^F \cong F \circ L$ is the monad of the adjunction ([[Monads from Adjunctions]]) — so every monad is a codensity monad. A functor with $\mathrm{Ran}_F F \cong \mathrm{Id}$ is called *codense*.

> Sources: DaoFP §19.3 ("Codensity monad", "Codensity monad in Haskell"), Exercises 19.3.3–19.3.4; §14.6 (continuation passing style), §14.8 (free monads).

*Proof of $T^F \cong F L$.* For any $G$: $[\mathcal{C}, \mathcal{C}](G, F L) \cong \int_c \mathcal{C}(G c, F L c) \cong \int_c \int_d \mathbf{Set}(\mathcal{D}(L c, d), \mathcal{C}(G c, F d)) \cong \int_c \int_d \mathbf{Set}(\mathcal{C}(c, F d), \mathcal{C}(G c, F d)) \cong \int_d \mathcal{C}(G F d, F d) \cong [\mathcal{D}, \mathcal{C}](G F, F) \cong [\mathcal{C}, \mathcal{C}](G, \mathrm{Ran}_F F)$ (Yoneda, adjunction, ninja Yoneda over $c$, the Ran adjunction). $\blacksquare$

**In Haskell.** From the end formula, $\mathrm{Ran}_F F\, c = \int_d \mathbf{Set}(\mathcal{C}(c, F d), F d)$:
```haskell
newtype Codensity f c = C (forall d. (c -> f d) -> f d)
instance Monad (Codensity f) where
  return x = C (\k -> k x)
  m >>= kl = C (\k -> runCodensity m (\a -> runCodensity (kl a) k))
```
— the [[Continuation Monad]] with `f = Identity`, and with the same performance benefits: binds are nested "inside out", so long chains of `>>=` (as accumulated by a [[Free Monad]], whose interpretation otherwise re-traverses the growing tree from the root on every bind) become linear — the same trick as [[Tannakian Reconstruction|difference lists]].

````tabs
tab: Julia
```julia
# Codensity of a functor as callbacks: C(k -> ...) with k :: c -> f d; here f = identity
ret(x) = k -> k(x)
bind(m, kl) = k -> m(a -> kl(a)(k))
run(m, k) = m(k)
run(bind(ret(3), x -> ret(x * 2)), identity)       # 6
```
tab: Lean
```lean
import Mathlib
open CategoryTheory
#check @CategoryTheory.Functor.ran            -- Ran_F F is the codensity monad
#check @CategoryTheory.Monad                  -- its monad structure is not packaged in Mathlib
```
tab: Haskell
```haskell
{-# LANGUAGE RankNTypes #-}
newtype Codensity f c = C (forall d. (c -> f d) -> f d)
runCodensity :: Codensity f c -> forall d. (c -> f d) -> f d
runCodensity (C h) = h
instance Functor (Codensity f) where                    -- Exercise 19.3.3
  fmap g (C h) = C (\k -> h (k . g))
instance Applicative (Codensity f) where                -- Exercise 19.3.4
  pure x = C (\k -> k x)
  C hf <*> C hx = C (\k -> hf (\g -> hx (k . g)))
instance Monad (Codensity f) where
  m >>= kl = C (\k -> runCodensity m (\a -> runCodensity (kl a) k))
```
````
