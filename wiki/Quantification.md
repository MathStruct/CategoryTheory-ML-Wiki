#definition #example #theorem

In a [[Topos]], the quantifiers $\forall$ and $\exists$ turn a [[Predicate]] $p : S \times T \to \Omega$ of $n + 1$ variables into predicates $\forall(t : T).\, p(s, t)$ and $\exists(t : T).\, p(s, t)$ on $S$. They are defined purely from the topos structure:

- **Universal**: $\forall_t p$ is the [[Subobject]] of $S$ obtained by pulling back $\mathsf{true}_T : 1 \to \Omega^T$ (the currying of $1 \times T \to 1 \xrightarrow{\mathsf{true}} \Omega$) along the currying $p' : S \to \Omega^T$ of $p$ ([[Currying]], [[Exponential Object]]).
- **Existential**: take the subobject $\{S \times T \mid p\} \rightarrowtail S \times T$ classified by $p$, compose with the projection $\pi_S : S \times T \to S$, and take the [[Epi-Mono Factorization]]; the mono part $\exists_t p \rightarrowtail S$ is the image.

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}
\forall_t p \arrow[r] \arrow[d] \arrow[dr, phantom, "\lrcorner", very near start] & 1 \arrow[d, "\mathsf{true}_T"] & & \{S \times T \mid p\} \arrow[r] \arrow[d, two heads] & S \times T \arrow[d, "\pi_S"] \\
S \arrow[r, "p'"'] & \Omega^T & & \exists_t p \arrow[r, tail] & S
\end{tikzcd}
\end{document}
```

> Sources: 7 Sketches §7.4.4 ("Quantification"), Example 7.65, Exercises 7.66–7.68; §7.2.1 (epi-mono factorizations "in any topos"); DaoFP §11.4–11.5 (dependent sum and product as adjoints to substitution).

## In $\mathbf{Set}$

For $p : \mathbb{N} \times \mathbb{Z} \to \mathbb{B}$, $p(n, z) = [n \leq |z|]$: $\forall(z : \mathbb{Z}).\, p(n, z)$ holds exactly for $n = 0$, $\exists(z : \mathbb{Z}).\, p(n, z)$ for all $n$, $\forall(n : \mathbb{N}).\, p(n, z)$ for no $z$, and $\exists(n : \mathbb{N}).\, p(n, z)$ for all $z$ ([[7S Exercise 7.66]]).

## In a sheaf topos $\mathbf{Shv}(X)$

For a section $s \in S(U)$:
- $(\forall(t : T).\, p(s, t))(s)$ is the *largest* open $V \subseteq U$ such that $p(s|_V, t) = V$ for all $t \in T(V)$.
- $(\exists(t : T).\, p(s, t))(s)$ is the *union* $V = \bigcup_i V_i$ of all opens $V_i \subseteq U$ for which some $t_i \in T(V_i)$ satisfies $p(s|_{V_i}, t_i) = V_i$. If the result is all of $U$ this does *not* mean a single $t \in T(U)$ works — only a cover $U = \bigcup U_i$ with local witnesses $t_i$: "the existential quantifier is doing a lot of work under the hood, taking coverings into account".

Example 7.65: $S$ = people, $T$ = newsworthy items, $p(s, t)$ = "$s$ is worried about $t$". Then $\forall t.\, p(s,t)$ is the time during which $s$ is worried about *everything* in the news ([[7S Exercise 7.67]]), and $\exists t.\, p(s, t)$ is the time during which $s$ is worried about *something* — the worrying item being allowed to change over time ([[7S Exercise 7.68]]).

## Adjoint form

Quantifiers are the [[Adjunction|adjoints]] of pullback: $\exists_\pi \dashv \pi^* \dashv \forall_\pi$ on subobject posets $\mathrm{Sub}(S \times T) \rightleftarrows \mathrm{Sub}(S)$ — the same triple as [[Direct Image, Preimage, and Dual Image]] $f_! \dashv f^* \dashv f_*$ for sets, and as [[Dependent Sum]] $\dashv$ [[Base Change Functor|substitution]] $\dashv$ [[Dependent Product]] for types (Lawvere: "quantifiers are adjoints").

````tabs
tab: Julia
```julia
using Catlab
# Set: quantify the finite predicate p(n, z) = (n ≤ |z|) on 0:3 × -3:3
N = 0:3; Z = -3:3
p(n, z) = n <= abs(z)
[n for n in N if all(p(n, z) for z in Z)]      # ∀z: [0]
[n for n in N if any(p(n, z) for z in Z)]      # ∃z: [0, 1, 2, 3]
# ∃ as the image of a subobject along a projection, in FinSet:
S = FinSet(length(N)); T = FinSet(length(Z))
P = product(S, T); πS = proj1(P)
sub = Subobject(ob(P), [i for i in 1:length(ob(P)) if p(N[πS(i)], Z[proj2(P)(i)])])
image = first(epi_mono(hom(sub) ⋅ πS))          # ∃_t p as the epi part's codomain
collect(compose(hom(sub), πS)) |> unique |> sort  # elements of S in ∃z.p
```
tab: Lean
```lean
import Mathlib
-- quantifiers as adjoints to preimage on Set: ∃ (image) ⊣ preimage ⊣ ∀ (dual image)
#check @Set.image_subset_iff        -- f '' s ⊆ t ↔ s ⊆ f ⁻¹' t   (∃_f ⊣ f^*)
#check @Set.preimage               -- f^*
#check @CategoryTheory.Subobject.pullback
```
tab: Haskell
```haskell
-- quantifying a finite predicate over T
forallT, existsT :: [t] -> (s -> t -> Bool) -> (s -> Bool)
forallT ts p s = all (p s) ts
existsT ts p s = any (p s) ts
-- ∃ as image: existsT ts p s == s `elem` map fst (filter (uncurry p) [(s', t) | s' <- [s], t <- ts])
```
````
