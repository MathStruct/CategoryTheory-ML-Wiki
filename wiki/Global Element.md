#definition #example

An element $x$ of a [[Set]] $X$ can be identified with the [[Function]] $\{1\} \to X$ sending $1 \mapsto x$. The three functions $\{1\} \to \{1,2,3\}$ correspond to the three elements. Given $F : X \to Y$, evaluating $F$ at $x$ is the composite $x \mathbin{;} F : \{1\} \to Y$.

> Sources: 7 Sketches Example 1.29; DaoFP §1.3 ("Elements"), §2.2; Kittenlab Lecture 12.

In an arbitrary [[Category]] with a [[Terminal Object]] $1$, an arrow $1 \to X$ is called a **global element** of $X$; DaoFP calls any arrow $A \to X$ a *generalized element* of shape $A$ ("we can only learn about an object by probing it with arrows"). In $\mathbf{Set}$, $X \cong \mathrm{Hom}(1, X)$ — a triviality that is the seed of the [[Yoneda Lemma]] (Kittenlab Lecture 12: "$A^1 \cong A$"). In a [[Topos]], global elements of the [[Subobject Classifier]] $\Omega$ are the truth values. For [[C-Set|C-sets]] the analogue is a morphism out of a [[Representable Functor|representable]] $y_X \to F$, which picks out an element of $F(X)$.

````tabs
tab: Julia
```julia
using Catlab
X = FinSet(3)
x = FinFunction([2], X)          # the element 2 as a map 1 → X
F = FinFunction([5, 6, 7], 7)
compose(x, F)                     # evaluates F at 2: FinFunction([6], 7)
```
tab: Lean
```lean
-- elements of X ↔ functions PUnit → X
example (X : Type) : X ≃ (PUnit → X) :=
  ⟨fun x _ => x, fun f => f PUnit.unit, fun _ => rfl, fun _ => rfl⟩
```
tab: Haskell
```haskell
elemAsArrow :: a -> (() -> a)
elemAsArrow x = const x

arrowAsElem :: (() -> a) -> a
arrowAsElem f = f ()
```
````
