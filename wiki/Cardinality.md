#definition #theorem #proof #program

A set $A$ has **cardinality** $n \in \mathbb{N}$, written $|A| = n$, if there is an [[Isomorphism|isomorphism]] (a [[Bijection]]) $A \cong \underline{n} = \{1, \dots, n\}$; $A$ is **finite** if it has cardinality $n$ for some $n$, and infinite otherwise (CTfS Definition 2.1.2.15). Kittenlab, which represents a finite set by a list, phrases this as "the number of unique elements listed".

> Sources: Kittenlab Lecture 2; CTfS Definition 2.1.2.15, Lemma 2.1.2.17, §2.7.3 ("the natural numbers are literally the isomorphism classes of finite sets"), Exercises 2.1.2.5, 2.1.2.10, 2.1.2.16.

**Counting is building an isomorphism.** To count the cows in a field one points at a cow and says "1", at another and says "2", and so on: this builds a bijection between the herd and $\underline{n}$. So the natural numbers *are* the isomorphism classes of finite sets, and arithmetic mirrors set operations: $|A \sqcup B| = |A| + |B|$, $|A \times B| = |A| \cdot |B|$, $|B^A| = |B|^{|A|}$ — including $0^0 = 1$, since there is exactly one function $\varnothing \to \varnothing$ ([[Arithmetic of Sets]], [[CTfS Chapter 2 Exercises#Exercise 2.7.3.2|CTfS Exercise 2.7.3.2]]). E.g. $|\mathbf{Set}(\underline 5, \underline 2)| = 32$, $|\mathbf{Set}(\underline 2, \underline 5)| = 25$, and an $n$-element set has $n!$ automorphisms ([[CTfS Chapter 2 Exercises#Exercise 2.1.2.10|CTfS Exercise 2.1.2.10]]).

**Theorem.** If two finite sets have the same cardinality, then they are [[Isomorphism|isomorphic]].

*Proof (induction on cardinality).* If $A$ and $B$ both have $0$ elements they are the same set and the identity is an isomorphism. Suppose all sets of cardinality $n$ are isomorphic and let $A = \{a\} \cup A'$, $B = \{b\} \cup B'$ have cardinality $n+1$. By hypothesis there is an isomorphism $f' : A' \to B'$; define $f(a) = b$ and $f(x) = f'(x)$ for $x \in A'$. This is surjective (hits $b$ and, by hypothesis, everything in $B'$) and injective (distinct elements of $A'$ go to distinct elements; $f(a) = b \notin B'$). By [[Bijection|the theorem on bijections]] $f$ is an isomorphism. $\blacksquare$

Conversely an isomorphism preserves cardinality, so *cardinality is a complete invariant of finite sets up to isomorphism*: the [[Skeleton|skeleton]] of $\mathbf{FinSet}$ is the category of ordinals $\underline{n}$. The [[Pigeonhole Principle]] is the contrapositive for injections. 7 Sketches uses cardinality $|\cdot| : \mathcal{P}(X) \to \mathbb{N}$ as an example of a [[Monotone Map]] (Example 1.62).

````tabs
tab: Julia
**Docs:** [FinSets](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.FinSets) — Kittenlab [Lecture 2](https://algebraicjulia.github.io/Kittenlab.jl/lecture2.html)

**Builds on:** [[Finite Set]] (`Int𝔽`, `Vec𝔽`, `𝔽`), [[Function]] (`𝔽Mor`) — run those notes' Julia code first.
```julia
# Kittenlab Lecture 2: constructive proof
function find_isomorphism(A::𝔽, B::𝔽)
  A_vec, B_vec = unique!.([Any[A...], Any[B...]])
  @assert length(A_vec) == length(B_vec)
  n = length(A_vec)
  𝔽Mor(A, B, Dict(A_vec[i] => B_vec[i] for i in 1:n))
end
find_isomorphism(Vec𝔽([:c, :b, :a, :b]), Int𝔽(3)).vals
```
Catlab version (run in a fresh Julia session — Catlab exports its own `compose`, `id`, `FinFunction`, …):
```julia
# Catlab
using Catlab
length(FinSet(5))   # 5
```
tab: Lean
```lean
#check @Fintype.card
#check @Fintype.card_eq   -- Fintype.card α = Fintype.card β ↔ Nonempty (α ≃ β)
```
tab: Haskell
```haskell
import Data.List (nub)
cardinality :: Eq a => [a] -> Int
cardinality = length . nub

-- an isomorphism between equal-cardinality lists
findIso :: (Eq a, Eq b) => [a] -> [b] -> [(a, b)]
findIso xs ys = zip (nub xs) (nub ys)
```
````
