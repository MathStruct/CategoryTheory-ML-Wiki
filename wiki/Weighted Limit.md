#definition #theorem #example

Ordinary cones pick single "wires" $\gamma_j : 1 \to \mathcal{C}(x, D j)$ from hom-sets; the set of cones with apex $x$ is $[\mathcal{J}, \mathbf{Set}](\Delta_1, \mathcal{C}(x, D-))$. In an [[Enriched Category|enriched]] setting there is no constant $\mathcal{V}$-functor $\Delta_1$ (the unit $I$ need not be terminal), so one "smears the singularity" with a **weight** $W : \mathcal{J} \to \mathcal{V}$ selecting a thicker "cylinder" in each hom-object. A **weighted (indexed) limit** $\lim^W D$ of $D : \mathcal{J} \to \mathcal{C}$ is defined by

$$
\mathcal{C}(x, \lim{}^W D) \cong [\mathcal{J}, \mathcal{V}](W, \mathcal{C}(x, D-)),
$$

and dually the **weighted colimit** by $\mathcal{C}(\mathrm{colim}^W D, x) \cong [\mathcal{J}^{\mathrm{op}}, \mathcal{V}](W, \mathcal{C}(D-, x))$ with $W : \mathcal{J}^{\mathrm{op}} \to \mathcal{V}$. Ordinary (*conical*) limits are the case $W = \Delta_1$.

> Sources: DaoFP §20.5 ("Weighted Limits"), §20.6 ("Ends as Weighted Limits"), §20.7 ("Kan Extensions"), §20.8 ("Useful Formulas"), Exercises 20.6.1, 20.6.2, 20.7.1; §20.4 (enriched Yoneda).

- **Formulas** ([[DaoFP Exercise 20.6.2]]): $\lim^W D \cong \int_j W j \pitchfork D j$ and $\mathrm{colim}^W D \cong \int^j W j \cdot D j$ (power and copower, [[Kan Extension]]).
- **Ends as weighted limits** (§20.6): for $P : \mathcal{C}^{\mathrm{op}} \otimes \mathcal{C} \to \mathcal{D}$, $\int_c P\langle c, c\rangle = \lim^{\mathrm{Hom}_\mathcal{C}} P$ with the hom-functor as weight; proof by the Yoneda trick, Fubini and [[Ninja Yoneda Lemma|ninja Yoneda]] over $c'$. Dually $\int^c P\langle c, c\rangle = \mathrm{colim}^{\mathrm{Hom}_{\mathcal{C}^{\mathrm{op}}}} P$ ([[DaoFP Exercise 20.6.1]]). This *defines* [[End|ends]] and [[Coend|coends]] in the enriched setting.
- **Kan extensions as weighted (co)limits** (§20.7): $(\mathrm{Ran}_P F)\, e = \lim^{\mathcal{B}(e, P-)} F$ and $(\mathrm{Lan}_P F)\, e = \mathrm{colim}^{\mathcal{B}(P-, e)} F$ ([[DaoFP Exercise 20.7.1]]) — the weights are representables, replacing the functor from the terminal category $\mathbf{1}$ used for ordinary (co)limits.
- **Enriched Yoneda** (§20.4): weak form $\mathcal{V}\text{-nat}(\mathcal{C}(c, -), F) \cong \mathcal{V}(I, F c)$ (a *set* of $\mathcal{V}$-natural transformations vs. the global elements of $F c$); strong form $\int_x [\mathcal{C}(c, x), F x] \cong F c$ as an object of $\mathcal{V}$, using the internal hom.

````tabs
tab: Julia
```julia
using Catlab
# a weighted limit in Set with a discrete shape J = {1, 2}: lim^W D = ∏_j (W j ⋔ D j) = ∏_j D j^{W j}
D = [FinSet(2), FinSet(3)]; W = [2, 1]            # weights = sizes of W j
prod(length(D[j])^W[j] for j in 1:2)              # 2^2 · 3^1 = 12 elements
# conical limit (W = Δ1) is the plain product: 2 · 3 = 6
ob(product(D))
```
tab: Lean
```lean
import Mathlib
open CategoryTheory
-- Mathlib has ordinary (conical) limits; weighted limits appear through ends / Kan extensions
#check @CategoryTheory.Limits.limit
#check @CategoryTheory.Functor.ran            -- (Ran_P F) e = lim^{B(e, P-)} F
```
tab: Haskell
```haskell
{-# LANGUAGE RankNTypes #-}
-- a weighted cone with apex x: a natural transformation W ⇒ C(x, D-), i.e. for each j
-- a function from the weight W j to arrows x -> D j; in Hask with W j = weights as types:
type WeightedCone w d x = forall j. w j -> (x -> d j)
```
````
