#theorem #example #definition

The [[Subobject Classifier]] $\Omega$ of a [[Topos]] carries the **logical connectives** as morphisms $\wedge, \vee, \Rightarrow : \Omega \times \Omega \to \Omega$ and $\neg : \Omega \to \Omega$, each the characteristic map of a specific [[Subobject]] built from [[Limit|limits]] and [[Colimit|colimits]]. Applied pointwise they make each poset of [[Predicate|predicates]] $|\Omega^S|$ a [[Heyting Algebra]].

> Sources: 7 Sketches §7.2.3 ("Logic in the topos Set"), Eq. (7.18), Exercises 7.19–7.21; §7.4.2 ("Logic in a sheaf topos"), Eqs. (7.56)–(7.57), Example 7.58, Exercises 7.59–7.60; §7.4.6.

## In $\mathbf{Set}$ ($\Omega = \mathbb{B}$)

- **AND**: the element $(\mathsf{true}, \mathsf{true}) : 1 \to \mathbb{B} \times \mathbb{B}$ is a mono; its characteristic map sends only $(\mathsf{true}, \mathsf{true})$ to $\mathsf{true}$ — the truth table of $\wedge$. So $\wedge = \ulcorner (\mathsf{true}, \mathsf{true}) \urcorner$.
- **OR** classifies the subset $\{(\mathsf{t},\mathsf{t}), (\mathsf{t},\mathsf{f}), (\mathsf{f},\mathsf{t})\} \subseteq \mathbb{B} \times \mathbb{B}$, i.e. the union $\{\mathsf{true}\} \times \mathbb{B} \,\cup\, \mathbb{B} \times \{\mathsf{true}\}$ — a colimit of limits involving only $\Omega$ and $1$, hence available in every topos.
- **NOT** classifies the subobject $\{\mathsf{false}\} \subseteq \mathbb{B}$ ([[7S Exercise 7.19]]); **IMPLIES** $P \Rightarrow Q :\Leftrightarrow P = (P \wedge Q)$ classifies $\{(\mathsf{t},\mathsf{t}), (\mathsf{f},\mathsf{t}), (\mathsf{f},\mathsf{f})\}$, the equalizer of $\wedge$ and the first projection ([[7S Exercise 7.20]]).

## In a sheaf topos $\mathbf{Shv}(X)$ (truth values are open sets)

For $U, V \in \Omega(X) = \mathrm{Op}$:
$$U \wedge V := U \cap V, \qquad U \vee V := U \cup V, \qquad (U \Rightarrow V) := \bigcup \{R \in \mathrm{Op} \mid R \cap U \subseteq V\}, \qquad \neg U := (U \Rightarrow \mathsf{false}) = \mathrm{int}(X \setminus U).$$
$\mathsf{true} = X$ and $\mathsf{false} = \varnothing$ ([[7S Exercise 7.60]]). Implication is the hardest to picture; negation is the [[Interior Operator|interior]] of the complement. Example 7.58 on $X = \mathbb{R}$ with $U = (-\infty, 3)$, $V = (-4, 4)$: $U \wedge V = (-4, 3)$, $U \vee V = (-\infty, 4)$, $\neg U = (3, \infty)$, $\neg V = (-\infty, -4) \cup (4, \infty)$, $U \Rightarrow V = (-4, \infty)$, $V \Rightarrow U = U$.

The logic is **intuitionistic**: $U \subseteq \neg\neg U$ always, but $\neg\neg U \subseteq U$ can fail — for $U = \mathbb{R} \setminus \{0\}$, $\neg U = \varnothing$ and $\neg\neg U = \mathbb{R}$ ([[7S Exercise 7.59]]). Excluded middle $U \vee \neg U = X$ fails for the same $U$.

## Quantifiers and modalities

$\forall$ and $\exists$ are described in [[Quantification]]; the "assuming $p$" operators in [[Modality]]. The formal language these connectives belong to, and its compilation into statements about sheaves, is the [[Internal Language of a Topos]].

````tabs
tab: Julia
```julia
using Catlab
# Sub(G) for a graph G is a Heyting algebra (not Boolean): ¬¬A ≠ A can happen
G = path_graph(Graph, 3)                       # 1 → 2 → 3
sh(s) = (c = components(s); (collect(c[:V]), collect(c[:E])))
A = Subobject(G, V=[1, 2], E=Int[])            # vertices 1, 2 without the edge between them
sh(¬A)                                          # ([3], []): largest subgraph disjoint from A
sh(¬(¬A))                                       # ([1, 2], [1]) ≠ A: double negation adds the edge
sh(A ∨ ¬A)                                      # ([1, 2, 3], []) ≠ top: excluded middle fails
```
tab: Lean
```lean
import Mathlib
open TopologicalSpace
-- the opens of a space form a frame, hence a Heyting algebra with ⇨ and ᶜ
example (X : Type) [TopologicalSpace X] : Order.Frame (Opens X) := inferInstance
example (X : Type) [TopologicalSpace X] (U V : Opens X) : Opens X := U ⇨ V   -- implication
#check @himp_eq                              -- in a Boolean algebra a ⇨ b = bᶜ ⊔ b; not in general
#check @le_compl_compl                       -- a ≤ ¬¬a holds in every Heyting algebra
```
tab: Haskell
```haskell
-- connectives as characteristic maps of subobjects of Bool × Bool
andChar, orChar, impChar :: (Bool, Bool) -> Bool
andChar = (`elem` [(True, True)])
orChar  = (`elem` [(True, True), (True, False), (False, True)])
impChar = (`elem` [(True, True), (False, True), (False, False)])
notChar :: Bool -> Bool
notChar = (`elem` [False])
```
````
