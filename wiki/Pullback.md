#definition #example #theorem

The **pullback** of a [[Cospan]] $X \xrightarrow{f} A \xleftarrow{g} Y$ is its [[Limit]]: an object $X \times_A Y$ with projections $c_x : X \times_A Y \to X$, $c_y : X \times_A Y \to Y$ such that $c_x \mathbin{;} f = c_y \mathbin{;} g$, universal among such squares. The diagonal leg $c_a$ of the cone is superfluous (it equals $c_x \mathbin{;} f$), so pullback squares are drawn without it, marked with a corner symbol $\lrcorner$.

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}
X \times_A Y \arrow[r, "c_y"] \arrow[d, "c_x"'] \arrow[dr, phantom, "\lrcorner", very near start] & Y \arrow[d, "g"] \\
X \arrow[r, "f"'] & A
\end{tikzcd}
\end{document}
```

> Sources: 7 Sketches Example 3.99, Remark 3.100, §7.2.1 (pullbacks in a topos), Definition 3.68 (the *other* "pullback": [[Data Migration Functor|$\Delta_F$]]); Kittenlab Lecture 13 ("typed products" = products in a [[Slice Category]]), 14 (pullback of subsets); DaoFP §11.2 ("Pullbacks", "Substitution", "Base-change functor").

- **In $\mathbf{Set}$**: $X \times_A Y = \{(x, y) \mid f(x) = g(y)\}$ ([[Finite Limits in Set]]). Example 3.99: $X = \underline{6}$, $Y = \underline{4}$, $A = \{\mathsf{red}, \mathsf{blue}, \mathsf{black}\}$: the pullback selects pairs $(i, j)$ with the same colour. Pullbacks are how [[Data Migration Functor|$\Pi$-queries]] "pair and select data" (database *join*).
- **Typed products** (Kittenlab): the product in $\mathbf{FinSet}/T$ of $(A, t)$ and $(A', t')$ is the pullback $A \times_T A'$ — pairs of elements of the same type. **Pullback of a subset** $\chi : Y \to \mathbb{B}$ along $f : X \to Y$ is $\chi \circ f$, the preimage; in the subobject picture this is literally a categorical pullback of $U \hookrightarrow Y$ along $f$ ([[Direct Image, Preimage, and Dual Image]]).
- **Base change / substitution** (DaoFP §11.2): pulling back a family $p : e \to c$ (a [[Dependent Type]]) along $f : c' \to c$ gives the family $f^* e \to c'$ — substituting $f$ into the type; $f^* : \mathcal{C}/c \to \mathcal{C}/c'$ is the **base-change functor**, with adjoints [[Dependent Sum|$\Sigma_f$]] $\dashv f^* \dashv$ [[Dependent Product|$\Pi_f$]].
- Monos are pullback-stable; in a [[Topos]] every mono is a pullback of $\mathsf{true} : 1 \to \Omega$ ([[Subobject Classifier]]). Pullback of a [[Sheaf|covering]] gives restriction. The pullback along a functor $\Delta_F$ is, via the [[Category of Elements]], a pullback in $\mathbf{Cat}$ (Remark 3.100).
- Dual: [[Pushout]].

````tabs
tab: Julia
```julia
using Catlab
f = FinFunction([1, 2, 2, 3, 1, 3], 3)      # colours of 6 things
g = FinFunction([1, 1, 3, 2], 3)            # colours of 4 things
P = pullback(f, g)
apex(P)                                     # FinSet(8): pairs with equal colour
collect(zip(collect(legs(P)[1]), collect(legs(P)[2])))
# pullback of graphs / ACSets works the same way (pointwise)
```
tab: Lean
```lean
#check CategoryTheory.Limits.pullback        -- pullback f g with pullback.fst, pullback.snd, pullback.lift
#check CategoryTheory.Limits.pullback.condition   -- fst ≫ f = snd ≫ g
#check CategoryTheory.Limits.Types.pullbackIsoPullback   -- in Type: { p : X × Y // f p.1 = g p.2 }
#check CategoryTheory.Over.pullback           -- base change C/c ⥤ C/c'
```
tab: Haskell
```haskell
-- the pullback in Hask as a subtype of the product (on finite carriers)
pullbackSet :: Eq c => [a] -> [b] -> (a -> c) -> (b -> c) -> [(a, b)]
pullbackSet as bs f g = [ (a, b) | a <- as, b <- bs, f a == g b ]

-- base change of a "family" p :: e -> c along f :: c' -> c: pairs (c', e) with f c' = p e
```
````
