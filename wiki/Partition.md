#definition #example

If $A$ is a [[Set]], a **partition** of $A$ consists of a set $P$ and, for each $p \in P$, a nonempty [[Subset]] $A_p \subseteq A$, such that

$$
A = \bigcup_{p \in P} A_p \qquad\text{and}\qquad p \neq q \implies A_p \cap A_q = \varnothing.
$$

We denote the partition by $\{A_p\}_{p \in P}$, call $P$ the set of **part labels** and $A_p$ the **$p$-th part**. The conditions say that each $a \in A$ lies in exactly one part.

Two partitions $\{A_p\}_{p\in P}$ and $\{A'_{p'}\}_{p' \in P'}$ are considered *the same* if for each $p \in P$ there is $p' \in P'$ with $A_p = A'_{p'}$ (only the labels changed; cf. [[7S Exercise 1.16]]).

> Sources: 7 Sketches Definition 1.14, Examples 1.26, 1.49, 1.52; Section 1.1 (systems as partitions).

## Partitions as surjections (Example 1.26)

A partition of $A$ is the same thing as a [[Surjection|surjective function]] $f : A \twoheadrightarrow P$: the preimages $f^{-1}(p)$ form the parts. For $S = \{11,12,13,21,22,23\}$ partitioned into $\{11,12\},\{13\},\{21\},\{22,23\}$, take $P = \{a,b,c,d\}$ and $f(11)=f(12)=a$, $f(13)=b$, $f(21)=c$, $f(22)=f(23)=d$.

## Partitions as equivalence relations

[[Partitions Correspond to Equivalence Relations]] (Proposition 1.19): the parts are the equivalence classes, and the set of parts is the [[Quotient Set]] $A/\!\sim$.

## The preorder of partitions

The set $\mathrm{Prt}(A)$ of all partitions of $A$ is ordered by *fineness*; see [[Preorder of Partitions]]. The motivating "systems" of 7 Sketches §1.1 — ways of connecting the points $\bullet, \circ, \ast$ — are exactly the five partitions of a three-element set, and joining systems is the [[Join]] in $\mathrm{Prt}$. See [[Generative Effect]].

Any function $g : S \to T$ induces a [[Galois Connection]] $g_! \dashv g^*$ between $\mathrm{Prt}(S)$ and $\mathrm{Prt}(T)$; see [[Pushforward and Pullback of Partitions]].

````tabs
tab: Julia
```julia
# A partition of {1,…,n} as a surjection {1,…,n} → {1,…,k}; parts are preimages
using Catlab
f = FinFunction([1,1,2,3,4,4], 4)      # partition {11,12},{13},{21},{22,23}
parts = [preimage(f, p) for p in 1:4]  # [[1,2],[3],[4],[5,6]]
is_epic(f)                              # true: every part is nonempty

# Kittenlab Lecture 9: equivalence classes via union-find (see [[Equivalence Relation]])
```
tab: Lean
```lean
-- Mathlib: `Setoid.IsPartition` and the equivalence with setoids
#check @Setoid.IsPartition          -- (c : Set (Set α)) : Prop
#check @Setoid.partition_iff_setoid -- partitions ↔ equivalence relations
-- the parts of a setoid are its classes
#check @Setoid.classes
```
tab: Haskell
```haskell
import Data.List (groupBy, sortOn)
import Data.Function (on)

-- a partition of xs induced by a "surjection" f (parts = fibres of f)
partitionBy :: Ord b => (a -> b) -> [a] -> [[a]]
partitionBy f = groupBy ((==) `on` f) . sortOn f

-- partitionBy (`div` 10) [11,12,13,21,22,23] == [[11,12,13],[21,22,23]]
```
````
