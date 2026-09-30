#example #definition #program

The **Maybe monad** encodes partiality: `data Maybe a = Nothing | Just a`, i.e. $1 + a$ ([[Sum Type]]).
```haskell
instance Monad Maybe where
  Nothing  >>= k = Nothing
  (Just a) >>= k = k a
  return = Just
```
Kleisli composition short-circuits: if the first computation fails, the second is skipped — a change of control flow driven by the effect. `Either e` is the variant carrying error data in `Left`.

> Sources: DaoFP §14.1–14.4, §15.3 ("Pointed objects and the Maybe monad"), Exercises 14.4.1, 15.3.1; §4.3 ("Maybe"); CTfS §5.3.1 (Example 5.3.1.1), Examples 5.3.2.4, 5.3.3.2, Exercise 5.3.2.5, Example 5.3.4.1

**Partial functions** (CTfS Example 5.3.1.1). The partial function $x \mapsto 1/(x^2 - 1)$ on $\mathbb R$ is an ordinary function $\mathbb R \to \mathbb R \sqcup \{\star\}$ sending $\pm 1$ to the "no answer" element $\star$. Composing $f : A \to B \sqcup \{\star\}$ with $g : B \to C \sqcup \{\star\}$ means: extend $g$ by $\star \mapsto \star$, compose, and merge the two $\star$'s — the unit and multiplication of the monad $X \mapsto X \sqcup \{\star\}$. With a set $E$ of exceptions ("overflow!", "division by zero!") in place of $\star$ one gets the exception monad $X \sqcup E$ (Haskell's `Either e`, [[CTfS Chapter 5 Exercises#Exercise 5.3.2.5|CTfS Exercise 5.3.2.5]]). A database instance valued in partial functions is a graph whose edges may lack a source or target ([[Kleisli Instance]]).

**From an adjunction** ([[Monads from Adjunctions]]): a *pointed object* is a pair $(a, p : 1 \to a)$; pointed objects and point-preserving arrows form the coslice category $1/\mathcal{C}$. The forgetful $U : 1/\mathcal{C} \to \mathcal{C}$ has left adjoint $F a = (1 + a, \mathsf{Left})$ (freely add a point), and $U F a = 1 + a$ is `Maybe` ([[DaoFP Chapter 15 Exercises#Exercise 15.3.1|DaoFP Exercise 15.3.1]]); replacing $1$ by a fixed $e$ gives `Either e`. The [[Natural Numbers Object]] is the [[Initial Algebra]] of the same functor.

````tabs
tab: Julia
**Docs:** plain Julia — Catlab has no dedicated API for this; related: [Catlab v0.16 docs](https://algebraicjulia.github.io/Catlab.jl/v0.16/) · [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/)
```julia
# Maybe as Union{Some{T}, Nothing}: bind short-circuits on nothing
bind(::Nothing, k) = nothing
bind(x::Some, k) = k(something(x))
ret(a) = Some(a)
safehead(v) = isempty(v) ? nothing : Some(v[1])
bind(safehead([4, 5]), x -> Some(x + 1))       # Some(5)
bind(safehead(Int[]), x -> Some(x + 1))        # nothing
```
tab: Lean
```lean
import Mathlib
#check @Option.bind          -- Option α → (α → Option β) → Option β
#check @Option.some
example : Option ℕ := (some 4).bind (fun x => some (x + 1))
```
tab: Haskell
```haskell
safeDiv :: Double -> Double -> Maybe Double
safeDiv _ 0 = Nothing
safeDiv x y = Just (x / y)
calc :: Maybe Double
calc = safeDiv 8 2 >>= safeDiv 12 >>= \z -> return (z + 1)   -- Just 4.0
```
````
