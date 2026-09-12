#theorem #proof #program

**Yoneda lemma.** Let $\mathcal{C}$ be a (locally small) [[Category]], $F : \mathcal{C} \to \mathbf{Set}$ a [[Functor]], and $X$ an object. Then there is a bijection
$$[\mathcal{C}, \mathbf{Set}]\big(\mathrm{Hom}(X, -),\, F\big) \;\cong\; F(X),$$
natural in both $X$ and $F$. Dually (contravariant Yoneda), $[\mathcal{C}^{\mathrm{op}}, \mathbf{Set}](\mathrm{Hom}(-, X), F) \cong F(X)$ for a [[Presheaf]] $F$.

> Sources: Kittenlab Lecture 12 ("That's Yoneda, Babe"); DaoFP §3.3 ("Reasoning with Arrows"), §9.6 ("The Yoneda Lemma", "Yoneda lemma in programming", "The contravariant Yoneda lemma"), §9.10, §17.6 ("Ninja Yoneda"), §20.4 (enriched); 7 Sketches Exercise 1.66 ([[Yoneda Lemma for Preorders]]), Remark 1.82; DaoFP Preface: "the fundamental theorem of category theory".

## Intuition

- "A vibe check for category theory": we say all the time that all that matters is the morphisms out of (or into) an object; the Yoneda lemma formalizes this (Kittenlab).
- The trivial case: for a set $A$, $A \cong \mathrm{Hom}_{\mathbf{Set}}(1, A)$ — "$A^1 \cong A$". For [[Graph|graphs]]: the vertices of $G$ are the maps from the one-vertex graph $y_V$, $G(V) \cong \mathrm{Hom}(y_V, G)$, and the edges are maps from the one-edge graph, $G(E) \cong \mathrm{Hom}(y_E, G)$, because *naturality* forces where $\mathrm{src}, \mathrm{tgt}$ go once $\mathrm{id}_E$ is sent to an edge (Kittenlab).
- DaoFP: $\mathrm{Hom}(a, -)$ is the "panoramic, very detailed view of $\mathcal{C}$ from the vantage point of $a$"; an arbitrary $F$ is another, lossy model; a natural transformation embeds one model in the other, and the set of all such is "fully determined by the set $F a$". "The proof starts with a single identity arrow and lets naturality propagate it across the whole category."

## Proof (Kittenlab / DaoFP)

*Forward.* Given $x \in F(X)$ define $x^* : \mathrm{Hom}(X, -) \Rightarrow F$ by $x^*_Y(f) := F(f)(x)$ for $f : X \to Y$. (Naturality in $Y$ is [[DaoFP Exercise 9.6.2]].)

*Backward.* Given $\alpha : \mathrm{Hom}(X, -) \Rightarrow F$, take $\alpha_X(\mathrm{id}_X) \in F(X)$ — the **Yoneda trick**: substitute $X$ for the variable to get an endo-hom-set and pick its canonical element.

*Inverse.* $x^*_X(\mathrm{id}_X) = F(\mathrm{id}_X)(x) = x$. Conversely, for $\alpha$ and $f : X \to Y$, the naturality square for $f$ applied to $\mathrm{id}_X$ gives
$$\alpha_Y(f) = \alpha_Y(\mathrm{id}_X \circ f) = \alpha_Y(\mathrm{Hom}(X, f)(\mathrm{id}_X)) = F(f)(\alpha_X(\mathrm{id}_X)),$$
so $\alpha = (\alpha_X(\mathrm{id}_X))^*$: "where $f$ goes is wholly determined by where $\mathrm{id}_X$ goes". $\blacksquare$ ([[DaoFP Exercise 9.6.1]] handles $F(X) = \varnothing$.)

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}
\mathrm{Hom}(X, X) \arrow[r, "\alpha_X"] \arrow[d, "f \circ -"'] & F(X) \arrow[d, "F(f)"] \\
\mathrm{Hom}(X, Y) \arrow[r, "\alpha_Y"'] & F(Y)
\end{tikzcd}
\end{document}
```

## Consequences

- **Corollary** (Kittenlab): $\mathrm{Hom}_{\mathcal{C}}(Y, X) \cong [\mathcal{C}, \mathbf{Set}](y_X, y_Y)$ — take $F = y_Y$. Hence $y_X \cong y_Y$ iff $X \cong Y$: [[Representable Functor|representing objects]] are unique up to isomorphism, and universal constructions defined by representability ([[Coproduct]], [[Coequalizer]], [[Colimit]], [[Product]], …) are well defined. E.g. $\mathrm{Hom}_{\mathsf{Gr}}(E, V) \cong \mathrm{Hom}(y_V, y_E)$: two morphisms each.
- The [[Yoneda Embedding]] $\mathcal{C} \to [\mathcal{C}^{\mathrm{op}}, \mathbf{Set}]$, $x \mapsto \mathrm{Hom}(-, x)$, is fully faithful (DaoFP §9.7); $\mathrm{Hom}$-sets naturally isomorphic implies objects isomorphic ([[Isomorphism]]).
- Every isomorphism of hom-sets used in [[Adjunction|adjunctions]], [[Unit and Counit of an Adjunction|units/counits]], [[Universal Arrow|universal arrows]] and [[Adjoint Functor Theorem|adjoint functor theorems]] is manipulated via the Yoneda trick (DaoFP §10). Preorder version: $p \leq p'$ iff $\uparrow p' \subseteq \uparrow p$ ([[Yoneda Lemma for Preorders]]).
- **In programming** (DaoFP §9.6): `forall x. (a -> x) -> f x ≅ f a`, with `yoneda g = g id` and `yoneda_1 y = \h -> fmap h y` (really the enriched Yoneda lemma in the self-enriched $\mathbf{Hask}$). For $f = \mathrm{Id}$: `a ≅ forall x. (a -> x) -> x` — the [[Continuation|continuation-passing]] transform (a value is replaced by a function taking a handler/callback), used for remote values and for turning recursion tail-recursive; continuations form a [[Continuation Monad|monad]]. Contravariant: `coyoneda g = g id`, `coyoneda_1 y = \h -> contramap h y`.
- Enriched and coend forms: the [[Ninja Yoneda Lemma]] $\int_x [\mathcal{C}(a, x), F x] \cong F a$ and $\int^x \mathcal{C}(x, a) \times F x \cong F a$ (DaoFP §17.6, §20.4).

````tabs
tab: Julia
```julia
# Kittenlab Lecture 12 for graphs: elements of G(V) ↔ maps from the one-vertex graph
using Catlab
G = @acset Graph begin V = 3; E = 2; src = [1, 2]; tgt = [2, 3] end
yV = representable(Graph, :V); yE = representable(Graph, :E)
# forward: x ∈ G(E) ↦ x* : yE → G sending id_E ↦ x (and src, tgt forced)
# (in Catlab's representable(Graph, :E) the edge runs from vertex 2 to vertex 1)
x_star(e) = ACSetTransformation(yE, G; E = [e], V = [tgt(G, e), src(G, e)])
all(is_natural(x_star(e)) for e in edges(G))                   # true
# backward: α ↦ α_E(id_E)
sort([α[:E](1) for α in homomorphisms(yE, G)]) == edges(G)     # true: the bijection G(E) ≅ Hom(yE, G)
```
tab: Lean
```lean
#check CategoryTheory.yonedaEquiv        -- (yoneda.obj X ⟶ F) ≃ F.obj (op X)
#check CategoryTheory.coyonedaEquiv      -- (coyoneda.obj (op X) ⟶ F) ≃ F.obj X   (covariant form)
#check CategoryTheory.yonedaLemma        -- the natural isomorphism, natural in X and F
#check CategoryTheory.Yoneda.fullyFaithful
```
tab: Haskell
```haskell
{-# LANGUAGE RankNTypes #-}
-- DaoFP §9.6: the Yoneda lemma as a pair of inverse functions
yoneda :: Functor f => (forall x. (a -> x) -> f x) -> f a
yoneda g = g id                                 -- the Yoneda trick

yoneda_1 :: Functor f => f a -> (forall x. (a -> x) -> f x)
yoneda_1 y = \h -> fmap h y

-- contravariant version
coyoneda :: Contravariant f => (forall x. (x -> a) -> f x) -> f a
coyoneda g = g id
coyoneda_1 :: Contravariant f => f a -> (forall x. (x -> a) -> f x)
coyoneda_1 y = \h -> contramap h y

-- f = Identity: continuation passing style,  a ≅ forall x. (a -> x) -> x
toCPS :: a -> (forall x. (a -> x) -> x)
toCPS a = \k -> k a
fromCPS :: (forall x. (a -> x) -> x) -> a
fromCPS c = c id
```
````
