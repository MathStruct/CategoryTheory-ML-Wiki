#definition #example

A **symmetric monoidal category** is a [[Monoidal Category]] $(\mathcal{C}, \otimes, I, \alpha, \lambda, \rho)$ equipped with a natural isomorphism $\gamma_{a,b} : a \otimes b \to b \otimes a$ (the *symmetry*, *swap* or *braiding*) with $\gamma_{b,a} \circ \gamma_{a,b} = \mathrm{id}$, compatible with the associator. The full definition, coherence laws and examples live in [[Monoidal Category]]; this note records the programmer's view.

> Sources: DaoFP §4.4 ("Symmetric Monoidal Category": sums), §5.3 ("Monoidal Category": products, "tensor product as the lowest common denominator of product and sum"); 7 Sketches Definition 4.45, §4.4.3; Kittenlab.

- **From sums** ([[Sum Type]], [[Coproduct]]): $a + 0 \cong a$, $a + b \cong b + a$, $(a + b) + c \cong a + (b + c)$, functorial — a *cocartesian* SMC.
- **From products** ([[Cartesian Category]], [[Product]]): $1 \times a \cong a$, $a \times b \cong b \times a$, $(a \times b) \times c \cong a \times (b \times c)$ — a *cartesian* SMC. A category can carry both at once.
- A general tensor $\otimes$ has an introduction rule (you need both $a$ and $b$) but *no elimination rule*: "once created, a tensor product forgets how it was created" — no projections, no injections. Morphisms $f \otimes g$ are actions performed *in parallel*, as opposed to serial composition. Non-symmetric examples exist (e.g. [[Day Convolution]] over a non-symmetric monoidal category, endofunctor composition).
- Symmetry is only up to isomorphism: `swap :: (a,b) -> (b,a)` changes how information is accessed, not its content.
- [[Monoid Object|Monoids]] can be defined in any monoidal category; [[Wiring Diagram|wiring diagrams]] are the graphical language of SMCs; [[Compact Closed Category|compact closed]] and [[Hypergraph Category|hypergraph]] categories are SMCs with extra structure.

````tabs
tab: Julia
```julia
using Catlab
@present P(FreeSymmetricMonoidalCategory) begin (A, B)::Ob end
A, B = generators(P)
σ = braid(A, B)                       # γ_{A,B} : A ⊗ B → B ⊗ A
σ ⋅ braid(B, A)                       # composes to the identity in the theory
```
tab: Lean
```lean
import Mathlib
#check @CategoryTheory.SymmetricCategory           -- braiding β with β ≫ β = id
#check @CategoryTheory.BraidedCategory.braiding
#check @CategoryTheory.symmetricOfChosenFiniteProducts        -- products give a symmetric structure
```
tab: Haskell
```haskell
-- the two symmetric monoidal structures on Hask
swapP :: (a, b) -> (b, a)
swapP (x, y) = (y, x)
swapS :: Either a b -> Either b a
swapS = either Right Left
-- parallel composition of actions
par :: (a -> a') -> (b -> b') -> (a, b) -> (a', b')
par f g (x, y) = (f x, g y)
```
````
