#definition #example

A [[Function]] $f : A \to B$ is **surjective** (a **surjection**, drawn $A \twoheadrightarrow B$) if for all $t \in B$ there exists $s \in A$ with $f(s) = t$.

> Sources: 7 Sketches Definition 1.22, Example 1.26; Kittenlab Lecture 2; DaoFP §2.5 (epimorphisms).

**Quantifier order matters** (Kittenlab): "for every $b$ there exists $a$" allows a different $a$ for each $b$; "there exists $a$ such that for every $b$" would force $B$ to be a singleton.

**Examples.** $\{1,2,3\} \to \{1,2\}$ with $1 \mapsto 1, 2 \mapsto 2, 3 \mapsto 2$ is surjective and not injective; $\{a, b\} \to \{1\}$ likewise.

- Surjections out of $A$ are the same as [[Partition|partitions]] of $A$ (Example 1.26): the parts are the preimages $f^{-1}(t)$.
- Surjective functions are exactly the [[Epimorphism|epimorphisms]] of $\mathbf{Set}$ (DaoFP §2.5). Any arrow into the [[Terminal Object]] is an epimorphism.
- A function is a [[Bijection]] iff surjective and [[Injection|injective]].
- Every function factors as a surjection followed by an injection ([[Epi-Mono Factorization]]), which is how [[Pushforward and Pullback of Partitions|partitions are pulled back]] along arbitrary functions.

````tabs
tab: Julia
```julia
# Kittenlab Lecture 2
function is_surjective(f::𝔽Mor)
  seen = Set([f(x) for x in f.dom])
  all(y ∈ seen for y in f.codom)
end

# Catlab
using Catlab
f = FinFunction([1, 2, 2], 2)
is_epic(f)      # true
```
tab: Lean
```lean
#check @Function.Surjective   -- ∀ b, ∃ a, f a = b
#check @CategoryTheory.epi_iff_surjective
```
tab: Haskell
```haskell
isSurjective :: Eq b => [a] -> [b] -> (a -> b) -> Bool
isSurjective dom cod f = all (`elem` map f dom) cod
```
````
