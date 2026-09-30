#definition #example

Let $X$ be a [[Set]] and $\mathcal{P}(X)$ its [[Power Set]]. A **topology** on $X$ is a subset $\mathrm{Op} \subseteq \mathcal{P}(X)$, whose elements are called **open sets**, such that

(a) *whole set*: $X \in \mathrm{Op}$;
(b) *binary intersections*: $U, V \in \mathrm{Op} \Rightarrow U \cap V \in \mathrm{Op}$;
(c) *arbitrary unions*: for any family $(U_i)_{i \in I}$ of opens, $\bigcup_{i \in I} U_i \in \mathrm{Op}$; with $I = \varnothing$ this says $\varnothing \in \mathrm{Op}$.

If $U = \bigcup_{i \in I} U_i$ we say $(U_i)_{i \in I}$ **covers** $U$ (an *open cover*). A **topological space** is a pair $(X, \mathrm{Op})$. A **continuous function** $f : (X, \mathrm{Op}_X) \to (Y, \mathrm{Op}_Y)$ is a function with $f^{-1}(U) \in \mathrm{Op}_X$ for every $U \in \mathrm{Op}_Y$. Spaces and continuous maps form the category $\mathbf{Top}$.

> Sources: 7 Sketches §7.3.2, Definition 7.25, Examples 7.26, 7.28, 7.30, Exercises 7.27, 7.29, 7.31, 7.32, 7.34, Remark 7.33; CTfS §3.4.4.8 (basic open sets, Example 3.4.4.9, Exercises 3.4.4.10–3.4.4.12), Example 4.2.3.1, Exercise 4.2.3.2, Example 4.2.3.3

## Examples

- **Metric spaces** (Example 7.26): in $\mathbb{R}^2$ (or any [[Metric Space]]) the $\epsilon$-ball is $B(p; \epsilon) = \{p' \mid d(p, p') < \epsilon\}$; $U$ is open iff every $p \in U$ has some $B(p; \epsilon) \subseteq U$. On $\mathbb{R}$: $B(x, \epsilon) = (x - \epsilon, x + \epsilon)$; $(0,2)$ and $(1,3)$ cover $(0,3)$; $\bigcup_{i \geq 1} (\tfrac{1}{i}, 1) = (0, 1)$ is an infinite cover ([[7S Chapter 7 Exercises#Exercise 7.27|7S Exercise 7.27]]).
- **Coarse and discrete** (Example 7.28): $\mathrm{Op}_{\mathrm{crse}} = \{\varnothing, X\}$ has the fewest opens; $\mathrm{Op}_{\mathrm{fine}} = \mathcal{P}(X)$ the most — the **discrete space**, from which every function is continuous ([[7S Chapter 7 Exercises#Exercise 7.29|7S Exercise 7.29]]).
- **Sierpiński space** (Example 7.30): $X = \{1, 2\}$ with $\mathrm{Op}_1 = \{\varnothing, \{1\}, X\}$ (or the isomorphic $\mathrm{Op}_2$); the two remaining topologies on $\{1, 2\}$ are coarse and discrete. See [[Sierpinski Space]].
- **Subspace topology** ([[7S Chapter 7 Exercises#Exercise 7.32|7S Exercise 7.32]]): for $Y \subseteq X$, $A \subseteq Y$ is open iff $A = B \cap Y$ for some $B \in \mathrm{Op}$; the inclusion $Y \hookrightarrow X$ is then continuous.
- The [[Interval Domain]] $\mathbb{I}\mathbb{R}$, the site of the [[Topos of Behavior Types]].

## Spaces in Category Theory for Scientists

CTfS introduces open sets through *basic* open sets — interiors of closed curves such as circles, ellipses or kidney beans — and unions of them: the half-plane $\{(x,y) \mid x > 0\}$ is the union of the open unit squares with upper-left corner $(a, b)$, $a > 0$ (CTfS Example 3.4.4.9). "On the earth one could define a basic open set to be the interior of any region one can draw a circle around." A topology is a sub-order $\mathrm{Op}(X) \subseteq \mathcal{P}(X)$ closed under finite meets and arbitrary joins; continuity says preimage restricts to $\mathrm{Op}(Y) \to \mathrm{Op}(X)$, so "looking at points" is a functor $\mathbf{Top} \to \mathbf{Set}$ while "looking at open sets" is a functor $\mathbf{Top}^{\mathrm{op}} \to \mathbf{Preord}$ (CTfS Exercise 4.2.3.2). Assigning to each region the interval of recorded temperatures is a monotone map preserving joins but not meets ([[Join]]); a functor from the topological monoid $(\mathbb R, 0, +)$ to $\mathbf{Top}$ is a continuous dynamical system (CTfS Example 4.2.3.3); paths up to homotopy form the fundamental [[Groupoid]].

## The preorder of open sets

$(\mathrm{Op}, \subseteq)$ is a [[Preorder]] (indeed a [[Partial Order]]), hence a [[Category]]: one morphism $U \to V$ iff $U \subseteq V$. A [[Presheaf]] on $\mathrm{Op}$ assigns sets of *sections* to opens with *restriction* maps; a [[Sheaf]] is a presheaf respecting covers. Moreover $(\mathrm{Op}, \subseteq, X, \cap)$ is a [[Quantale]] (Remark 7.33) and a [[Heyting Algebra]]: a $\mathrm{Op}$-[[Enriched Category|enriched category]] has "size restrictions" $\mathcal{C}(a, b) \in \mathrm{Op}$ like bridges a truck must fit under ([[7S Chapter 7 Exercises#Exercise 7.34|7S Exercise 7.34]]).

````tabs
tab: Julia
**Docs:** [ThThinCategory (GATlab)](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/#GATlab.Stdlib.StdTheories.ThThinCategory) · [Theories & presentations](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/theories/)
```julia
# a finite topology as a set of BitSets, and the axioms checked by brute force
X = 1:2
Op1 = Set([BitSet(), BitSet([1]), BitSet([1, 2])])        # Sierpiński
istopology(X, Op) = BitSet(X) in Op && BitSet() in Op &&
  all(union(U, V) in Op && intersect(U, V) in Op for U in Op, V in Op)
istopology(X, Op1)                                        # true
istopology(X, Set([BitSet(), BitSet([1]), BitSet([2])]))  # false: missing the whole set
```
Catlab version (run in a fresh Julia session — Catlab exports its own `compose`, `id`, `FinFunction`, …):
```julia
# in Catlab the poset Op is a thin category; e.g. Sierpiński as a FreePreorder presentation
using Catlab
@present Op1Cat(FreePreorder) begin (∅, U, X)::El; a::Leq(∅, U); b::Leq(U, X) end
```
tab: Lean
```lean
import Mathlib
#check @TopologicalSpace              -- class: IsOpen, isOpen_univ, isOpen_inter, isOpen_sUnion
#check @Continuous                    -- ∀ U, IsOpen U → IsOpen (f ⁻¹' U)
#check @TopologicalSpace.Opens        -- the poset (frame) of open sets
#check @sierpinskiSpace               -- TopologicalSpace Prop
#check @instTopologicalSpaceSubtype   -- subspace topology
#check @Metric.isOpen_iff             -- ε-ball characterization
```
tab: Haskell
```haskell
import qualified Data.Set as S
type Topology a = S.Set (S.Set a)
isTopology :: Ord a => S.Set a -> Topology a -> Bool
isTopology x op = S.member x op && S.member S.empty op
  && and [ S.member (S.union u v) op && S.member (S.intersection u v) op | u <- S.toList op, v <- S.toList op ]
```
````
