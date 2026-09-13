#definition #example #theorem #proof

Let $(P, \leq)$ be a [[Preorder]] and $A \subseteq P$ a subset. An element $p \in P$ is a **meet** (greatest lower bound) of $A$ if

(a) for all $a \in A$, $p \leq a$ (it is a *lower bound*), and
(b) for all $q$ with $q \leq a$ for all $a \in A$, we have $q \leq p$ (it is the *greatest* one).

We write $p = \bigwedge A$ or $\bigwedge_{a \in A} a$; if $A = \{a, b\}$ we write $a \wedge b$.

> Sources: 7 Sketches Definition 1.81, Remark 1.82, Examples 1.83–1.89, Proposition 1.91, Exercises 1.80, 1.85, 1.90; Kittenlab Lecture 13 (products in a poset); DaoFP §9.5, 10.4.

## "The" meet (Remark 1.82)

Two meets $p, q$ of the same $A$ satisfy $p \leq q$ and $q \leq p$, so $p \cong q$ ([[Equivalent Elements of a Preorder]]); in a [[Partial Order]] they are equal ([[7S Chapter 1 Exercises#Exercise 1.85|7S Exercise 1.85]]). Since category theory cares only about how things relate to other things, the abuse of writing "the" meet is harmless — "any two things defined by the same universal property are unique up to unique isomorphism".

## Examples

- Meets need not exist: in the [[Discrete Preorder]] $\{p, q, r\}$ the set $\{p, q\}$ has no join (Example 1.83) and no meet.
- Several meets may exist: in $a, b \geq c \cong d$, both $c$ and $d$ are meets of $\{a, b\}$ (Example 1.84).
- $\bigwedge \{p\} \cong p$ and in a partial order $p \wedge p = p$ (Example 1.86).
- [[Power Set]]: $A \wedge B = A \cap B$ (Example 1.87); Kittenlab Lecture 13: the [[Product]] of two subsets is their intersection.
- [[Booleans]]: meet is AND (Example 1.88). [[Total Order]]: meet is infimum (Example 1.89). [[Divisibility Order]]: meet is $\gcd$ ([[7S Chapter 1 Exercises#Exercise 1.90|7S Exercise 1.90]]).
- In $\mathbb{R}$, $\bigwedge \mathbb{N} = 0$ and $\bigwedge \{\frac{1}{n+1}\} = 0$ ([[7S Chapter 1 Exercises#Exercise 1.80|7S Exercise 1.80]]).

## Proposition 1.91 (meets of nested subsets)

If $A \subseteq B \subseteq P$ both have meets then $\bigwedge B \leq \bigwedge A$. *Proof.* Let $m = \bigwedge A$, $n = \bigwedge B$. For $a \in A$ we have $a \in B$, so $n \leq a$; thus $n$ is a lower bound for $A$ and $n \leq m$. $\blacksquare$ (Dually $\bigvee A \leq \bigvee B$.)

## Categorical meaning

A meet is a [[Limit]] in the thin category $P$: the meet of $\{a,b\}$ is the [[Product]] $a \times b$, the meet of $\varnothing$ is the top element, a [[Terminal Object]]. [[Galois Connection|Right adjoints]] preserve meets ([[Right Adjoints Preserve Meets]]) and, when $P$ has all meets, a monotone map is a right adjoint iff it preserves them ([[Adjoint Functor Theorem for Preorders]]). Dual: [[Join]].

````tabs
tab: Julia
```julia
# meet of a subset A of a finite preorder xs (may return nothing, or one of several equivalent meets)
function meet(leq, xs, A)
  lbs = [q for q in xs if all(leq(q, a) for a in A)]
  for p in lbs
    all(leq(q, p) for q in lbs) && return p
  end
  nothing
end
meet((a,b) -> b % a == 0, 1:12, [4, 6])   # 2  (gcd)

# Catlab: meets in the subobject lattice of a FinSet
using Catlab
X = FinSet(3); U = Subobject(X, [1,2]); V = Subobject(X, [2,3])
meet(U, V)   # {2}
```
tab: Lean
```lean
#check @IsGLB            -- IsGLB s p : p is a greatest lower bound of s
#check @sInf             -- ⨅ in a complete lattice
example (s : Set ℝ) (h : BddBelow s) (hs : s.Nonempty) : IsGLB s (sInf s) := isGLB_csInf hs h
example (A B : Set ℕ) : A ⊓ B = A ∩ B := rfl
```
tab: Haskell
```haskell
-- meet on a finite preorder (returns one greatest lower bound, if any)
meet :: Preorder a => [a] -> [a] -> Maybe a
meet xs as =
  let lbs = [q | q <- xs, all (leq q) as]
  in case [p | p <- lbs, all (`leq` p) lbs] of
       (p:_) -> Just p
       []    -> Nothing
```
````
