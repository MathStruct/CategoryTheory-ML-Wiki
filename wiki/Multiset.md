#definition #example

A **multiset** is a set in which elements may occur more than once. Category Theory for Scientists makes this precise with a [[Surjection]]: a multiset is a triple $X = (E, B, \pi)$ of a set $E$ of **element instances**, a set $B$ of **element names**, and a surjective [[Function]] $\pi : E \to B$; the **multiplicity** of a name $b$ is the cardinality of its [[Fiber]] $\pi^{-1}(b)$. A **mapping of multisets** $(E, B, \pi) \to (E', B', \pi')$ is a pair of functions $f : E \to E'$, $g : B \to B'$ making the square commute, $\pi' \circ f = g \circ \pi$.

> Sources: CTfS §2.7.6 (Definition 2.7.6.3, Exercises 2.7.6.2, 2.7.6.4–2.7.6.5, Definition 2.7.6.7, Exercises 2.7.6.8–2.7.6.9); [[Indexed Set]] for the fiberwise view.

## Examples

- $X = (1, 1, 2, 3)$ is $E = \{e_1, e_2, e_3, e_4\}$ over $B = \{1, 2, 3\}$ with $e_1, e_2 \mapsto 1$ — the name $1$ has multiplicity 2. $Y = (a, b, b, b)$ has $b$ with multiplicity 3.
- **Mappings** ([[CTfS Chapter 2 Exercises#Exercise 2.7.6.5|CTfS Exercise 2.7.6.5]]): a mapping $X \to Y$ must send the two instances of $1$ to instances of one and the same name. Sending $1 \mapsto b$, $2 \mapsto a$, $3 \mapsto b$ is fine — the instances land among the three $b$'s and the one $a$. Counting: for each choice of $g : \{1,2,3\} \to \{a, b\}$ the instances can be sent anywhere in the right fibers, giving $(1 + 3^2)(1 + 3)(1 + 3) = 160$ mappings; without the commuting square there would be $4^4 \cdot 2^3 = 2048$ pairs of functions.
- **Data**: a column of a database table, such as the surnames of all employees, is a multiset — rows are instances, distinct values are names, and repeated surnames have multiplicity $> 1$.
- **The bag of words** of a document, histograms, the prime factorization $12 = 2 \cdot 2 \cdot 3$.

## Relative sets and the slice category

Dropping the surjectivity requirement (allowing multiplicity 0 — CTfS's *pseudo-multisets*, [[CTfS Chapter 2 Exercises#Exercise 2.7.6.4|CTfS Exercise 2.7.6.4]]) and fixing the set of names $B$ gives CTfS's **relative sets over $B$** (Definition 2.7.6.7): a set $E$ with a function $E \to B$; morphisms over $B$ are functions $E \to E'$ commuting with the maps to $B$. These form the [[Slice Category]] $\mathbf{Set}_{/B}$ ([[CTfS Chapter 2 Exercises#Exercise 2.7.6.8|CTfS Exercise 2.7.6.8]] checks that composites are again maps over $B$). Relative sets over $\{*\}$ are just sets; over $\varnothing$ there is only the empty set, with its identity ([[CTfS Chapter 2 Exercises#Exercise 2.7.6.9|CTfS Exercise 2.7.6.9]]). Via fibers, a set over $B$ is the same as a $B$-[[Indexed Set|indexed family of sets]], and a multiset is one in which every fiber is nonempty. Multisets of *counts* form the free commutative monoid, $\mathbb N^{(B)}$ — the "commutative [[List]]", a [[Monad]] on $\mathbf{Set}$.

````tabs
tab: Julia
**Docs:** [FinSets](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.FinSets)
```julia
using Catlab
# a multiset as a surjection π : E → B (CTfS Definition 2.7.6.3): X = (1,1,2,3), Y = (a,b,b,b)
πX = FinFunction([1, 1, 2, 3], 3)        # instances e₁…e₄ ↦ names 1,2,3
πY = FinFunction([1, 2, 2, 2], 2)        # names a = 1, b = 2
multiplicity(π) = [count(==(b), collect(π)) for b in codom(π)]
multiplicity(πX), multiplicity(πY)                       # ([2, 1, 1], [1, 3])
# a mapping (f, g) : X → Y must satisfy πY ∘ f = g ∘ πX
g = FinFunction([2, 1, 2], 3, 2)         # 1 ↦ b, 2 ↦ a, 3 ↦ b
f = FinFunction([2, 3, 1, 4], 4, 4)      # the two 1s ↦ two b's, the 2 ↦ the a, the 3 ↦ a b
force(compose(f, πY)) == force(compose(πX, g))           # true: the square commutes
```
tab: Lean
```lean
import Mathlib
#check Multiset                 -- quotient of lists by permutation
#check @Multiset.count          -- multiplicity of an element
#eval Multiset.count 1 ({1, 1, 2, 3} : Multiset ℕ)   -- 2
-- the fibre view: a set over B
#check @CategoryTheory.Over     -- the slice category Over B of relative sets
```
tab: Haskell
```haskell
import qualified Data.Map as Map

-- a multiset as element-name ↦ multiplicity: the free commutative monoid
type Multiset a = Map.Map a Int

fromList :: Ord a => [a] -> Multiset a
fromList xs = Map.fromListWith (+) [(x, 1) | x <- xs]

-- fromList [1,1,2,3] == Map.fromList [(1,2),(2,1),(3,1)]
-- the union of bags adds multiplicities: Map.unionWith (+)
```
````
