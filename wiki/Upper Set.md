#definition #example

Given a [[Preorder]] $(P, \leq)$, an **upper set** in $P$ is a [[Subset]] $U \subseteq P$ such that if $p \in U$ and $p \leq q$ then $q \in U$: "if $p$ is an element then so is anything bigger". Write $\mathcal{U}(P)$ for the set of upper sets, ordered by inclusion $U \leq V$ iff $U \subseteq V$.

> Sources: 7 Sketches Example 1.54, 1.64, Exercises 1.55, 1.57, 1.65, 1.66, 1.79, Proposition 1.78.

**Example.** For the [[Booleans]], $\mathcal{U}(\mathbb{B})$ is $\varnothing \leq \{\mathsf{true}\} \leq \{\mathsf{true},\mathsf{false}\}$; $\{\mathsf{false}\}$ is not an upper set since $\mathsf{false} \leq \mathsf{true}$.

- On a [[Discrete Preorder]] every subset is an upper set, so $\mathcal{U}(X) = \mathcal{P}(X)$ ([[7S Chapter 1 Exercises#Exercise 1.55|7S Exercise 1.55]]).
- The inclusion $\mathcal{U}(P) \to \mathcal{P}(P)$ is a [[Monotone Map]] (Example 1.64).
- **[[Upper Sets Classified by Maps to Bool]]** (Proposition 1.78): upper sets of $P$ correspond to monotone maps $P \to \mathbb{B}$ via $U = f^{-1}(\mathsf{true})$.
- **Pullback**: a monotone $f : P \to Q$ induces $f^* : \mathcal{U}(Q) \to \mathcal{U}(P)$, $U \mapsto f^{-1}(U)$, which in terms of classifying maps is precomposition $u \mapsto f \mathbin{;} u$ ([[7S Chapter 1 Exercises#Exercise 1.79|7S Exercise 1.79]]).
- **Principal upper sets and Yoneda**: $\uparrow p := \{p' \mid p \leq p'\}$ is an upper set, $\uparrow : P^{\mathrm{op}} \to \mathcal{U}(P)$ is monotone, and $p \leq p'$ iff $\uparrow p' \subseteq \uparrow p$ — the [[Yoneda Lemma for Preorders]] ([[7S Chapter 1 Exercises#Exercise 1.66|7S Exercise 1.66]]): to know an element is to know its web of relationships.

Upper sets are the preorder version of [[Presheaf|presheaves]] / [[C-Set|co-presheaves]] valued in $\mathbb{B}$.

````tabs
tab: Julia
```julia
# upper sets of a finite preorder given by a leq predicate on elements xs
is_upper(leq, xs, U) = all((p ∈ U && leq(p, q)) <= (q ∈ U) for p in xs, q in xs)
upper_sets(leq, xs) = [Set(U) for U in Iterators.map(collect, powerset(xs)) if is_upper(leq, xs, Set(U))]
principal_up(leq, xs, p) = Set(q for q in xs if leq(p, q))   # ↑p
# (powerset from Combinatorics.jl)
```
tab: Lean
```lean
-- Mathlib: `UpperSet α` and `IsUpperSet`
#check @IsUpperSet          -- ∀ ⦃a b⦄, a ≤ b → a ∈ s → b ∈ s
#check (UpperSet ℕ)         -- bundled, a complete lattice
#check @UpperSet.Ici        -- the principal upper set ↑p = Set.Ici p
```
tab: Haskell
```haskell
-- an upper set as a predicate closed upwards (law unenforced)
type UpperSet a = a -> Bool

principalUp :: Preorder a => a -> UpperSet a
principalUp p q = leq p q         -- ↑p

pullback :: (a -> b) -> UpperSet b -> UpperSet a
pullback f u = u . f              -- f^* U = f⁻¹(U)
```
````
