#definition #example #program

Let $S$ and $T$ be [[Set|sets]]. A **function** from $S$ to $T$ is a [[Relation]] $F \subseteq S \times T$ such that for all $s \in S$ there exists a *unique* $t \in T$ with $(s, t) \in F$. We write $F : S \to T$, $F(s) = t$ or $s \mapsto t$. $S$ is the **domain**, $T$ the **codomain**. For $t \in T$ the **preimage** of $t$ along $F$ is $\{s \in S \mid F(s) = t\}$.

> Sources: 7 Sketches Definition 1.22, Example 1.23, 1.29; Kittenlab Lecture 1 & 3; DaoFP §1.1 ("Types and Functions"), §2.2 ("Function application").

## Special kinds of function

| name | arrow | condition |
|---|---|---|
| [[Surjection|surjective]] | $\twoheadrightarrow$ | every $t$ has some $s$ with $F(s) = t$ |
| [[Injection|injective]] | $\hookrightarrow$ | $F(s_1) = F(s_2)$ implies $s_1 = s_2$ |
| [[Bijection|bijective]] | $\xrightarrow{\ \sim\ }$ | both |

The [[Identity Function]] $\mathrm{id}_X(x) = x$ is bijective. Functions compose ([[Function Composition]]), and sets with functions form the [[Category of Sets]] $\mathbf{Set}$. A function to the empty set forces the domain to be empty ([[7S Exercise 1.25]]).

## Elements as functions (Example 1.29, DaoFP §1.3)

An element $x \in X$ is the same as a function $\{1\} \to X$ (a [[Global Element]]). Evaluating $F$ at $x$ is then composition: $F(x) = x \mathbin{;} F$. DaoFP takes this further: in an arbitrary category, a "generalized element" of $X$ is any arrow into $X$.

## Kittenlab: functions as data structures

A morphism of [[Finite Set|finite sets]] can be stored as a dictionary of values; a function between ordinals $\{1..n\} \to \{1..m\}$ as a vector of integers. Not every value of the Julia type is a valid function — validity has to be checked (`isvalid`), since Julia's types cannot narrow the value space enough ("languages whose types can, don't have well-maintained BLAS bindings"). For infinite sets, $A \to B$ is the set of Julia callables $f$ with $f(a) \in B$ for all $a \in A$; membership is not checkable.

## DaoFP: types and functions

In programming a function $f : A \to B$ is an arrow between *types*; the type is a set of values and the function a total, deterministic mapping. "There are no functions out of the empty set … except the empty function"; DaoFP's [[Initial Object|Void]] and [[Terminal Object|unit]] types are the categorical shadow of the sets $\varnothing$ and $\{1\}$.

````tabs
tab: Julia
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
