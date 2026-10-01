#definition #theorem #example #program

A **$K$-relation**, for a commutative semiring $(K, \oplus, \otimes, 0, 1)$ (a commutative [[Rig]]), is a relation in which every tuple carries an annotation in $K$ — formally a function from tuples to $K$ with finite support. Positive relational algebra extends to $K$-relations with one rule: **joint use multiplies, alternative use adds**.

- a **join** annotates an output tuple with the product $a \otimes b$ of the two input annotations;
- a **union** or a **projection** that merges tuples annotates the result with the sum $a \oplus b$;
- a selection keeps annotations or sends them to $0$.

Choosing $K$ chooses the semantics: $\mathbb B = (\{0,1\}, \vee, \wedge)$ gives ordinary set semantics, $\mathbb N$ gives SQL's bag semantics, $(\mathbb R_{\ge 0} \cup \{\infty\}, \min, +)$ gives cheapest derivations, $([0,1], \max, \cdot)$ most likely ones. The **provenance semiring** is the free one, the polynomials $\mathbb N[X]$ over a set $X$ of tuple identifiers: annotating each input tuple with its own variable records, for every output tuple, *every way it was derived*.

> Sources: Green, Karvounarakis & Tannen, *Provenance semirings*, PODS 2007 (the definitions, the universality of $\mathbb N[X]$, and Datalog provenance as formal power series); Abo Khamis, Ngo, Pichler, Suciu & Wang, *Convergence of Datalog over (Pre-) Semirings*, [arXiv:2105.14435](https://arxiv.org/abs/2105.14435) ([[Convergence of Datalog over (Pre-) Semirings|notes]]) Definition 2.3 (POPS), Theorems 1.2, 5.10, 5.12 (stability and convergence); 7 Sketches §2.3.3 and [[Quantale]] for semirings as the enrichment bases of [[Matrix Multiplication in a Quantale|matrix multiplication]]; [[Rig]] (semiring basics and message passing).

## The universality theorem

$\mathbb N[X]$ is the **free commutative semiring** on $X$: every valuation $\nu : X \to K$ into a commutative semiring extends uniquely to a semiring homomorphism $\mathrm{Eval}_\nu : \mathbb N[X] \to K$ (the adjunction $\mathbf{CSRing} \rightleftarrows \mathbf{Set}$ of [[Free-Forgetful Adjunction|free and forgetful functors]]). Query evaluation is built from $\oplus$ and $\otimes$ only, so it **commutes with every semiring homomorphism**:

$$
\mathrm{Eval}_\nu\big(Q(I_{\mathbb N[X]})\big) \;=\; Q\big(\mathrm{Eval}_\nu(I_{\mathbb N[X]})\big).
$$

Compute a query *once* over provenance polynomials and every other semantics — set, bag, cost, probability, access control, trust — follows by substitution. This is the database instance of [[Functorial Semantics]]: the query is a term in the theory of commutative semirings, and each $K$ is a model.

For a derivation like $Q(1, 4) = r_1 s_1 + r_2 s_2 + t_1$, the polynomial reads: "(1, 4) has three derivations — from $r_1$ joined with $s_1$, from $r_2$ joined with $s_2$, and directly from $t_1$". Deleting tuple $r_1$ is the homomorphism $r_1 \mapsto 0$; the answer survives iff the polynomial stays non-zero.

## Recursion: provenance of Datalog

For recursive queries the number of derivations can be infinite (a cycle can be traversed any number of times), so provenance lives in **formal power series** $\mathbb N^\infty[\![X]\!]$, and the meaning of a Datalog program is a least fixed point in a semiring that must be ordered and complete enough ([[Least Fixed Point]]). Whether the fixed-point iteration actually converges depends on the semiring: it converges on every program iff $K$ (with a bottom adjoined) is **stable** (Abo Khamis et al., Theorem 1.2), and every polynomial map over a stable semiring is stable (Theorem 5.10). The Boolean and tropical semirings are stable; $\mathbb N$ is not — the derivation count of a cyclic graph diverges.

## Why it is categorical

| construction | categorical reading |
|---|---|
| $K$-relation | a functor-like assignment $\text{tuples} \to K$; a matrix over $K$ for binary relations |
| join + projection | matrix multiplication in the [[Quantale]]/rig $K$ — composition in $K$-$\mathbf{Rel}$ |
| change of semiring $h : K \to K'$ | [[Change of Base]] along a rig homomorphism |
| $\mathbb N[X]$ | the free commutative rig, initial among rigs with an $X$-valuation |

Binary $K$-relations compose by matrix multiplication over $K$, so they form a category enriched in $K$-modules; the boolean case is the [[Category of Relations]], and the tropical case is the [[Lawvere Metric Space|Lawvere-metric]] world of 7 Sketches Ch. 2.

## Sophia

Sophia attaches provenance to everything it derives — which frontend, which rule set, which witnesses — and a build artifact is "tainted" by any `tested` or `asserted` equivalence used along the way. That is provenance in a semiring of trust levels: the trust of a derivation is the *minimum* over the edges used (⊗) and the best derivation wins (⊕). See [Trusted Computing Base](https://mathstruct.org/Sophia/vault/Design/Trusted-Computing-Base) and [Graph Schema](https://mathstruct.org/Sophia/vault/Design/Graph-Schema).

````tabs
tab: Julia
**Docs:** plain Julia — Catlab has no dedicated API for this; related: [Catlab v0.16 docs](https://algebraicjulia.github.io/Catlab.jl/v0.16/) · [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/)
```julia
# K-relations: every tuple carries an annotation in a commutative semiring (K, ⊕, ⊗, 0, 1).
# Provenance polynomials ℕ[X]: monomials are sorted symbol lists, a polynomial maps monomials to coefficients.
const Poly = Dict{Vector{Symbol},Int}
padd(p, q) = mergewith(+, p, q)
pmul(p, q) = (r = Poly(); for (m, a) in p, (n, b) in q; k = sort([m; n]); r[k] = get(r, k, 0) + a * b; end; r)
var(x) = Poly([x] => 1)
# The query Q(x, z) = (∃y. R(x, y) ∧ S(y, z)) ∪ T(x, z), evaluated in an arbitrary semiring.
function Q(R, S, T, add, mul, zero)
    out = Dict{Tuple{Int,Int},Any}()
    for ((x, y), a) in R, ((y2, z), b) in S
        y == y2 && (out[(x, z)] = add(get(out, (x, z), zero), mul(a, b)))   # join = ⊗, projection = ⊕
    end
    for (k, c) in T; out[k] = add(get(out, k, zero), c); end                  # union = ⊕
    out
end
R = Dict((1, 2) => :r1, (1, 3) => :r2); S = Dict((2, 4) => :s1, (3, 4) => :s2); T = Dict((1, 4) => :t1)
lift(D, f) = Dict(k => f(v) for (k, v) in D)
prov = Q(lift(R, var), lift(S, var), lift(T, var), padd, pmul, Poly())
prov[(1, 4)]          # r1·s1 + r2·s2 + t1: the three ways (1, 4) was derived
# Any valuation X → K extends uniquely to a semiring homomorphism ℕ[X] → K (ℕ[X] is free).
evalpoly(p, v, add, mul, zero, one) = reduce(add, (reduce(mul, (v[x] for x in m); init = one) * c for (m, c) in p); init = zero)
counts = Dict(:r1 => 2, :r2 => 1, :s1 => 3, :s2 => 1, :t1 => 1)        # bag semantics: multiplicities
Q(lift(R, x -> counts[x]), lift(S, x -> counts[x]), lift(T, x -> counts[x]), +, *, 0)[(1, 4)] ==
    evalpoly(prov[(1, 4)], counts, +, *, 0, 1)                             # true, both are 8
# tropical semiring (min, +): costs; the answer is the cheapest derivation
cost = Dict(:r1 => 1.0, :r2 => 5.0, :s1 => 1.0, :s2 => 0.0, :t1 => 4.0)
trop = Q(lift(R, x -> cost[x]), lift(S, x -> cost[x]), lift(T, x -> cost[x]), min, +, Inf)[(1, 4)]
trop == minimum(sum(cost[x] for x in m) for (m, c) in prov[(1, 4)])     # true, both are 2.0
```
tab: Lean
```lean
import Mathlib
-- ℕ[X] is free: a valuation X → K extends to a ring hom MvPolynomial X ℕ →+* K (here K a comm. semiring).
#check @MvPolynomial.eval₂Hom      -- (f : R →+* S₁) → (σ → S₁) → MvPolynomial σ R →+* S₁
#check @MvPolynomial.aeval         -- the R-algebra version
-- evaluating the provenance of (1,4), r1*s1 + r2*s2 + t1, at bag multiplicities (2,3,1,1,1) gives 8
open MvPolynomial in
example : eval (fun i : Fin 5 => ([2, 1, 3, 1, 1] : List ℕ).getD i 0)
    (X 0 * X 2 + X 1 * X 3 + X 4 : MvPolynomial (Fin 5) ℕ) = 8 := by
  simp [eval_add, eval_mul, eval_X]
```
````
