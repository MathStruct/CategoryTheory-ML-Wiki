#definition #example #theorem #proof

A **Galois connection** between [[Preorder|preorders]] $P$ and $Q$ is a pair of [[Monotone Map|monotone maps]] $f : P \to Q$ and $g : Q \to P$ such that

$$
f(p) \leq q \quad\text{if and only if}\quad p \leq g(q) \qquad (1.96)
$$

for all $p \in P$, $q \in Q$. We say $f$ is the **left adjoint** and $g$ the **right adjoint**, and write $f \dashv g$. Galois connections were first considered by Galois (field extensions vs. automorphism groups); they are the preorder case of [[Adjunction|adjunctions]], and a "relaxed version" of [[Isomorphism of Preorders|isomorphisms]].

> Sources: 7 Sketches §1.4, Definition 1.95, Examples 1.97, 1.113, 1.117, 1.122, Propositions 1.107, 1.111, Theorem 1.115, Exercises 1.98–1.101, 1.109, 1.110, 1.114, 1.119, 1.125; Remark 1.100; DaoFP §10.8 ("Freyd's theorem in a preorder").

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}
P \arrow[r, bend left, "f"] & Q \arrow[l, bend left, "g"]
\end{tikzcd}
\end{document}
```

## Examples

- **Example 1.97.** $\lceil -/3 \rceil : \mathbb{R} \to \mathbb{Z}$ is left adjoint to $(3 \times -) : \mathbb{Z} \to \mathbb{R}$, since $\lceil x/3 \rceil \leq y$ iff $x \leq 3y$. The right adjoint of $(3 \times -)$ is $\lfloor -/3 \rfloor$ ([[7S Chapter 1 Exercises#Exercise 1.98|7S Exercise 1.98]]); $\lceil -/3 \rceil$ has no left adjoint ([[7S Chapter 1 Exercises#Exercise 1.101|7S Exercise 1.101]]).
- Between [[Total Order|total orders]] drawn with bending arrows, $f \dashv g$ iff the arrows do not cross (Remark 1.100, [[7S Chapter 1 Exercises#Exercise 1.99|7S Exercise 1.99]]).
- [[Pushforward and Pullback of Partitions]]: any function $g : S \to T$ gives $g_! \dashv g^*$ between $\mathrm{Prt}(S)$ and $\mathrm{Prt}(T)$.
- [[Direct Image, Preimage, and Dual Image]]: $f_! \dashv f^* \dashv f_*$ between power sets (Example 1.117).
- [[Closure Operator|Closure operators]] $j$ give $j \dashv \iota$ between $P$ and $\mathrm{fix}_j$ (Example 1.122).
- [[Reflexive Transitive Closure]]: $\mathrm{Cl} \dashv U$ between relations and preorders on a set (§1.4.5).
- Example 1.113 shows right adjoints need not preserve joins ([[7S Chapter 1 Exercises#Exercise 1.114|7S Exercise 1.114]]).
- [[Quantale|Monoidal closed preorders]]: $(- \otimes a) \dashv (a \multimap -)$.

## Proposition 1.107 (unit/counit characterization)

For monotone $f : P \to Q$, $g : Q \to P$ the following are equivalent:
(a) $f \dashv g$;
(b) for all $p, q$: $p \leq g(f(p))$ and $f(g(q)) \leq q$. $\qquad (1.108)$

*Proof.* Suppose $f \dashv g$. For $p \in P$ put $q := f(p)$; reflexivity $f(p) \leq q$ gives $p \leq g(q) = g(f(p))$. Similarly $f(g(q)) \leq q$ ([[7S Chapter 1 Exercises#Exercise 1.109|7S Exercise 1.109]]). Conversely assume (1.108). If $f(p) \leq q$ then by monotonicity $g(f(p)) \leq g(q)$, and $p \leq g(f(p))$, so $p \leq g(q)$. The other direction is similar. $\blacksquare$

Replacing $\leq$ by $\cong$ in (1.108) recovers isomorphism. The inequalities are the preorder versions of the [[Unit and Counit of an Adjunction|unit and counit]].

## Basic theory

- **Uniqueness**: a right (or left) adjoint, if it exists, is unique up to equivalence: $g(q) \cong g'(q)$ for all $q$ ([[7S Chapter 1 Exercises#Exercise 1.110|7S Exercise 1.110]]; proof: $g(q) \leq g'(f(g(q))) \leq g'(q)$).
- **[[Right Adjoints Preserve Meets]]**, left adjoints preserve joins (Proposition 1.111). Hence left adjoints have no [[Generative Effect]].
- **[[Adjoint Functor Theorem for Preorders]]** (Theorem 1.115): if $Q$ has all meets, $g : Q \to P$ is a right adjoint iff it preserves meets; dually for joins. DaoFP §10.8 presents the same fact as Freyd's adjoint functor theorem in a preorder.
- The composite $f \mathbin{;} g : P \to P$ is a [[Closure Operator]], and $g \mathbin{;} f$ an [[Interior Operator]] ([[7S Chapter 1 Exercises#Exercise 1.119|7S Exercise 1.119]]).
- Galois connections relate different models of computation states in program analysis (abstract interpretation, [NNH99]).

````tabs
tab: Julia
```julia
# check the adjunction condition on finite preorders
is_galois(pP, pQ, f, g, ps, qs) =
  all(leq(pQ, f(p), q) == leq(pP, p, g(q)) for p in ps, q in qs)

# Example 1.97 restricted to a finite window: ⌈-/3⌉ ⊣ (3×-)
f(x) = ceil(Int, x / 3); g(y) = 3y
is_galois(UsualOrder(), UsualOrder(), f, g, -10:10, -4:4)   # true

# Catlab: Galois connections appear as adjunctions between thin categories;
# e.g. the closure/underlying adjunction between relations and preorders (see [[Reflexive Transitive Closure]])
```
tab: Lean
```lean
-- Mathlib: `GaloisConnection l u : ∀ a b, l a ≤ b ↔ a ≤ u b`
#check @GaloisConnection
example : GaloisConnection (fun n : ℕ => n + 1) (fun m : ℕ => m - 1) := by
  intro a b; constructor <;> omega   -- (in ℕ with truncated subtraction, b ≥ 1 needed; illustrative)
#check @GaloisConnection.le_u_l          -- p ≤ g (f p)   (unit)
#check @GaloisConnection.l_u_le          -- f (g q) ≤ q   (counit)
#check @GaloisConnection.u_iInf          -- right adjoints preserve meets
#check @GaloisConnection.l_iSup          -- left adjoints preserve joins
#check @GaloisConnection.compose
-- Example 1.97: Int.ceil ⊣ cast, cast ⊣ Int.floor
#check @Int.gc_ceil_coe                  -- GaloisConnection Int.ceil (↑)
#check @Int.gc_coe_floor                 -- GaloisConnection (↑) Int.floor
```
tab: Haskell
```haskell
-- a Galois connection between preorders (laws unenforced)
data Galois a b = Galois { leftAdj :: a -> b, rightAdj :: b -> a }
-- law: leq (leftAdj p) q == leq p (rightAdj q)

-- Example 1.97 on Double/Integer
ex197 :: Galois Double Integer
ex197 = Galois (\x -> ceiling (x / 3)) (\y -> 3 * fromInteger y)

checkGalois :: (Preorder a, Preorder b) => Galois a b -> [a] -> [b] -> Bool
checkGalois (Galois f g) ps qs = and [ leq (f p) q == leq p (g q) | p <- ps, q <- qs ]
```
````
