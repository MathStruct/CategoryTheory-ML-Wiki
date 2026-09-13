#definition

If $F : X \to Y$ and $G : Y \to Z$ are [[Function|functions]], their **composite** is the function $X \to Z$ defined by $x \mapsto G(F(x))$. It is often denoted $G \circ F$, but 7 Sketches prefers the *diagrammatic order* $F \mathbin{;} G$ ("$F$ then $G$"), which Catlab writes `compose(F, G)` or `F ⋅ G`.

> Sources: 7 Sketches Definition 1.28, Example 1.29; Kittenlab Lecture 2; DaoFP §2.1–2.2.

Composition is associative and has the [[Identity Function|identities]] as units — the axioms of a [[Category]]. Evaluating $F$ at an element $x : \{1\} \to X$ is the composite $x \mathbin{;} F$ (see [[Global Element]]). DaoFP: "composition is the essence of programming"; function application $f(a)$ is composition with the arrow $a : 1 \to A$; pre-composition $(- \circ f)$ and post-composition $(g \circ -)$ are themselves functions between hom-sets, and $(- \circ f)$ reverses the order of composition ([[DaoFP Chapter 2 Exercises#Exercise 2.1.3|DaoFP Exercise 2.1.3]]).

````tabs
tab: Julia
```julia
# Kittenlab Lecture 2 (diagrammatic order: f then g)
function compose(f::𝔽Mor, g::𝔽Mor)
  @assert f.codom == g.dom
  𝔽Mor(f.dom, g.codom, Dict(a => g(f(a)) for a in f.dom))
end

# Catlab
using Catlab
f = FinFunction([2, 2, 1], 2)
g = FinFunction([3, 1], 3)
compose(f, g)      # f ⋅ g : FinSet(3) → FinSet(3)
f ⋅ g == compose(f, g)
```
tab: Lean
```lean
#check @Function.comp        -- (g ∘ f) x = g (f x)
example (f : ℕ → ℤ) (g : ℤ → ℚ) : ℕ → ℚ := g ∘ f
-- in Mathlib categories, diagrammatic order is written f ≫ g
#check @CategoryTheory.CategoryStruct.comp
```
tab: Haskell
```haskell
-- Prelude: (.) :: (b -> c) -> (a -> b) -> a -> c
compose :: (b -> c) -> (a -> b) -> (a -> c)
compose g f = \x -> g (f x)

-- diagrammatic order, as in 7 Sketches: f ; g
(>>>) :: (a -> b) -> (b -> c) -> (a -> c)
f >>> g = g . f
```
````
