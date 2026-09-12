#definition #example #theorem #proof

A **monotone map** between [[Preorder|preorders]] $(A, \leq_A)$ and $(B, \leq_B)$ is a [[Function]] $f : A \to B$ such that for all $x, y \in A$, if $x \leq_A y$ then $f(x) \leq_B f(y)$. Kittenlab calls these **order-preserving maps**. Monotone maps are the structure-preserving maps for preorders: 7 Sketches thinks of them as *observations* of one system by another.

> Sources: 7 Sketches Definition 1.59, Examples 1.60–1.64, 1.68, Propositions 1.70, 1.78, Exercises 1.67, 1.71, 1.77; Kittenlab Lecture 5, 7; DaoFP §20.1.

## Examples

- $\mathbb{B} \to \mathbb{N}$, $\mathsf{false} \mapsto 17$, $\mathsf{true} \mapsto 24$ (Example 1.60).
- The [[Tree of Life]] map from classifications to taxonomic ranks (Example 1.61).
- [[Cardinality]] $|\cdot| : \mathcal{P}(X) \to \mathbb{N}$ (Example 1.62).
- The inclusion $\mathcal{U}(P) \to \mathcal{P}(P)$ of [[Upper Set|upper sets]] into the [[Power Set]] (Example 1.64).
- Pullback of [[Partition|partitions]] $f^* : \mathrm{Prt}(Y) \to \mathrm{Prt}(X)$ along a surjection $f : X \twoheadrightarrow Y$, $s \mapsto f \mathbin{;} s$ (Example 1.68); see [[Pushforward and Pullback of Partitions]].
- Every function out of a [[Discrete Preorder]] is monotone ([[7S Exercise 1.67]]).
- Monotone maps $P \to \mathbb{B}$ are the same as [[Upper Set|upper sets]] ([[Upper Sets Classified by Maps to Bool]]).
- The connectivity observation $\Phi : \mathrm{Prt}(\{\bullet,\circ,\ast\}) \to \mathbb{B}$ ([[7S Exercise 1.77]]) — monotone but with a [[Generative Effect]].

## Monotone maps are functors (Kittenlab Lecture 5)

**Proposition.** A [[Functor]] between preorders $X$ and $Y$ viewed as categories is exactly a function $F : X \to Y$ with $x \leq x'$ implying $F(x) \leq F(x')$.

*Proof.* A morphism $x \to y$ must be sent to a morphism $F(x) \to F(y)$, and since there is at most one morphism between any two objects, $F$ automatically preserves composites and identities. $\blacksquare$

**Proposition 1.70.** The [[Identity Function]] is monotone, and the composite $f \mathbin{;} g$ of monotone maps is monotone ([[7S Exercise 1.71]]). Hence preorders and monotone maps form a category $\mathbf{Preord}$ (Kittenlab: $\mathsf{Preorder}$), a full [[Subcategory]] of $\mathbf{Cat}$; see [[Category of Preorders]].

Some functors involving $\mathbf{Preord}$ (Kittenlab):
1. $\mathbf{Preord} \to \mathbf{Cat}$, view a preorder as a category;
2. $\mathbf{Cat} \to \mathbf{Preord}$, the [[Preorder Reflection]]: $x \leq y$ iff $\mathrm{Hom}(x,y) \neq \varnothing$;
3. $\mathbf{Preord} \to \mathbf{Set}$, the underlying set;
4. $\mathbf{Set} \to \mathbf{Preord}$, the [[Discrete Preorder]];
5. $\mathbf{Set} \to \mathbf{Preord}$, the [[Codiscrete Preorder]].

Between monotone maps $f, g : P \to Q$ there is at most one [[Natural Transformation]], existing iff $f(x) \leq g(x)$ for all $x$ (Lecture 7).

## Preservation

A monotone map may or may not preserve [[Meet|meets]] and [[Join|joins]] ([[Preservation of Meets and Joins]]); failing to preserve joins is a [[Generative Effect]]. Monotone maps that preserve all meets are exactly right adjoints of [[Galois Connection|Galois connections]] ([[Adjoint Functor Theorem for Preorders]]). Between [[Symmetric Monoidal Preorder|monoidal preorders]] the right notion is a [[Monoidal Monotone Map]]; between [[Lawvere Metric Space|metric spaces]], monotone maps become [[Enriched Functor|$\mathbf{Cost}$-functors]], i.e. distance-non-increasing maps.

````tabs
tab: Julia
```julia
# check monotonicity on finite preorders (Kittenlab-style `leq`)
is_monotone(pA, pB, f, xs) = all(!leq(pA, x, y) || leq(pB, f(x), f(y)) for x in xs, y in xs)

# Example 1.60: Bool → ℕ
f(b) = b ? 24 : 17
is_monotone(BoolPre(), UsualOrder(), f, [false, true])   # true
```
tab: Lean
```lean
#check @Monotone     -- ∀ ⦃a b⦄, a ≤ b → f a ≤ f b
example : Monotone (fun n : ℕ => n + 5) := fun _ _ h => Nat.add_le_add_right h 5
-- monotone maps compose; bundled as `OrderHom` (notation α →o β)
#check @Monotone.comp
#check (OrderHom ℕ ℕ)
-- monotone maps are functors between the thin categories
example {α β : Type} [Preorder α] [Preorder β] (f : α →o β) :
    CategoryTheory.Functor α β := f.monotone.functor
```
tab: Haskell
```haskell
-- a monotone map is a function with a promise (unenforced): leq x y ==> leq (f x) (f y)
newtype Monotone a b = Monotone (a -> b)

isMonotoneOn :: (Preorder a, Preorder b) => [a] -> (a -> b) -> Bool
isMonotoneOn xs f = and [ not (leq x y) || leq (f x) (f y) | x <- xs, y <- xs ]

-- Example 1.60
ex160 :: Bool -> Int
ex160 False = 17
ex160 True  = 24
```
````
