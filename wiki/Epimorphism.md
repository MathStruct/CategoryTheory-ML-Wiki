#definition #example

An arrow $f : a \to b$ is an **epimorphism** ("epi", drawn $a \twoheadrightarrow b$) if for every object $c$ and every pair $g_1, g_2 : b \to c$,
$$g_1 \circ f = g_2 \circ f \implies g_1 = g_2.$$
Equivalently, pre-composition $(- \circ f) : \mathcal{C}(b, c) \to \mathcal{C}(a, c)$ is injective for every $c$. To show $f$ is *not* epi, find $c$ and two different $g_1, g_2$ that agree after precomposing with $f$.

> Sources: DaoFP §2.5 ("Epimorphisms"), Exercise 2.5.1; 7 Sketches §1.4.2; Kittenlab Lecture 2.

**Intuition (DaoFP).** Mappings *out* of an object define its properties: think of elements of a finite target $c$ as colours painting $b$. If $f$ is not epi, its image may cover only the part of $b$ painted alike by $g_1$ and $g_2$, so the two agree on $a$ although they differ on $b$. "Of course, in an actual category there is no peeking inside objects."

- In $\mathbf{Set}$ epis are exactly the [[Surjection|surjections]] (`even :: Int -> Bool` covers all of `Bool`). Any arrow *to* the [[Terminal Object]] is epi ([[DaoFP Exercise 2.5.1]]).
- Epi is dual to [[Monomorphism]]: an epi in $\mathcal{C}$ is a mono in $\mathcal{C}^{\mathrm{op}}$. A [[Section and Retraction|retraction]] is always epi.
- Surjections out of $A$ = [[Partition|partitions]] of $A$; the [[Epi-Mono Factorization]] $A \twoheadrightarrow \mathrm{im}(f) \hookrightarrow B$ underlies [[Pushforward and Pullback of Partitions]].
- Epi + mono need not be iso (DaoFP; e.g. dense inclusions in $\mathbf{Top}$).
- **Via pushouts** (7 Sketches Definition 7.5): $f : A \to B$ is epi iff the square with $f$ twice and $\mathrm{id}_B$ twice is a [[Pushout]] (the cokernel pair of $f$ is trivial) — the exact dual of the pullback characterization of [[Monomorphism|monos]]. In a [[Topos]] "$f$ is epi" is expressed by the internal formula $\forall(t : T).\, \exists(s : S).\, f(s) = t$ ([[Internal Language of a Topos]], Example 7.74).

````tabs
tab: Julia
```julia
using Catlab
is_epic(FinFunction([1, 2, 2], 2))    # true: surjective
is_epic(FinFunction([1, 1, 1], 2))    # false
```
tab: Lean
```lean
#check CategoryTheory.Epi            -- class Epi f : ∀ g h, f ≫ g = f ≫ h → g = h
#check @CategoryTheory.epi_iff_surjective
```
tab: Haskell
```haskell
even' :: Int -> Bool                  -- an epimorphism in Hask
even' n = n `mod` 2 == 0
```
````
