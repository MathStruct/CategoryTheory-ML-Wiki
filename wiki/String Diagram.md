#definition #example #annotation

**String diagrams** are the Poincaré dual of the usual diagrams of [[Category of Categories|categories, functors and natural transformations]]: categories become *areas* of the plane, functors become *lines* separating areas, and [[Natural Transformation|natural transformations]] become *dots* joining line segments. Read bottom-up and left-to-right. Parallel vertical lines are functor composition; stacking is vertical composition; placing side by side is horizontal composition, and the **interchange law** says the exact heights of dots do not matter — "we are free to slide natural transformations like beads on a string". Whiskering is horizontal composition with an identity, which need not be drawn.

> Sources: DaoFP §15.1 ("String Diagrams": "String diagrams for the monad", "String diagrams for the adjunction"), Exercises 15.1.1, 15.2.1; §15.2, §15.4 (monad transformers drawn as string diagrams); 7 Sketches §2.2.3, §4.4.4 ([[Wiring Diagram|wiring diagrams]] are string diagrams for [[Monoidal Category|monoidal categories]] — a monoidal category is a one-object 2-category).

- **Haskell reading**: a dot $\alpha : F \to G$ is `alpha :: forall x. F x -> G x`; vertical composition is `.`; whiskering $\beta \circ F$ is `beta` instantiated at `F x`, $G \circ \alpha$ is `fmap alpha`.
- **Monad**: $\eta$ is a dot spawning a $T$-string from nothing (the identity functor is not drawn), $\mu$ a dot merging two $T$-strings into one; the unit laws are pictures where an $\eta$-appendage is retracted by yanking, associativity is the two ways of merging three strings.
- **[[Adjunction]]**: the unit is a *cup* $\eta : \mathrm{Id} \to R L$, the counit a *cap* $\varepsilon : L R \to \mathrm{Id}$; the triangle (zigzag) identities say a zigzag string can be pulled straight: `triangle = fmap counit . unit :: R x -> R x` is the identity ([[DaoFP Chapter 15 Exercises#Exercise 15.1.1|DaoFP Exercise 15.1.1]]).
- Monad from an adjunction: $\mu = R \varepsilon L$ is a cap between two $L R$ pairs, sandwiched by $R \dots L$ ([[Monads from Adjunctions]]). Compare the snake equations of a [[Compact Closed Category]].

````tabs
tab: Julia
```julia
using Catlab
# Catlab draws string (wiring) diagrams for monoidal categories; a one-object 2-category is the same thing
@present P(FreeSymmetricMonoidalCategory) begin (T,)::Ob; η::Hom(munit(), T); μ::Hom(T ⊗ T, T) end
T, η, μ = generators(P)
left_unit = (η ⊗ id(T)) ⋅ μ          # the string diagram for μ ∘ (η ∘ T)
d = to_wiring_diagram(left_unit)      # can be rendered with Catlab.Graphics
```
tab: Lean
```lean
import Mathlib
open CategoryTheory
#check @CategoryTheory.whiskerLeft      -- F ◁ α
#check @CategoryTheory.whiskerRight     -- α ▷ G
#check @CategoryTheory.NatTrans.exchange  -- the interchange law
#check @CategoryTheory.Adjunction.left_triangle
```
tab: Haskell
```haskell
-- vertical composition of natural transformations is function composition
type Nat f g = forall x. f x -> g x
vcomp :: Nat g h -> Nat f g -> Nat f h
vcomp beta alpha = beta . alpha
-- whiskering: beta at (F x) is just beta; G ∘ alpha is fmap alpha
```
````
