#definition #example

For an object $x$ of a [[Category]] $\mathcal C$, the **endomorphism monoid** is $\mathrm{End}(x) := \mathcal C(x, x)$ with composition as multiplication and $\mathrm{id}_x$ as unit. Its invertible elements form the **automorphism group** $\mathrm{Aut}(x) \subseteq \mathrm{End}(x)$. In other words, $\mathrm{End}(x)$ is the full [[Subcategory]] of $\mathcal C$ on the single object $x$ — a one-object category, i.e. a [[Monoid]] (CTfS Slogan 4.2.1.2) — and $\mathrm{Aut}(x)$ is its largest sub-[[Groupoid]].

> Sources: CTfS §4.2.1 (Exercises 4.2.1.8–4.2.1.11), Slogans 4.2.1.2, 4.2.1.5; 7 Sketches §3.2 (monoids as one-object categories).

## Examples

- **Functions on a finite set** ([[CTfS Chapter 4 Exercises#Exercise 4.2.1.10|CTfS Exercise 4.2.1.10]]): for $S = \{1, 2, 3, 4\}$ in $\mathbf{Set}$, $\mathrm{End}(S)$ has $4^4 = 256$ elements, while $\mathrm{Aut}(S)$ is the symmetric group $\Sigma_4$ with $4! = 24$ elements. The inclusion $\mathrm{Aut}(S) \hookrightarrow \mathrm{End}(S)$ is a *proper* submonoid: a monoid need not be the underlying monoid of any group.
- **Symmetries of a square** ([[CTfS Chapter 4 Exercises#Exercise 4.2.1.11|CTfS Exercise 4.2.1.11]]): in $\mathbf{Grph}$ take the graph with vertices $1, 2, 3, 4$ and arrows in both directions along the 4-cycle $1 - 2 - 4 - 3 - 1$. Its automorphisms are the vertex permutations preserving adjacency — the 8 symmetries of a square, the dihedral group $D_4$ of [[Group Action]].
- **A monoid as a category**: if $\mathcal M$ is a monoid viewed as a one-object category with object $\bullet$, then $\mathrm{End}(\bullet) = \mathcal M$ — every monoid is an endomorphism monoid. Every monoid is even an endomorphism monoid in $\mathbf{Set}$: $M$ embeds in $\mathrm{End}(M)$ by $m \mapsto (m \star -)$ (Cayley).
- **Linear maps**: $\mathrm{End}(\mathbb R^n)$ is the monoid of $n \times n$ matrices under multiplication; $\mathrm{Aut}(\mathbb R^n) = GL_n$.
- **In a preorder** every hom-set has at most one element, so $\mathrm{End}(x) = \mathrm{Aut}(x) = \{\mathrm{id}_x\}$.

## Why it matters

A [[Monoid Action]] of $M$ on $S$ is the same thing as a monoid homomorphism $M \to \mathrm{End}(S)$, and a [[Group Action]] of $G$ is a group homomorphism $G \to \mathrm{Aut}(S)$ — this is the proof of the [[Finite State Machine]] proposition. More generally, $\mathrm{Aut}(x)$ acts on every hom-set $\mathcal C(x, y)$ by precomposition, and a [[Functor]] $F$ restricts to homomorphisms $\mathrm{End}(x) \to \mathrm{End}(Fx)$ and $\mathrm{Aut}(x) \to \mathrm{Aut}(Fx)$ (functors preserve [[Isomorphism|isomorphisms]]). "Symmetry" in the categorical sense means an automorphism group.

````tabs
tab: Julia
**Docs:** [FinSets](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.FinSets) · [C-set morphisms](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.CSets) · [ACSets API](https://algebraicjulia.github.io/ACSets.jl/stable/api/) · [Graphs](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/graphs/)
```julia
using Catlab
# End(S) and Aut(S) for S = {1,2,3,4} (CTfS Exercise 4.2.1.10)
S = FinSet(4)
endos = [FinFunction(collect(f), S, S) for f in Iterators.product(fill(1:4, 4)...)]
autos = filter(f -> allunique(collect(f)), endos)
length(endos), length(autos)                                 # (256, 24)
# Aut of the symmetric 4-cycle 1-2-4-3-1 (CTfS Exercise 4.2.1.11): graph automorphisms
C4 = @acset Graph begin V = 4; E = 8
  src = [1, 2, 1, 3, 2, 4, 3, 4]; tgt = [2, 1, 3, 1, 4, 2, 4, 3] end
length(isomorphisms(C4, C4))                                  # 8 = |D₄|
```
tab: Lean
```lean
import Mathlib
open CategoryTheory
#check @End         -- End X := X ⟶ X, a Monoid under composition
#check @Aut         -- Aut X := X ≅ X, a Group
#check @Aut.unitsEndEquivAut   -- (End X)ˣ ≃* Aut X: automorphisms are the invertible endomorphisms
example : Fintype.card (Equiv.Perm (Fin 4)) = 24 := by simp [Fintype.card_perm]
example : Fintype.card (Fin 4 → Fin 4) = 256 := by simp
```
tab: Haskell
```haskell
import Data.List (permutations, nub)
import Data.Monoid (Endo(..))       -- the endomorphism monoid of a Haskell type

-- End({1,2,3,4}) as lookup tables, Aut as the bijective ones
endos :: [[Int]]
endos = sequence (replicate 4 [1..4])
autos :: [[Int]]
autos = filter (\f -> length (nub f) == 4) endos
-- (length endos, length autos) == (256, 24); autos == permutations [1..4] up to order

twiceThenInc :: Endo Int
twiceThenInc = Endo (+1) <> Endo (*2)   -- composition: appEndo twiceThenInc 5 == 11
```
````
