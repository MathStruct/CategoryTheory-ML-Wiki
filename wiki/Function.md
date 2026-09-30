#definition #example #program

Let $S$ and $T$ be [[Set|sets]]. A **function** from $S$ to $T$ is a [[Relation]] $F \subseteq S \times T$ such that for all $s \in S$ there exists a *unique* $t \in T$ with $(s, t) \in F$. We write $F : S \to T$, $F(s) = t$ or $s \mapsto t$. $S$ is the **domain**, $T$ the **codomain**. For $t \in T$ the **preimage** of $t$ along $F$ is $\{s \in S \mid F(s) = t\}$.

> Sources: 7 Sketches Definition 1.22, Example 1.23, 1.29; Kittenlab Lecture 1 & 3; DaoFP §1.1 ("Types and Functions"), §2.2 ("Function application"); CTfS §2.1.2 (Applications 2.1.2.1, 2.1.2.9, Exercises 2.1.2.2–2.1.2.6), §2.3.2 (aspects in ologs)

## Special kinds of function

| name | arrow | condition |
|---|---|---|
| [[Surjection\|surjective]] | $\twoheadrightarrow$ | every $t$ has some $s$ with $F(s) = t$ |
| [[Injection\|injective]] | $\hookrightarrow$ | $F(s_1) = F(s_2)$ implies $s_1 = s_2$ |
| [[Bijection\|bijective]] | $\xrightarrow{\ \sim\ }$ | both |

The [[Identity Function]] $\mathrm{id}_X(x) = x$ is bijective. Functions compose ([[Function Composition]]), and sets with functions form the [[Category of Sets]] $\mathbf{Set}$. A function to the empty set forces the domain to be empty ([[7S Chapter 1 Exercises#Exercise 1.25|7S Exercise 1.25]]).

## Functions in science (Category Theory for Scientists)

- **Is it a function?** "For every element $x \in X$ there is exactly one arrow emanating from $x$, but an element of $Y$ can be hit several times or not at all." About 100 million photoreceptor cells each connect to exactly one retinal ganglion cell, many to the same one: a function $PR \to RG$, not $RG \to PR$ ([[CTfS Chapter 2 Exercises#Exercise 2.1.2.2|CTfS Exercise 2.1.2.2]]). Measuring a material's force–extension curve is ideally a function from materials (really: material *samples*) to curves (CTfS Application 2.1.2.1).
- **Aspects.** In an [[Olog]] every arrow must be a function — an *aspect* of the source type. "A person *has as mother* a woman" is valid; "a person *has* a child" is not (zero or several children), and is repaired by reversing the arrow ("a child has a mother") or by introducing a new type of pairs.
- **Restriction and image**: composing with a subset inclusion $X' \subseteq X$ restricts $f$ to $X'$; the *image* $\mathrm{im}(f) = \{y \mid \exists x.\ f(x) = y\}$, e.g. "a mother" is the image of "has as mother" (CTfS §2.3.3.8). There are $|B|^{|A|}$ functions $A \to B$: 32 from a 5-element set to a 2-element set and 25 the other way ([[CTfS Chapter 2 Exercises#Exercise 2.1.2.5|CTfS Exercise 2.1.2.5]]).

## Elements as functions (Example 1.29, DaoFP §1.3)

An element $x \in X$ is the same as a function $\{1\} \to X$ (a [[Global Element]]). Evaluating $F$ at $x$ is then composition: $F(x) = x \mathbin{;} F$. DaoFP takes this further: in an arbitrary category, a "generalized element" of $X$ is any arrow into $X$.

## Kittenlab: functions as data structures

A morphism of [[Finite Set|finite sets]] can be stored as a dictionary of values; a function between ordinals $\{1..n\} \to \{1..m\}$ as a vector of integers. Not every value of the Julia type is a valid function — validity has to be checked (`isvalid`), since Julia's types cannot narrow the value space enough ("languages whose types can, don't have well-maintained BLAS bindings"). For infinite sets, $A \to B$ is the set of Julia callables $f$ with $f(a) \in B$ for all $a \in A$; membership is not checkable.

## DaoFP: types and functions

In programming a function $f : A \to B$ is an arrow between *types*; the type is a set of values and the function a total, deterministic mapping. "There are no functions out of the empty set … except the empty function"; DaoFP's [[Initial Object|Void]] and [[Terminal Object|unit]] types are the categorical shadow of the sets $\varnothing$ and $\{1\}$.

````tabs
tab: Julia
**Docs:** [FinSets](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.FinSets) — Kittenlab [Lecture 1](https://algebraicjulia.github.io/Kittenlab.jl/lecture1.html)

**Builds on:** [[Finite Set]] (`Int𝔽`, `Vec𝔽`, `𝔽`) — run that note's Julia code first.
```julia
# Kittenlab Lecture 1
struct 𝔽Mor
  dom::𝔽
  codom::𝔽
  vals::Dict{Any,Any}
end
(f::𝔽Mor)(x) = f.vals[x]
isvalid(f::𝔽Mor) = all(x ∈ keys(f.vals) && f(x) ∈ f.codom for x in f.dom)

f = 𝔽Mor(Vec𝔽([:carrots, :peas]), Int𝔽(3), Dict(:carrots => 3, :peas => 3))

# functions between ordinals as integer vectors
struct Int𝔽Mor
  dom::Int𝔽; codom::Int𝔽; vals::Vector{Int}
end
(f::Int𝔽Mor)(i) = f.vals[i]
isvalid(f::Int𝔽Mor) = length(f.vals) == f.dom.n && all(f(x) ∈ f.codom for x in f.dom)
```
Catlab version (run in a fresh Julia session — Catlab exports its own `compose`, `id`, `FinFunction`, …):
```julia
# Catlab
using Catlab
g = FinFunction([3, 3], 3)     # {1,2} → {1,2,3}, both sent to 3
g(1)                            # 3
dom(g), codom(g)
preimage(g, 3)                  # [1, 2]
```
tab: Lean
```lean
-- functions are primitive: `f : S → T`
def f : Fin 2 → Fin 3 := fun _ => 2
#check @Function.Injective
#check @Function.Surjective
#check @Function.Bijective
-- preimage of a set (and of a point)
#check @Set.preimage      -- f ⁻¹' t
example (f : ℕ → ℕ) (t : ℕ) : Set ℕ := f ⁻¹' {t}
```
tab: Haskell
```haskell
-- functions are primitive: the arrow type a -> b
f :: Bool -> Int
f True  = 3
f False = 3

-- an element x :: a is a function () -> a
elemAsFn :: a -> (() -> a)
elemAsFn x = \() -> x

-- a finite function stored as a table (Kittenlab style)
import qualified Data.Map as M
newtype FinFn a b = FinFn (M.Map a b)
apply :: Ord a => FinFn a b -> a -> b
apply (FinFn m) a = m M.! a
```
````
