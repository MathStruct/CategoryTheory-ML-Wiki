#definition #example

A [[Function]] $f : A \to B$ is **injective** (an **injection**, drawn $A \hookrightarrow B$) if for all $t \in B$ and $s_1, s_2 \in A$ with $f(s_1) = t = f(s_2)$ we have $s_1 = s_2$. Equivalently (Kittenlab): whenever $a \neq a'$ then $f(a) \neq f(a')$.

> Sources: 7 Sketches Definition 1.22, Exercise 1.24; Kittenlab Lecture 2; DaoFP §2.4 (monomorphisms).

**Examples.** $\{1,2\} \to \{1,2,3\}$, $1 \mapsto 1$, $2 \mapsto 2$ is injective (and not surjective). $\{1,2,3\} \to \{1,2\}$ with $3 \mapsto 2$ is not. The unique function $\varnothing \to \{1\}$ is injective but not surjective.

- Injective functions are exactly the [[Monomorphism|monomorphisms]] in $\mathbf{Set}$ (DaoFP §2.4). Any arrow out of the [[Terminal Object]] is a monomorphism.
- A function is a [[Bijection]] iff it is injective and [[Surjection|surjective]]; then it has an inverse.
- The [[Pigeonhole Principle]]: a function from a larger finite set to a smaller one cannot be injective.
- An injection $U \hookrightarrow X$ is the "[[Subobject]]" view of a [[Subset]].

````tabs
tab: Julia
```julia
# Kittenlab Lecture 2
function is_injective(f::𝔽Mor)
  length(unique!(collect(f.dom))) == length(unique!([f(x) for x in f.dom]))
end

# Catlab
using Catlab
f = FinFunction([1, 2], 3)
is_monic(f)     # true
```
tab: Lean
```lean
#check @Function.Injective   -- ∀ a₁ a₂, f a₁ = f a₂ → a₁ = a₂
example : Function.Injective (fun n : ℕ => n + 1) := Nat.succ_injective
-- in Mathlib's category of types, mono ↔ injective:
#check @CategoryTheory.mono_iff_injective
```
tab: Haskell
```haskell
import Data.List (nub)
-- check injectivity on a finite domain
isInjective :: Eq b => [a] -> (a -> b) -> Bool
isInjective dom f = length (nub (map f dom)) == length dom
```
````
