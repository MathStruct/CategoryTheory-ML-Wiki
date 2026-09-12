#definition #theorem #proof #program

The **cardinality** of a [[Finite Set]] is the number of unique elements listed in it.

> Source: Kittenlab Lecture 2.

**Theorem.** If two finite sets have the same cardinality, then they are [[Isomorphism|isomorphic]].

*Proof (induction on cardinality).* If $A$ and $B$ both have $0$ elements they are the same set and the identity is an isomorphism. Suppose all sets of cardinality $n$ are isomorphic and let $A = \{a\} \cup A'$, $B = \{b\} \cup B'$ have cardinality $n+1$. By hypothesis there is an isomorphism $f' : A' \to B'$; define $f(a) = b$ and $f(x) = f'(x)$ for $x \in A'$. This is surjective (hits $b$ and, by hypothesis, everything in $B'$) and injective (distinct elements of $A'$ go to distinct elements; $f(a) = b \notin B'$). By [[Bijection|the theorem on bijections]] $f$ is an isomorphism. $\blacksquare$

Conversely an isomorphism preserves cardinality, so *cardinality is a complete invariant of finite sets up to isomorphism*: the [[Skeleton|skeleton]] of $\mathbf{FinSet}$ is the category of ordinals $\underline{n}$. The [[Pigeonhole Principle]] is the contrapositive for injections. 7 Sketches uses cardinality $|\cdot| : \mathcal{P}(X) \to \mathbb{N}$ as an example of a [[Monotone Map]] (Example 1.62).

````tabs
tab: Julia
```julia
# Kittenlab Lecture 2: constructive proof
function find_isomorphism(A::𝔽, B::𝔽)
  A_vec, B_vec = unique!.([Any[A...], Any[B...]])
  @assert length(A_vec) == length(B_vec)
  n = length(A_vec)
  𝔽Mor(A, B, Dict(A_vec[i] => B_vec[i] for i in 1:n))
end
find_isomorphism(Vec𝔽([:c, :b, :a, :b]), Int𝔽(3)).vals

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
