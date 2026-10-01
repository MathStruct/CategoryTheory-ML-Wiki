#definition #theorem #example #program

A **congruence** on an algebra $A$ for a signature $\Sigma$ (a set with operations $f^A : A^{n} \to A$) is an [[Equivalence Relation]] $\theta \subseteq A \times A$ that is compatible with every operation:

$$
a_1 \,\theta\, b_1, \;\dots,\; a_n \,\theta\, b_n \;\Longrightarrow\; f^A(a_1, \dots, a_n) \;\theta\; f^A(b_1, \dots, b_n).
$$

Equivalently, $\theta$ is an equivalence relation that is also a **subalgebra** of $A \times A$. Congruences are exactly the equivalence relations one can quotient by and still have an algebra: the [[Quotient Set]] $A/\theta$ carries operations $f([a_1], \dots, [a_n]) = [f(a_1, \dots, a_n)]$, well defined *because* of compatibility. The **congruence closure** of a relation $R$ is the smallest congruence containing it.

> Sources: Burris & Sankappanavar, *A Course in Universal Algebra* (Springer GTM 78, 1981; free online edition), Chapter II §§5–6 (congruences, quotient algebras, the homomorphism and isomorphism theorems); Zhang, Wang, Willsey & Tatlock [arXiv:2108.02290](https://arxiv.org/abs/2108.02290) ([[Relational E-Matching|notes]]) Definition 3 (congruence relation and congruence closure on terms); Nelson & Oppen, *Fast decision procedures based on congruence closure*, JACM 27 (1980); Downey, Sethi & Tarjan, *Variations on the common subexpression problem*, JACM 27 (1980). Congruences on the paths of a graph, which present a category, are in [[Database Schema]] (CTfS §3.5).

## Congruences are kernels

For a homomorphism $h : A \to B$ the **kernel** $\ker h = \{(a, a') \mid h(a) = h(a')\}$ is a congruence, and every congruence is the kernel of its quotient map $q : A \to A/\theta$. The **homomorphism theorem** says $h$ factors uniquely as

$$
A \xrightarrow{\;q\;} A/\ker h \xrightarrow{\;\cong\;} h(A) \hookrightarrow B,
$$

the algebraic shadow of [[Epi-Mono Factorization]]. Categorically, $\ker h$ is the **kernel pair** of $h$ — the [[Pullback]] of $h$ along itself — and a congruence is an *internal equivalence relation* in the category of $\Sigma$-algebras. That category is regular and every internal equivalence relation is a kernel pair ("congruences are effective"), which is what makes quotients by congruences well behaved.

## Congruences on terms

The free $\Sigma$-algebra is the term algebra $T(\Sigma, X)$, the [[Initial Algebra]] of the polynomial functor of the signature. A set of equations $E$ generates a congruence $\equiv_E$ on terms — the congruence closure of all instances of $E$ — and $T(\Sigma, X)/{\equiv_E}$ is the **free algebra of the equational theory** $(\Sigma, E)$. Two terms are provably equal from $E$ iff they are congruent (Birkhoff's completeness theorem). This is the bridge to [[Lawvere Theory|Lawvere theories]] (the theory *is* the category of these quotiented term algebras) and to [[Functorial Semantics]].

For *ground* equations the congruence closure is decidable in $O(n \log n)$ — Nelson–Oppen and Downey–Sethi–Tarjan — and the data structure that does it is the [[E-Graph]]. For equations with variables (the word problem of an equational theory) it is undecidable in general, which is why equality saturation needs budgets.

## Why compilers care

A rewrite $u \to v$ may be applied *inside* any context only if the equivalence it generates is a congruence: $t \approx u \Rightarrow C[t] \approx C[u]$. Equivalences built from sound rewrite rules and from definitional equality are congruences by construction. Equivalences "established" by testing are not: two sorting functions that agree on every test can differ on stability, and a context that observes stability tells them apart. The logical-relations method ([[Logical Relations]]) exists precisely to prove that a semantic equivalence is a congruence without quantifying over all contexts ([[Contextual Equivalence|contextual equivalence]] is the largest congruence that respects observations).

## Sophia

Sophia's `EQUIV` ladder is a ladder of relations of decreasing strength; only the ones that are congruences (`alpha`, `defeq`, sound `rewrite`, proved `observational`) may license substitution. See [Equivalence and Witnesses](https://mathstruct.org/Sophia/vault/Design/Equivalence-and-Witnesses) ("the congruence trap").

````tabs
tab: Julia
**Docs:** plain Julia — Catlab has no dedicated API for this; related: [Catlab v0.16 docs](https://algebraicjulia.github.io/Catlab.jl/v0.16/) · [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/)
```julia
# The algebra (ℤ/12, s) with one unary operation s(x) = 2x mod 12.
n = 12; s(x) = mod(2x, n)
# Congruence closure of R = {(0, 4)}: smallest equivalence containing R and closed under s.
parent = collect(0:n-1)
find(x) = parent[x+1] == x ? x : (parent[x+1] = find(parent[x+1]))
unite!(x, y) = (parent[find(x)+1] = find(y))
function close!(pairs)
    queue = copy(pairs)
    while !isempty(queue)
        x, y = pop!(queue)
        find(x) == find(y) && continue
        unite!(x, y)
        push!(queue, (s(x), s(y)))            # congruence: x ~ y ⇒ s(x) ~ s(y)
    end
end
close!([(0, 4)])
classes = Dict{Int,Vector{Int}}(); for x in 0:n-1; push!(get!(classes, find(x), Int[]), x); end
sort(sort.(collect(values(classes))))   # [[0, 4, 8], [1], [2], …]: 0 ~ 4 forces 0 ~ 8 = s(4)
# Every congruence is the kernel of a homomorphism: the quotient map q(x) = class of x.
q(x) = find(x)
all(q(s(x)) == q(s(y)) for x in 0:n-1, y in 0:n-1 if q(x) == q(y))   # true: s descends to the quotient
length(classes)                                                       # 10 elements in the quotient algebra
```
tab: Lean
```lean
import Mathlib
-- Mathlib's `Con M`: congruences on a multiplicative structure (one binary operation).
-- The kernel of a monoid hom is a congruence, and the first isomorphism theorem holds.
#check @Con.ker                      -- (f : M →* P) → Con M
#check @Con.quotientKerEquivRange    -- (Con.ker f).Quotient ≃* MonoidHom.mrange f
#check @conGen                       -- congruence closure of a relation: (M → M → Prop) → Con M
example {M : Type*} [Monoid M] (c : Con M) {a b x : M} (h : c a b) : c (x * a) (x * b) :=
  c.mul (c.refl x) h
```
tab: Haskell
```haskell
-- A congruence for s(x) = 2x mod 12, as the kernel of the quotient map.
s :: Int -> Int
s x = (2 * x) `mod` 12

-- quotient map for the congruence generated by 0 ~ 4 (classes {0,4,8}, singletons otherwise)
q :: Int -> Int
q x = if x `elem` [0, 4, 8] then 0 else x

main :: IO ()
main = print (and [ q (s x) == q (s y) | x <- [0 .. 11], y <- [0 .. 11], q x == q y ])
-- True: s is well defined on the quotient, i.e. ker q is a congruence
```
````
