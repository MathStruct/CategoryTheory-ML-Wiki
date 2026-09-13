#theorem #proof #example

**Theorem.** Every [[Adjunction]] $L \dashv R$ with unit $\eta : \mathrm{Id} \to R L$ and counit $\varepsilon : L R \to \mathrm{Id}$ defines a [[Monad]]

$$
(T, \eta, \mu) = (R \circ L,\ \eta,\ R \circ \varepsilon \circ L),
$$

the multiplication being the double whiskering of the counit ($\mu : R L R L \to R L$). Dually $L \circ R$ is a [[Comonad]]. Conversely every monad arises from an adjunction — in fact from a whole category of them, with the [[Kleisli Category|Kleisli adjunction]] initial and the [[Eilenberg-Moore Category|Eilenberg–Moore adjunction]] terminal.

> Sources: DaoFP §15.2 ("Monads from Adjunctions"), §15.3 ("Examples of Monads from Adjunctions"), §15.4–15.5, Exercises 15.2.1, 15.3.1; §10.9; Kittenlab Lecture 7 (free monoid round trip); 7 Sketches §1.4.4 ([[Closure Operator|closure operators]] from [[Galois Connection|Galois connections]], [[7S Chapter 1 Exercises#Exercise 1.119|7S Exercise 1.119]]).

*Proof sketch* ([[String Diagram|string diagrams]]). The monad laws follow from the triangle identities: replace each $T$-string by the parallel pair $L, R$; the unit law $\mu \circ (\eta \circ T) = \mathrm{id}$ becomes a zigzag in the $R$-string, which the first triangle identity straightens; associativity is the statement that two caps can be applied in either order ([[DaoFP Chapter 15 Exercises#Exercise 15.2.1|DaoFP Exercise 15.2.1]]). In Haskell, when $T$ is an endofunctor, `join = fmap counit` (the left whiskering by $R$ is a lifting, the right whiskering by $L$ is instantiation done by type inference). $\blacksquare$

## Examples

| monad | adjunction | $\eta$ | $\varepsilon$ | $\mu = R \varepsilon L$ |
|---|---|---|---|---|
| [[List Monad]] | free monoid $F \dashv U$, $\mathbf{Set} \rightleftarrows \mathbf{Mon}$ | `x ↦ [x]` | `foldr mappend mempty` | `concat` |
| [[State Monad]] | currying $(- \times s) \dashv (-)^s$ | `\a s -> (a, s)` | application `uncurry runState` | `fmap counit` |
| [[Writer Monad]] | free $M$-set $F \dashv U$, $\mathbf{Set} \rightleftarrows \mathbf{MSet}$ | `x ↦ (x, 1)` | `(x, m) ↦ a_m x` | `((x,m),n) ↦ (x, n·m)` |
| [[Maybe Monad]] | pointed objects $F \dashv U$, $\mathcal{C} \rightleftarrows 1/\mathcal{C}$ | `Just` | `[id, p]` | collapse `Just (Just a)` |
| [[Continuation Monad]] | $\mathbf{Set}(-, Z) : \mathbf{Set}^{\mathrm{op}} \rightleftarrows \mathbf{Set}$ | `\a k -> k a` | evaluation | |
| closure operator | [[Galois Connection]] $f \dashv g$ | $p \leq g f p$ | $f g q \leq q$ | idempotence |

Most of these adjunctions leave the category of Haskell types (into $\mathbf{Mon}$, $\mathbf{MSet}$, $\mathbf{Set}^{\mathrm{op}}$) even though the round trip is an endofunctor, which is why they cannot be written directly in Haskell. Composable adjunctions give [[Monad Transformer|monad transformers]].

````tabs
tab: Julia
```julia
using Catlab
# the free-monoid adjunction as a round trip on FinSets: T X = lists over X (Kittenlab lecture 7)
η(x) = [x]                                     # unit: generators
ε(ms::Vector{String}) = join(ms)               # counit at the String monoid: concatenate
μ(xss) = reduce(vcat, xss; init=Any[])         # U ε F = concat: multiplication of the list monad
μ([[1, 2], [3]])                               # [1, 2, 3]
```
tab: Lean
```lean
import Mathlib
open CategoryTheory
#check @CategoryTheory.Adjunction.toMonad      -- (L ⊣ R) → Monad C, with μ = R ε L
#check @CategoryTheory.Adjunction.toComonad
#check @CategoryTheory.Monad.adj               -- the Eilenberg–Moore adjunction of a monad
#check @CategoryTheory.Kleisli.adjunction      -- the Kleisli adjunction
```
tab: Haskell
```haskell
-- currying adjunction L s ⊣ R s and the state monad it generates
newtype L s a = L (a, s)
newtype R s c = R (s -> c)
instance Functor (R s) where fmap f (R g) = R (f . g)
unit :: a -> R s (L s a)
unit a = R (\s -> L (a, s))
counit :: L s (R s a) -> a
counit (L (R f, s)) = f s
mu :: R s (L s (R s (L s a))) -> R s (L s a)
mu = fmap counit                                  -- μ = R ∘ ε ∘ L
```
````
