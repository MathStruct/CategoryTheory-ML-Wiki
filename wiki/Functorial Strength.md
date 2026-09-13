#definition #theorem #example

A **strength** (tensorial strength) for an endofunctor $F$ on a [[Monoidal Category]] is a natural transformation

$$
\sigma_{a,b} : a \otimes F b \to F(a \otimes b)
$$

compatible with unitors and associator: it "smuggles" the environment $a$ *under* the functor. In Haskell every functor is strong: `strength (e, as) = fmap (e,) as` (tuple section). This is what allows a closure defined inside a functor — `pairWith' a = [\x -> (a, x)]` — to capture `a` from *outside* the list, and what lets the final `return` in a [[Do Notation|do block]] see variables bound in outer lambdas.

> Sources: DaoFP §14.9 ("Functorial strength"), §20.1 ("Self-enrichment": every Haskell endofunctor is enriched, hence strong); §14.9 ("Monads and applicatives").

- Every monad in Haskell is strong, hence lax [[Monoidal Functor|monoidal]] (`unit = return (); ma >*< mb = do { a <- ma; b <- mb; return (a, b) }`) and hence an [[Applicative Functor]].
- Categorically not every endofunctor is strong; the "magic incantation" is that Haskell's category is self-[[Enriched Category|enriched]] and every definable endofunctor is an enriched functor. Strong monads are exactly what is needed to interpret effectful languages with variables in context (Moggi).

````tabs
tab: Julia
```julia
# strength for the list functor: (e, [b]) ↦ [(e, b)]
strength(e, bs) = [(e, b) for b in bs]
strength(:env, [1, 2])                # [(:env, 1), (:env, 2)]
```
tab: Lean
```lean
import Mathlib
-- for any Functor f in Lean, strength is definable exactly as in Haskell:
def strength [Functor f] (e : α) (x : f β) : f (α × β) := Functor.map (fun b => (e, b)) x
```
tab: Haskell
```haskell
{-# LANGUAGE TupleSections #-}
strength :: Functor f => (e, f a) -> f (e, a)
strength (e, as) = fmap (e,) as

pairWith' :: Int -> [String -> (Int, String)]
pairWith' a = [\x -> (a, x)]          -- the closure captures a from outside the list
```
````
