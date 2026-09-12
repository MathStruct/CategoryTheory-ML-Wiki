#definition #example #program

A **dependent type** is a type parameterized by *values* (not just by other types, as `Maybe` or `[]` are). Categorically there are two equivalent pictures:

1. **Type family**: a family of types $T(x)$ indexed by elements $x$ of a base type $B$; the whole family is the [[Dependent Sum|sum]] $\sum_{x : B} T(x)$.
2. **Fibration**: a single object $E$ with a *display map* $p : E \to B$; the type $T(x)$ is the [[Fiber]] $p^{-1}(x)$, and $E$ is a *bundle* of fibers over the base $B$. In an arbitrary category a **fibration** is just an arrow $p : e \to b$ read with this intuition; all fibrations over $b$ form the [[Slice Category]] $\mathcal{C}/b$, whose morphisms are fiber-preserving maps ($p' \circ f = p$).

> Sources: DaoFP Chapter 11 ("Dependent Types": §11.1 "Dependent Vectors", §11.2 "Dependent Types Categorically", "Fibrations", "Type families as fibrations", "Substitution", "Dependent environments", "Base-change functor"), §11.3–11.5; 7 Sketches §7.3.3 (fibers, [[Sheaf of Sections|sections]]); Kittenlab Lecture 13 (typed sets).

## The standard example: counted vectors

```haskell
data Vec (n :: Nat) (a :: Type) where       -- needs DataKinds, GADTs, KindSignatures
  VNil  :: Vec Z a
  VCons :: a -> Vec n a -> Vec (S n) a
headV :: Vec (S n) a -> a                   -- only defined on non-empty vectors: the compiler enforces it
headV (VCons a _) = a
zipV :: Vec n a -> Vec n b -> Vec n (a, b)  -- both arguments must have the same length
```
The family is $T(0) = 1$, $T(1) = a$, $T(2) = a \times a$, ...; its total space is $\sum_{n : \mathbb{N}} a^n = 1 + a + a^2 + \cdots = \mathrm{List}(a)$ ([[List]], [[Free Monoid]]) and the display map is `length : List(a) → ℕ`. So counted vectors are the object $\langle \mathrm{List}(a), \mathrm{length} \rangle$ of $\mathcal{C}/\mathbb{N}$. The purpose is provable correctness: with an equality type one could state the monoid laws `assoc :: m <> (n <> p) = (m <> n) <> p` inside the type system ([[Equality Type]]). Languages: Idris, Agda, Lean; Haskell's support is "rather patchy" (singletons).

## Operations

- **Substitution / base change** along $f : A \to B$: the [[Pullback]] $f^* E \to A$ replants the fiber over $y$ onto every $x$ with $f(x) = y$; on families, $T(y) \mapsto T(f(x))$. This is the [[Base Change Functor]] $f^* : \mathcal{C}/B \to \mathcal{C}/A$.
- Its left adjoint is the [[Dependent Sum]] $\Sigma_f$ (existential quantification), its right adjoint the [[Dependent Product]] $\Pi_f$ (universal quantification): $\Sigma_f \dashv f^* \dashv \Pi_f$. A category where all slices are cartesian closed — a [[Locally Cartesian Closed Category]] — has all three, and is the model of dependent type theory, just as a [[Cartesian Closed Category]] models the simply typed lambda calculus.
- Environments: with dependent types the type being added to the context $\Gamma$ may depend on values already in $\Gamma$, so contexts are iterated dependent sums rather than plain products.
- Type families with no `Functor` instance are functors from a [[Discrete Category]] (footnote 1). Dependent elimination for $\mathbb{N}$ is the induction principle ([[Natural Numbers Object]]).

````tabs
tab: Julia
```julia
using Catlab
# a type family over a finite base as a fibration p : E → B; fibers are preimages
p = FinFunction([1, 1, 2, 3, 3, 3], 3)          # E = 6 elements over B = 3
fiber(x) = preimage(p, x)                        # T(1) has 2 elements, T(2) has 1, T(3) has 3
fiber.(1:3)
# Julia's own dependent-ish types: StaticArrays encode the length in the type, SVector{3,Int}
# base change = pullback along f : A → B
f = FinFunction([3, 3, 1], 3)
E′ = pullback(f, p)                              # f*E: 3 + 3 + 2 = 8 elements
ob(E′)
```
tab: Lean
```lean
import Mathlib
-- Lean is a dependently typed language: Fin n, Vector α n, Σ and Π types are primitive
#check @Vector                 -- Vector α n : lists of length n
#check @Vector.head            -- Vector α (n+1) → α
#check @Vector.zipWith
#check @Sigma                  -- Σ x : B, T x
#check @CategoryTheory.Over    -- the slice category C/b of "fibrations" over b
```
tab: Haskell
```haskell
{-# LANGUAGE DataKinds, GADTs, KindSignatures #-}
import Data.Kind (Type)
data Nat = Z | S Nat
data Vec (n :: Nat) (a :: Type) where
  VNil  :: Vec 'Z a
  VCons :: a -> Vec n a -> Vec ('S n) a
emptyV :: Vec 'Z Int
emptyV = VNil
singleV :: Vec ('S 'Z) Int
singleV = VCons 42 VNil
headV :: Vec ('S n) a -> a
headV (VCons a _) = a
```
````
