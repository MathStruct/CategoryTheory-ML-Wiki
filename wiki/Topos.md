#definition #example #theorem

A **topos** (plural *toposes* or *topoi*) is a [[Category]] that behaves like $\mathbf{Set}$ well enough to do logic inside it. 7 Sketches uses the *Grothendieck* (sheaf) definition: a topos is a category of [[Sheaf|sheaves]] $\mathbf{Shv}(X)$ on a site, e.g. on a [[Topological Space]]. The more general *elementary topos* of Lawvere–Tierney is a category $\mathcal{E}$ that

1. has all finite [[Limit|limits]] (a [[Terminal Object]] and [[Pullback|pullbacks]] suffice),
2. is [[Cartesian Closed Category|cartesian closed]], and
3. has a [[Subobject Classifier]] $\mathsf{true} : 1 \to \Omega$.

Every Grothendieck topos is an elementary topos. Facts true in any topos $\mathcal{E}$ (7 Sketches §7.2.1): $\mathcal{E}$ has all (finite) [[Limit|limits]] and [[Colimit|colimits]], is cartesian closed, has [[Epi-Mono Factorization|epi-mono factorizations]], and has a subobject classifier.

> Sources: 7 Sketches §7.1–§7.2 (Set as an exemplar topos), §7.4 (Toposes), footnote 4 (elementary topos), footnotes 9–10, §7.6 ([MM92], [Joh02], [McL92]); DaoFP §11 (dependent types), §17 (presheaf categories).

## Examples

| topos | site | truth values $\Omega$ |
|---|---|---|
| [[Category of Sets|$\mathbf{Set}$]] | the one-point space $\{*\}$ (Example 7.48) | [[Booleans|$\mathbb{B}$]] |
| [[C-Set|$\mathcal{C}$-$\mathbf{Inst}$]], [[Presheaf|presheaves]] $[\mathcal{C}^{\mathrm{op}}, \mathbf{Set}]$ | a small category $\mathcal{C}$ with trivial covers | sieves on $c$ |
| [[Category of Graphs|$\mathbf{Grph}$]] | the arrow shape $\mathbf{ArShp}$ (Example 7.23) | the graph $\Omega_{\mathbf{Grph}}$, see [[Topos of Graphs]] |
| $\mathbf{Shv}(X)$ | a [[Topological Space]] $(X, \mathrm{Op})$ | $\Omega(U) = $ open subsets of $U$ |
| [[Topos of Behavior Types|$\mathbf{BT} = \mathbf{Shv}(\mathbb{I}\mathbb{R})$]] | the [[Interval Domain]] | open sets of time intervals |

## Why care

- Each topos has an [[Internal Language of a Topos|internal language]] — a higher-order logic with $\wedge, \vee, \neg, \Rightarrow, \forall, \exists$ whose semantics (Kripke–Joyal) is given by the [[Subobject Classifier]], the [[Heyting Algebra]] of [[Predicate|predicates]], and [[Quantification]]. "Any object $Y$ understands itself — its parts and the logic of how they fit together — by asking questions of the oracle $\Omega$."
- Truth values in a sheaf topos are *open sets*: a proposition is "true on $U$", not merely true or false. This lets one define graphs, groups or spaces *that change through time* ([[Topos of Behavior Types]]) and prove safety properties in a [[Temporal Logic|temporal logic]].
- Toposes were invented by Grothendieck's school ([AGV71]) for the Weil conjectures; Lawvere and Tierney recognized their logical content.

````tabs
tab: Julia
```julia
using Catlab
# Catlab's C-sets (ACSets) form presheaf toposes: finite limits, colimits,
# a Heyting algebra of subobjects, and a subobject classifier are all available.
G = path_graph(Graph, 3)
Ω, _ = subobject_classifier(Graph)       # the graph Ω_Grph: 2 vertices, 5 edges
nparts(Ω, :V), nparts(Ω, :E)            # (2, 5)
A = Subobject(G, V=[1, 2], E=[1])
B = Subobject(G, V=[2, 3], E=[2])
A ∧ B, A ∨ B, implies(A, B), ¬A          # Heyting operations on Sub(G)
```
tab: Lean
```lean
import Mathlib
open CategoryTheory
-- Mathlib has the ingredients of an elementary topos (no single `Topos` class yet):
#check @CategoryTheory.HasClassifier        -- subobject classifier
#check @CategoryTheory.CartesianClosed
#check @CategoryTheory.Limits.HasFiniteLimits
-- and Grothendieck toposes as sheaf categories:
#check @CategoryTheory.Sheaf                -- Sheaf J A for a Grothendieck topology J
#check @TopCat.Sheaf                        -- sheaves on a topological space
```
tab: Haskell
```haskell
-- Hask is not a topos, but Set-like reasoning shows up as: Bool is the subobject
-- classifier of finite types, and predicates are characteristic functions.
type Predicate a = a -> Bool
subobject :: [a] -> Predicate a -> [a]       -- {Y | p}
subobject ys p = filter p ys
```
````
