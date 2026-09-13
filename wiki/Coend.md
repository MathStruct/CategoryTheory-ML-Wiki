#definition #theorem #example #program

The **coend** of a functor $P : \mathcal{C}^{\mathrm{op}} \times \mathcal{C} \to \mathcal{D}$ (a [[Profunctor]] when $\mathcal{D} = \mathbf{Set}$) is the "sum of its diagonal entries" $P\langle x, x\rangle$, corrected for double counting. A **cowedge** is an object $d$ with injections $i_x : P\langle x, x \rangle \to d$ such that for every $f : x \to y$

$$
i_x \circ P\langle f, \mathrm{id}_y \rangle = i_y \circ P\langle \mathrm{id}_x, f \rangle : P\langle y, x \rangle \to d
$$

(the two ways of "extending" a common ancestor $P\langle y, x \rangle$ agree). The coend $\int^{x : \mathcal{C}} P\langle x, x \rangle$ is the universal cowedge: every cowedge $(d, g_x)$ factors uniquely as $g_x = h \circ i_x$.

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}
 & P\langle y, x \rangle \arrow[dl, "{P\langle \mathrm{id}, f \rangle}"'] \arrow[dr, "{P\langle f, \mathrm{id} \rangle}"] & \\
P\langle y, y \rangle \arrow[dr, "i_y"'] & & P\langle x, x \rangle \arrow[dl, "i_x"] \\
 & \int^x P\langle x, x \rangle \arrow[d, "h", dashed] & \\
 & d &
\end{tikzcd}
\end{document}
```

> Sources: DaoFP §17.2 ("Coends": "Extranatural transformations", "Profunctor composition using coends", "Colimits as coends"), §17.4–17.6, §17.9 ("Existential lens"), §17.11 ("Important Formulas"), Exercises 17.2.1–17.2.3; 7 Sketches §4.3.2 (profunctor composition as a [[Quantale|quantale]]-valued matrix product — the preorder shadow of a coend).

- **In $\mathbf{Set}$**: take the disjoint union of all $P\langle x, x\rangle$ and identify $a \in P\langle x, x\rangle$ with $b \in P\langle y, y \rangle$ whenever some $c \in P\langle y, x\rangle$ and $f : x \to y$ satisfy $P\langle \mathrm{id}, f\rangle(c) = b$, $P\langle f, \mathrm{id}\rangle(c) = a$. On a [[Discrete Category]] the cowedge condition is empty and the coend is a plain [[Coproduct]] — the trace of a matrix.
- **Extranatural transformations**: a family $\alpha_{cd} : P\langle c, c\rangle \to Q\langle d, d\rangle$ satisfying two "diamond" conditions; the cowedge condition is extranaturality into the constant profunctor $\Delta_d$ ([[DaoFP Exercise 17.2.1]]). The coend is universal among extranatural $P \to \Delta_c$.
- **Profunctor composition** ([[Category of Profunctors]], [[Bicategory of Profunctors]]): $(P \diamond Q)\langle a, b\rangle = \int^{x} Q\langle a, x\rangle \times P\langle x, b\rangle$; in Haskell `data Procompose p q a b where Procompose :: q a x -> p x b -> Procompose p q a b` — an *existential* type, `data Coend p where Coend :: p x x -> Coend p`, and parametricity enforces the cowedge condition for free ([[DaoFP Exercise 17.2.2]], [[DaoFP Exercise 17.2.3]]).
- **Colimits as coends**: for a profunctor $P\langle x, y \rangle = F y$ ignoring its first argument, a cowedge is a [[Cocone]], so $\int^x F x = \mathrm{colim}\, F$ (a matrix with identical rows: the trace is the sum of the vector). Dually [[End|ends]] are limits and products.
- **Calculus**: co-continuity of the hom-functor pulls coends out as ends, $\mathcal{C}(\int^a P\langle a, a\rangle, d) \cong \int_a \mathcal{C}(P\langle a, a\rangle, d)$ — the mapping-out property; the **Fubini rule** lets double (co)ends be swapped or merged into one over $\mathcal{C} \times \mathcal{D}$; the [[Ninja Yoneda Lemma|ninja co-Yoneda lemma]] $\int^x \mathcal{C}(x, a) \times F x \cong F a$ "integrates against a delta function". Applications: [[Day Convolution]], the [[Terminal Coalgebra|existential type `Nu`]], the existential [[Lens]] $\int^c \mathcal{C}(s, c \times a) \times \mathcal{C}(c \times b, t)$.

````tabs
tab: Julia
```julia
using Catlab
# coend of a Set-valued profunctor on a finite category = colimit-style quotient: for a
# discrete category it is just the disjoint union of the diagonal sets
Pdiag = [FinSet(2), FinSet(3), FinSet(1)]        # P⟨x,x⟩ for x = 1,2,3, no non-identity arrows
ob(coproduct(Pdiag))                             # FinSet(6) = ∫^x P⟨x,x⟩ (coproduct of a list)
# with arrows the cowedge condition is a coequalizer of the two extensions (cf. Finite Colimits in Set)
```
tab: Lean
```lean
import Mathlib
open CategoryTheory
-- Mathlib has no dedicated coend API; coends are colimits over the twisted arrow category
#check @CategoryTheory.Limits.colimit
#check @CategoryTheory.Functor.leftKanExtension   -- Kan extensions, computed by coends
```
tab: Haskell
```haskell
{-# LANGUAGE GADTs, RankNTypes #-}
data Coend p where
  Coend :: p x x -> Coend p                     -- exists x. p x x
mapOutCoend :: (forall x. p x x -> c) -> Coend p -> c   -- the universal property
mapOutCoend f (Coend pxx) = f pxx

data Procompose p q a b where                   -- (p ⋄ q) a b = ∫^x q a x × p x b
  Procompose :: q a x -> p x b -> Procompose p q a b
```
````
