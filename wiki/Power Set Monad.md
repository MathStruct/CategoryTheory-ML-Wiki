#definition #example #program

The **power set monad** $(\mathcal P, \eta, \mu)$ on $\mathbf{Set}$ sends a set $X$ to its [[Power Set]] $\mathcal P(X)$ and a function $f$ to direct image, $\mathcal P(f)(S) = f(S)$. The unit and multiplication are

$$
\eta_X(x) = \{x\}, \qquad \mu_X(\mathcal S) = \bigcup_{S \in \mathcal S} S \quad (\mathcal S \in \mathcal P(\mathcal P(X))).
$$

It is the monad of **nondeterminism**: a Kleisli arrow $A \to \mathcal P(B)$ returns a *set* of possible results, and composing two means collecting all results reachable through some intermediate. The [[List Monad]] is the same idea with order and multiplicity kept; $\mathcal P$ is the commutative, idempotent quotient.

> Sources: CTfS Example 5.3.2.3, Exercises 5.3.3.5–5.3.3.7; [[Kleisli Category]]; 7 Sketches §1.4 (Galois connection $f_! \dashv f^*$ on power sets).

## The monad laws on a small example

For $X = \{a, b\}$ (CTfS Example 5.3.2.3): $\eta$ is $a \mapsto \{a\}$, $b \mapsto \{b\}$, and $\mu : \mathcal P(\mathcal P(X)) \to \mathcal P(X)$ has $2^4 = 16$ rows, e.g. $\{\{a\}, \{a, b\}\} \mapsto \{a, b\}$, $\{\varnothing\} \mapsto \varnothing$, $\{\{b\}, \varnothing\} \mapsto \{b\}$. Unitality says $\bigcup \{S\} = S = \bigcup_{x \in S} \{x\}$; associativity says a union of unions can be computed in either order.

## Kleisli arrows are relations

**Theorem** ([[CTfS Chapter 5 Exercises#Exercise 5.3.3.5|CTfS Exercise 5.3.3.5]]). $\mathrm{Kl}(\mathcal P) \cong \mathbf{Rel}$, the [[Category of Relations]].

*Proof.* A function $f : A \to \mathcal P(B)$ is the same as the relation $R_f = \{(a, b) \mid b \in f(a)\} \subseteq A \times B$, and conversely. Kleisli composition is $(g \circ_{\mathcal P} f)(a) = \mu(\mathcal P g (f a)) = \bigcup_{b \in f(a)} g(b)$, i.e. $c$ is reachable from $a$ iff there is a $b$ with $a R_f b$ and $b R_g c$ — relational composition. The Kleisli identity $\eta$ is the diagonal relation. $\blacksquare$

Products and coproducts in $\mathrm{Kl}(\mathcal P) = \mathbf{Rel}$ are *both* disjoint unions: for $\{1, 2, 3\}$ and $\{a, b\}$ both are $\{1, 2, 3, a, b\}$ ([[CTfS Chapter 5 Exercises#Exercise 5.3.3.6|CTfS Exercises 5.3.3.6]]–[[CTfS Chapter 5 Exercises#Exercise 5.3.3.7|5.3.3.7]]) — a relation *into* $X \sqcup Y$ is a pair of relations into $X$ and into $Y$. Such simultaneous products and coproducts are *biproducts*, which makes $\mathbf{Rel}$ look like linear algebra over the Booleans: a relation $\underline m \to \underline n$ is an $m \times n$ Boolean matrix and Kleisli composition is matrix multiplication in the [[Quantale]] $\mathbf{Bool}$.

## Further structure

- **Adjunction.** $\mathcal P$ comes from the free–forgetful adjunction between sets and complete join-semilattices (sup-lattices): $\mathcal P(X)$ is the free sup-lattice on $X$, and algebras for $\mathcal P$ are exactly sup-lattices ([[Join]], [[Monads from Adjunctions]]).
- **Nondeterministic machines.** A nondeterministic automaton is a Kleisli action $\Sigma \times S \to \mathcal P(S)$ of an alphabet ([[Finite State Machine]]); determinization is the subset construction — running the machine on $\mathcal P(S)$.
- **Kleisli instances.** A $\mathcal P$-valued instance on the graph schema lets each arrow have any set of sources and targets ([[Kleisli Instance]]).
- The finite power set $\mathcal P_{\mathrm{fin}}$ is also a monad; its algebras are join-semilattices with $\bot$.

````tabs
tab: Julia
**Docs:** plain Julia — Catlab has no dedicated API for this; related: [Catlab v0.16 docs](https://algebraicjulia.github.io/Catlab.jl/v0.16/) · [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/)
```julia
# the power set monad on finite sets: unit, multiplication, Kleisli composition
η(x) = Set([x])
μ(SS) = isempty(SS) ? Set() : union(SS...)
kleisli(f, g) = a -> μ(Set(g(b) for b in f(a)))            # g ∘_P f
# μ on P(P({a, b})): 16 inputs (CTfS Example 5.3.2.3)
P(xs) = [Set(x for (i, x) in enumerate(xs) if isodd(m >> (i - 1))) for m in 0:2^length(xs) - 1]
length(P(P([:a, :b]))), μ(Set([Set([:a]), Set([:a, :b])]))  # (16, Set([:a, :b]))
# Kleisli arrows are relations: parent/grandparent
parent = Dict(:ann => [:bob, :cat], :bob => [:dan], :cat => [:eve, :fay])
children(x) = Set(get(parent, x, Symbol[]))
kleisli(children, children)(:ann)                           # Set([:dan, :eve, :fay])
kleisli(η, children)(:ann) == children(:ann) == kleisli(children, η)(:ann)   # unit laws
```
tab: Lean
```lean
import Mathlib
-- Set is a monad in Mathlib (pure = singleton, bind = indexed union)
#check (inferInstance : Monad Set)
example (x : ℕ) : (pure x : Set ℕ) = {x} := rfl
#check @Set.bind                  -- s.bind f = ⋃ a ∈ s, f a
#check @Rel.comp                  -- composition of relations = Kleisli composition
```
tab: Haskell
```haskell
import qualified Data.Set as Set

-- Data.Set is not a Monad instance (it needs Ord), so write the structure directly
unitP :: a -> Set.Set a
unitP = Set.singleton

joinP :: Ord a => Set.Set (Set.Set a) -> Set.Set a
joinP = Set.unions . Set.toList

kleisliP :: (Ord b, Ord c) => (a -> Set.Set b) -> (b -> Set.Set c) -> a -> Set.Set c
kleisliP f g = joinP . Set.map g . f          -- relational composition

-- nondeterministic machine: step :: Char -> s -> Set s; a word acts by folding kleisliP
```
````
