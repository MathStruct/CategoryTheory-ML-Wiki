#definition #example #theorem

For a set $A$, an **$A$-indexed set** is a family $\{S_a\}_{a \in A}$ of sets, one for each $a \in A$. A **mapping** $\{S_a\} \to \{T_a\}$ of $A$-indexed sets is a family of functions $f_a : S_a \to T_a$, one per index. Viewing $A$ as a [[Discrete Category]], an $A$-indexed set is exactly a [[Functor]] $A \to \mathbf{Set}$ — a [[C-Set]] on the discrete schema $\mathrm{Disc}(A)$ — and a mapping is a [[Natural Transformation]] (naturality is vacuous, as there are no non-identity arrows; CTfS Exercise 4.3.3.3).

> Sources: CTfS §2.7.6.10 (Example 2.7.6.11, Definition 2.7.6.12, Exercises 2.7.6.13–2.7.6.14), Exercise 4.3.3.3, Exercise 4.6.2.2; DaoFP §6 (families of types); [[Dependent Type]].

## Examples

- **Classrooms** (CTfS Example 2.7.6.11): index by the classrooms $A$ of a school; $S_a$ is the set of seats in room $a$, and $T_a$ the set of people in room $a$ at 10 a.m. A seating is a mapping $\{T_a\} \to \{S_a\}$ that seats every person *in their own room*.
- **People by city** (CTfS Exercise 4.6.2.2): $S_{\mathrm{BOS}} = \{\text{Abby}, \text{Bob}, \text{Casandra}\}$, $S_{\mathrm{NYC}} = \varnothing$, $S_{\mathrm{LA}} = \{\text{John}, \text{Jim}\}$, $S_{\mathrm{DC}} = \{\text{Abby}, \text{Carla}\}$ — drawn as a histogram with one column per city.
- **Fibers of a function**: any function $\pi : E \to A$ gives the $A$-indexed set of its [[Fiber|fibers]] $\{\pi^{-1}(a)\}$.
- **Vector bundles, dependent types**: a type family $B : A \to \mathrm{Type}$ is an indexed set; a [[Dependent Type|dependent function]] $(a : A) \to B\,a$ picks one element in each $S_a$.

## Indexed sets are sets over the index

**Theorem** ([[CTfS Chapter 2 Exercises#Exercise 2.7.6.14|CTfS Exercise 2.7.6.14]]). $A$-indexed sets are equivalent to relative sets over $A$, i.e. $\mathbf{Set}^A \simeq \mathbf{Set}_{/A}$ ([[Slice Category]]).

*Proof.* Given $\{S_a\}$, form the disjoint union $E := \coprod_{a \in A} S_a = \{(a, s) \mid s \in S_a\}$ with projection $\pi(a, s) = a$. Given $\pi : E \to A$, take fibers $S_a := \pi^{-1}(a)$. A family of maps $f_a$ assembles to a map $\coprod f_a$ over $A$, and a map over $A$ restricts to the fibers. Taking fibers of a disjoint union returns the original family, and the disjoint union of the fibers of $\pi$ is isomorphic to $E$ over $A$ (not literally equal — hence *equivalence*, not isomorphism). $\blacksquare$

Categorically, $\coprod_a S_a \to A$ is the [[Category of Elements]] of the functor $S : \mathrm{Disc}(A) \to \mathbf{Set}$ — the "histogram" turned into a single set with a label on each element (CTfS Exercise 4.6.2.2). This is the discrete case of the Grothendieck correspondence $\mathbf{Set}^{\mathcal C} \simeq \mathrm{DiscFib}(\mathcal C)$, and in type theory it is the equivalence between families $B : A \to \mathrm{Type}$ and the projection $\Sigma_{a : A} B\, a \to A$. [[Multiset|Multisets]] are the sets over $B$ whose fibers are all nonempty.

````tabs
tab: Julia
**Docs:** [FinSets](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.FinSets) · [ACSets API](https://algebraicjulia.github.io/ACSets.jl/stable/api/) · [Theories & presentations](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/theories/)
```julia
using Catlab
# an A-indexed set as a dictionary, and the equivalent set over A (CTfS Exercise 2.7.6.14)
S = Dict(:BOS => [:Abby, :Bob, :Casandra], :NYC => Symbol[], :LA => [:John, :Jim], :DC => [:Abby, :Carla])
cities = [:BOS, :NYC, :LA, :DC]
E = [(a, s) for a in cities for s in S[a]]                  # the disjoint union ∐ S_a
π = FinFunction([findfirst(==(a), cities) for (a, _) in E], length(cities))
length(E), [length(preimage(π, i)) for i in 1:4]            # (7, [3, 0, 2, 2]): fibres recover S
# as a C-set on the discrete schema with one object per city
@present SchCities(FreeSchema) begin (BOS, NYC, LA, DC)::Ob end
@acset_type Cities(SchCities)
@acset Cities begin BOS = 3; NYC = 0; LA = 2; DC = 2 end
```
tab: Lean
```lean
import Mathlib
-- an A-indexed set is a family S : A → Type; the total space is the Sigma type
variable {A : Type} (S : A → Type)
#check (Sigma S)                          -- ∐ₐ S a, with projection Sigma.fst : Sigma S → A
-- going back: the fibres of π : E → A
def fibre {E : Type} (π : E → A) (a : A) : Type := {e : E // π e = a}
#check @CategoryTheory.Over               -- relative sets over A
#check @Equiv.sigmaFiberEquiv             -- (Σ a, {e // π e = a}) ≃ E
```
tab: Haskell
```haskell
import qualified Data.Map as Map

-- an A-indexed set as a map from indices to lists, and its total space over A
type Indexed a s = Map.Map a [s]

total :: Indexed a s -> [(a, s)]            -- the disjoint union with its projection fst
total ix = [ (a, s) | (a, ss) <- Map.toList ix, s <- ss ]

fibres :: Ord a => [a] -> [(a, s)] -> Indexed a s   -- back again (empty fibres kept)
fibres as e = Map.fromList [ (a, [ s | (a', s) <- e, a' == a ]) | a <- as ]
```
````
