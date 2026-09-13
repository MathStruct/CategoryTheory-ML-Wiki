#definition #theorem #program #annotation

The **terminal coalgebra** $(t, \tau)$ of an [[Endofunctor]] $F$ is the [[Terminal Object]] of the category of [[Coalgebra of an Endofunctor|$F$-coalgebras]]: from every coalgebra $(a, \alpha)$ there is a unique coalgebra morphism $h : a \to t$, the **anamorphism** ("lens brackets" $[\!(\alpha)\!]$, `ana coa`). By the dual of Lambek's lemma ([[DaoFP Chapter 13 Exercises#Exercise 13.2.1|DaoFP Exercise 13.2.1]]) $\tau : t \to F t$ is an isomorphism, so $t$ is a fixed point — the *greatest fixed point* $\nu F$. $(t, \tau^{-1})$ is also an algebra and $(i, \iota^{-1})$ a coalgebra, and the unique morphism $\rho : \mu F \to \nu F$ is the same whether computed as a catamorphism or an anamorphism; in $\mathbf{Set}$ it embeds the [[Initial Algebra]] as a subset.

> Sources: DaoFP §13.2 ("Category of Coalgebras"), §13.3 ("Anamorphisms", "Infinite data structures"), §13.4 ("Hylomorphisms", "The impedance mismatch"), §13.5 ("Terminal Coalgebra from Universality"), §13.6 ("Terminal Coalgebra as a Limit"); §16 (streams and comonads).

## Anamorphisms and hylomorphisms

Solving the square, $h = \tau^{-1} \circ F h \circ \alpha$:
```haskell
ana  :: Functor f => Coalgebra f a -> a -> Fix f
ana coa = In . fmap (ana coa) . coa
hylo :: Functor f => Algebra f b -> Coalgebra f a -> a -> b
hylo alg coa = alg . fmap (hylo alg coa) . coa           -- unfold then fold, no Fix in sight
```
`ana split` grows a sorted tree from a list; with `toList (NodeF n ns ms) = ns ++ [n] ++ ms`, `qsort = cata toList . ana split = hylo toList split` is (an inefficient) quicksort. Thanks to laziness the intermediate tree is never fully materialized — hylomorphisms replace recursive backtracking by *designing a data structure*.

## Infinite data and the impedance mismatch

Functors without leaves are fine for coalgebras: `data StreamF a x = StreamF a x` has terminal coalgebra the infinite **stream**; `ana step 0` with `step n = StreamF n (n+1)` is all naturals, lazily. In $\mathbf{Set}$, $\mu F$ ($= \varnothing$ here) is a proper subset of $\nu F$ and hylomorphisms exist only on it; Haskell conflates both in `Fix f`, so `hylo add step 1` (summing all naturals) type-checks but never terminates — it returns bottom $\bot$. Domain theory (lifted types) models this, but breaks uniqueness in universal constructions: "Haskell code should be treated as an illustration of categorical concepts rather than a source of rigorous proofs."

## Universality and the limit construction

- **Nu**: uncurrying `ana` gives the *existential* type `data Nu f = Nu (exists a. (a -> f a, a))` — a seed plus its unfolding; clients know only that such an `a` exists and can only keep applying `fmap`-lifted `coa`. Existentials are [[Coend|coends]]; compare `Mu f = forall a. Algebra f a -> a` ([[End|end]]).
- **Limit of the $\omega^{\mathrm{op}}$-chain**: $1 \xleftarrow{!} F 1 \xleftarrow{F !} F^2 1 \leftarrow \cdots$; for $F_a x = a \times x$ the stages are streams of length $n$, and the limit glues the approximations into the infinite stream. The proof dualizes the colimit construction of $\mu F$.

````tabs
tab: Julia
```julia
# anamorphism, hylomorphism and quicksort with the TreeF coalgebra of [[Coalgebra of an Endofunctor]]
ana(coa, fmap) = a -> Fix(fmap(ana(coa, fmap), coa(a)))
hylo(alg, coa, fmap) = a -> alg(fmap(hylo(alg, coa, fmap), coa(a)))
tolist(::LeafF) = Int[]
tolist(t::NodeF) = vcat(t.l, [t.n], t.r)
qsort = hylo(tolist, split_coalg, fmapT)
qsort([3, 1, 4, 1, 5, 9, 2, 6])               # [1, 1, 2, 3, 4, 5, 6, 9]
# streams: Julia has lazy iterators instead of laziness; Iterators.countfrom(0) is ν(StreamF Int)
```
tab: Lean
```lean
import Mathlib
open CategoryTheory
#check @CategoryTheory.Endofunctor.Coalgebra.Terminal.strInv     -- dual Lambek
#check @CategoryTheory.Endofunctor.Coalgebra.Terminal.str_isIso
#check @Stream'                    -- ℕ → α : the terminal coalgebra of α × (-) in Type
#check @Stream'.corec              -- the anamorphism: (β → α) → (β → β) → β → Stream' α
```
tab: Haskell
```haskell
ana :: Functor f => Coalgebra f a -> a -> Fix f
ana coa = In . fmap (ana coa) . coa

hylo :: Functor f => Algebra f b -> Coalgebra f a -> a -> b
hylo alg coa = alg . fmap (hylo alg coa) . coa

toList :: Algebra TreeF [Int]
toList LeafF = []
toList (NodeF n ns ms) = ns ++ [n] ++ ms
qsort :: [Int] -> [Int]
qsort = hylo toList split

data StreamF a x = StreamF a x deriving Functor
type Stream a = Fix (StreamF a)
allNats :: Stream Int
allNats = ana (\n -> StreamF n (n + 1)) 0

data Nu f where                               -- the terminal coalgebra as an existential
  Nu :: (a -> f a, a) -> Nu f
headNu :: Nu (StreamF a) -> a
headNu (Nu (unf, s)) = let StreamF a _ = unf s in a
```
````
