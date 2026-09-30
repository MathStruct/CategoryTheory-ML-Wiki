#definition #example

A **double category** $\mathbb D$ has objects, two kinds of arrows between them — **vertical** arrows $f : A \to A'$ and **horizontal** arrows $M : A \nrightarrow B$ — and **squares**

$$
\begin{array}{ccc}
A & \overset{M}{\nrightarrow} & B \\
{\scriptstyle f}\downarrow & \alpha & \downarrow{\scriptstyle g} \\
A' & \underset{N}{\nrightarrow} & B'
\end{array}
$$

which compose both vertically and horizontally, subject to an interchange law. Vertical arrows form a category, horizontal arrows compose (often only up to coherent isomorphism — a *pseudo* double category), and a square relates a horizontal arrow on top to one on the bottom *along* a pair of vertical arrows. A [[2-Category]] is a double category with only identity vertical arrows; a [[Bicategory]] is the horizontal part of a pseudo double category.

> Sources: Ehresmann (1963); Grandis & Paré, *Limits in double categories* (1999); Myers, *Double Categories of Open Dynamical Systems* [arXiv:2005.05956](https://arxiv.org/abs/2005.05956) ([[Double Categories of Open Dynamical Systems|notes]]); Myers, *Categorical Systems Theory* (book draft); Capucci et al. [arXiv:2105.06332](https://arxiv.org/abs/2105.06332) ([[Towards Foundations of Categorical Cybernetics|notes]]) §2 (string diagrams in a double category for $\mathbf{Para}$ and $\mathbf{CoPara}$); Catlab.jl `ThMonoidalDoubleCategory`.

## Examples

- **Spans/cospans and maps**: objects are sets, horizontal arrows spans $A \leftarrow S \to B$, vertical arrows functions, squares maps of spans. Open systems glued along boundaries ([[Cospan]], [[Decorated Cospan]], [[Structured Cospan]]) naturally form double categories: horizontal = *composition of systems*, vertical = *maps between interfaces*.
- **Quintets** of a 2-category, and **commutative squares** in any category.
- **Profunctors and functors**: horizontal = [[Profunctor|profunctors]], vertical = functors — the setting in which [[Optic|optics]] and lenses are naturally drawn.

## Why systems theory wants two directions

Myers' categorical systems theory builds double categories of open dynamical systems with two kinds of morphism: **covariant** ones (trajectories, steady states, periodic orbits — "behaviours") and **contravariant** ones, which *plug variables of some systems into parameters of other systems*. The second kind is how an optimiser attaches to the parameters of a learner, or a controller to a plant: it is not composition *side by side* (horizontal) but a morphism *over* another system (vertical). Capucci et al. likewise draw $\mathbf{Para}$ (parameters coming in from above) and $\mathbf{CoPara}$ (coparameters leaving below) as diagrams in one double category.

````tabs
tab: Julia
**Docs:** [Theories & presentations](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/theories/)
```julia
using Catlab
# Catlab has a theory of (monoidal) double categories; horizontal arrows are "proarrows".
@present D(FreeSymmetricMonoidalDoubleCategory) begin
  (A, B, A′, B′)::Ob
  f::Hom(A, A′); g::Hom(B, B′)          # vertical arrows
  M::Pro(A, B); N::Pro(A′, B′)          # horizontal arrows (proarrows)
  α::Cell(M, N, f, g)                   # a square from M to N along f and g
end
generators(D, :Cell)                     # [α]
```
tab: Lean
```lean
import Mathlib
-- A (strict) double category as data: objects, vertical and horizontal arrows, squares.
structure DoubleCatData where
  Ob : Type
  V : Ob → Ob → Type                     -- vertical arrows
  H : Ob → Ob → Type                     -- horizontal arrows
  Sq : ∀ {A B A' B' : Ob}, H A B → H A' B' → V A A' → V B B' → Type
```
tab: Haskell
```haskell
-- Squares between spans of finite sets: the prototypical double category
data Span a b s = Span (s -> a) (s -> b)

-- a square from a span over (a, b) to a span over (a', b') along f, g, given by h : s -> s'
data Square a b s a' b' s' = Square (a -> a') (b -> b') (s -> s')

checkSquare :: (Eq a', Eq b') => [s] -> Span a b s -> Span a' b' s' -> Square a b s a' b' s' -> Bool
checkSquare ss (Span l r) (Span l' r') (Square f g h) =
  and [ f (l s) == l' (h s) && g (r s) == r' (h s) | s <- ss ]
```
````
