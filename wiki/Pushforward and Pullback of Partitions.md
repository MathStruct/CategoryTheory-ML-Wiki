#definition #example #theorem

Any [[Function]] $g : S \to T$ induces a [[Galois Connection]]
$$g_! : \mathrm{Prt}(S) \rightleftarrows \mathrm{Prt}(T) : g^*$$
between the [[Preorder of Partitions|preorders of partitions]].

> Sources: 7 Sketches §1.4.2, Examples 1.68, 1.102, 1.104, Exercises 1.69, 1.103, 1.105, 1.106.

**Left adjoint (pushforward) $g_!$.** Given a partition $\sim_S$ of $S$, declare $t_1 \sim_T t_2$ if there are $s_1 \sim_S s_2$ with $g(s_1) = t_1$ and $g(s_2) = t_2$; this need not be transitive, so take the transitive closure. "For a seasoned category theorist": take the surjection $c : S \twoheadrightarrow P$ and [[Pushout|push out]] along $g$ to get a surjection $T \twoheadrightarrow P \sqcup_S T$.

**Right adjoint (pullback) $g^*$.** Given a partition $\sim_T$, set $s_1 \sim_S s_2$ iff $g(s_1) \sim_T g(s_2)$. Categorically: compose $c : T \twoheadrightarrow P$ with $g$ and take the [[Epi-Mono Factorization]] to get the surjection $S \twoheadrightarrow \mathrm{im}(g \mathbin{;} c)$. When $g$ is surjective the factorization is unnecessary and $g^*(c) = g \mathbin{;} c$ (Example 1.68).

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}
S \arrow[r, "g"] \arrow[d, "c"', two heads] & T \arrow[d, two heads] & & S \arrow[r, "g"] \arrow[d, two heads] & T \arrow[d, "c", two heads] \\
P \arrow[r] & P \sqcup_S T & & \mathrm{im}(g \mathbin{;} c) \arrow[r, hook] & P
\end{tikzcd}
\end{document}
```

**Example 1.102.** $S = \{1,2,3,4\}$, $T = \{12, 3, 4\}$, $g(1) = g(2) = 12$, $g(3) = 3$, $g(4) = 4$. The partition $(1)(2)(34)$ of $S$ is pushed forward to $(12)(34)$. Example 1.104 shows a pullback along a non-surjective map. [[7S Exercise 1.106]] checks the adjunction formula $g_!(c) \leq d \iff c \leq g^*(d)$ in examples.

This is the preorder shadow of the [[Data Migration Functor|data migration]] adjunction $\Sigma_g \dashv \Delta_g$ and of the [[Direct Image, Preimage, and Dual Image|image/preimage]] adjunction; it is why every function, not just surjections, acts contravariantly on partitions.

````tabs
tab: Julia
```julia
using Catlab
# partitions as surjections out of a FinSet; g : S → T
S, T = FinSet(4), FinSet(3)
g = FinFunction([1, 1, 2, 3], T)             # 1,2 ↦ 12; 3 ↦ 3; 4 ↦ 4
c = FinFunction([1, 2, 3, 3], 3)             # (1)(2)(34)

# pushforward g_!(c): pushout of c and g, then the leg out of T
po = pushout(c, g)
g_shriek_c = legs(po)[2]                     # T → P ⊔_S T ; parts: (12)(34)

# pullback g^*(d): s₁ ~ s₂ iff d(g(s₁)) == d(g(s₂)); the epi part of g⋅d
d = FinFunction([1, 1, 2], 2)                # (12 3)(4) on T
g_star_d = first(epi_mono(compose(g, d)))    # FinFunction([1,1,1,2], 2): (123)(4) on S
```
tab: Haskell
```haskell
-- partitions as functions to labels; pullback along g is precomposition
pullbackPart :: (s -> t) -> (t -> l) -> (s -> l)
pullbackPart g d = d . g

-- pushforward: relate t₁ ~ t₂ if some s₁ ~ s₂ map to them, then close transitively
pushforwardPart :: (Eq s, Eq t) => [s] -> (s -> t) -> (s -> s -> Bool) -> (t -> t -> Bool)
pushforwardPart ss g c = closure
  where step t1 t2 = t1 == t2 || or [ c s1 s2 | s1 <- ss, s2 <- ss, g s1 == t1, g s2 == t2 ]
        ts = map g ss
        closure t1 t2 = go [t1] []
          where go [] _ = False
                go (x:xs) seen | x == t2 = True
                               | x `elem` seen = go xs seen
                               | otherwise = go (xs ++ [y | y <- ts, step x y]) (x:seen)
```
````
