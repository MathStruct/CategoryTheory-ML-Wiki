#definition #example #theorem #proof

Let $L : \mathcal{C} \to \mathcal{D}$ and $R : \mathcal{D} \to \mathcal{C}$ be [[Functor|functors]]. $L$ is **left adjoint** to $R$ (and $R$ **right adjoint** to $L$), written $L \dashv R$, if for all $c \in \mathcal{C}$, $d \in \mathcal{D}$ there is an isomorphism of hom-sets

$$
\alpha_{c,d} : \mathcal{C}(c, R(d)) \xrightarrow{\ \cong\ } \mathcal{D}(L(c), d)
$$

natural in $c$ and $d$ (as functors $\mathcal{C}^{\mathrm{op}} \times \mathcal{D} \to \mathbf{Set}$: for $f : c' \to c$, $h : c \to Rd$ and $g : d \to d'$, $\alpha_{c',d'}(f \mathbin{;} h \mathbin{;} R g) = L f \mathbin{;} \alpha_{c,d}(h) \mathbin{;} g$). The image $\alpha_{c,d}(h)$ of $h : c \to R d$ is its **mate** (DaoFP: **transpose**), and vice versa. The turnstile $\dashv$ always points *from the left adjoint to the right adjoint*.

Intuition (Category Theory for Scientists §5.1): adjoint functors are dictionaries between categories that are *not on the same conceptual level*, like a baby's repeatable noises and an adult's meaningful words. The left adjoint promotes every noise to a word of unknown meaning ("I wonder what she means by *Ronnon*"), the right adjoint forgets the meaning of words and hears them as noises. The hom-set bijection says: a way to interpret the freely-promoted words in our lexicon is the same as a way for the baby to emulate our sounds.

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}
\mathcal{C} \arrow[r, bend left=35, "L"] \arrow[r, phantom, "\bot"] & \mathcal{D} \arrow[l, bend left=35, "R"]
\end{tikzcd}
\end{document}
```

> Sources: 7 Sketches §3.4.2 (Definition 3.70, Examples 3.71–3.74, Exercise 3.73), §3.4.3; DaoFP Chapter 10 (§10.1–10.11), §15 (monads from adjunctions); CTfS §5.1 (Definition 5.1.1.1, Proposition 5.1.1.2, Examples 5.1.1.4–5.1.1.7, §5.1.1.10 quantifiers, §5.1.2–5.1.4); Kittenlab (implicitly: Lecture 5 discrete/codiscrete, Lecture 7 free monoid unit). Preorder case: [[Galois Connection]] ("Galois connections and adjunctions between the corresponding categories are exactly the same thing", Example 3.71: hom-sets with one or zero elements).

## Examples

- **Currying** (Example 3.72, DaoFP §10.1): $(- \times B) \dashv (-)^B$ on $\mathbf{Set}$, $\mathbf{Set}(A \times B, C) \cong \mathbf{Set}(A, C^B)$; "if you give me just $a$, I'll return a function $B \to C$ waiting for the $B$ input". Defines the [[Exponential Object]] and [[Cartesian Closed Category|cartesian closed categories]]; $\mathbf{Cat}$ is one too, with $[\mathcal{D}, \mathcal{E}]$ ([[7S Chapter 3 Exercises#Exercise 3.73|7S Exercise 3.73]]: on morphisms $f \times B$ and $f^B = (- \mathbin{;} f)$; currying $+$ gives $p(3) = (n \mapsto n + 3)$).
- **Sum and product** (DaoFP §10.2): $(+) \dashv \Delta \dashv (\times)$ with the [[Diagonal Functor]]; more generally $\mathrm{Colim} \dashv \Delta \dashv \mathrm{Lim}$ (§10.4).
- **Free/forgetful** (Example 3.74, DaoFP §10.9): free [[Group|group]], [[Monoid|monoid]], ring, vector space $\dashv$ underlying set; [[Free Category|free category]] and free preorder on a graph $\dashv$ underlying graph; [[Discrete Category|discrete]] $\dashv$ underlying $\dashv$ [[Codiscrete Category|codiscrete]] (preorders, graphs, categories, topological spaces); abelianization $\dashv$ inclusion $\mathbf{Ab} \hookrightarrow \mathbf{Grp}$; [[Preorder Reflection]] $\dashv$ inclusion. See [[Free-Forgetful Adjunction]].
- **Data migration** (§3.4.3): $\Sigma_F \dashv \Delta_F \dashv \Pi_F$ ([[Data Migration Functor]]); [[Kan Extension|Kan extensions]] generalize.
- [[Dependent Sum|$\Sigma_f$]] $\dashv f^* \dashv$ [[Dependent Product|$\Pi_f$]] (DaoFP Ch. 11), [[Direct Image, Preimage, and Dual Image|$\exists_f \dashv f^* \dashv \forall_f$]] for subsets; $(- \otimes a) \dashv \underline{\mathrm{Hom}}(a, -)$ in a [[Monoidal Closed Category]].
- **Not every adjunction is symmetric** (CTfS Example 5.1.1.4): $\mathrm{List} \dashv U$ for $U : \mathbf{Mon} \to \mathbf{Set}$, but $\mathrm{List}$ is *not* right adjoint to $U$: the trivial monoid $\mathbf 1$ is initial, so $\mathbf{Mon}(\mathbf 1, \mathrm{List}\{a,b\})$ has one element while $\mathbf{Set}(U\mathbf 1, \{a,b\})$ has two.
- **Some functors have adjoints on both sides** (CTfS Examples 5.1.1.5–5.1.1.7, [[CTfS Chapter 5 Exercises#Exercise 5.1.1.6|CTfS Exercise 5.1.1.6]]): discrete $\dashv$ underlying set $\dashv$ indiscrete for preorders; for graphs, the vertex-set functor has left adjoint "no arrows" and right adjoint "one arrow between every ordered pair"; $\mathrm{Disc} \dashv \mathrm{Ob} \dashv \mathrm{Ind}$ for $\mathbf{Cat}$, and $\pi_0 \dashv \mathrm{Disc}$ (connected components, [[CTfS Chapter 5 Exercises#Exercise 5.1.1.9|CTfS Exercise 5.1.1.9]]).
- Every adjunction gives a [[Monad]] $RL$ and a [[Comonad]] $LR$ (DaoFP Ch. 15–16); every monad arises this way ([[Eilenberg-Moore Category]], [[Kleisli Category]]).

## Equivalent formulations (DaoFP §10.5–10.6)

- **Unit and counit**: $\eta : \mathrm{Id} \Rightarrow RL$ with $\eta_c := \alpha^{-1}(\mathrm{id}_{Lc})$, and $\varepsilon : LR \Rightarrow \mathrm{Id}$ with $\varepsilon_d := \alpha(\mathrm{id}_{Rd})$ (the Yoneda trick), satisfying the **triangle identities** $(\varepsilon \circ L) \cdot (L \circ \eta) = \mathrm{id}_L$ and $(R \circ \varepsilon) \cdot (\eta \circ R) = \mathrm{id}_R$. Conversely such $\eta, \varepsilon$ give the hom-set bijection: $f : c \to Rd \mapsto \varepsilon_d \circ Lf$ and $g : Lc \to d \mapsto Rg \circ \eta_c$ ([[DaoFP Chapter 10 Exercises#Exercise 10.5.2|DaoFP Exercise 10.5.2]]). This definition works in any [[2-Category]]. See [[Unit and Counit of an Adjunction]].
- **Universal arrows**: $L \dashv R$ iff for every $d$ there is a terminal object $(Rd, \varepsilon_d)$ in the [[Comma Category]] $L \downarrow d$ — a [[Universal Arrow]] from $L$ to $d$; dually initial objects $(Lc, \eta_c)$ in $c \downarrow R$. An adjunction is a "half-equivalence": if $\eta, \varepsilon$ are isomorphisms it is an [[Equivalence of Categories]].

## Properties

- [[Right Adjoints Preserve Limits]] and left adjoints preserve colimits (DaoFP §10.7; preorder version [[Right Adjoints Preserve Meets]]). Hence e.g. distributivity $(b + c) \times a \cong b \times a + c \times a$, since $(- \times a)$ is a left adjoint.
- Adjoints are unique up to natural isomorphism ([[7S Chapter 1 Exercises#Exercise 1.110|7S Exercise 1.110]] for preorders).
- Adjunctions compose: $L' \dashv R'$ and $L \dashv R$ give $(L' \circ L) \dashv (R \circ R')$; categories and adjunctions form $\mathbf{Adj}(\mathbf{Cat})$ (DaoFP §10.10).
- Existence: the [[Adjoint Functor Theorem]] (Freyd) — a limit-preserving functor from a complete category with a solution set has a left adjoint; preorder case [[Adjoint Functor Theorem for Preorders]]; programming instance: [[Defunctionalization]].
- $Lx$ [[Representable Functor|represents]] the co-presheaf $y \mapsto \mathcal{C}(x, Ry)$ and $Ry$ represents the presheaf $x \mapsto \mathcal{D}(Lx, y)$ ([[DaoFP Chapter 10 Exercises#Exercise 10.3.2|DaoFP Exercise 10.3.2]], [[DaoFP Chapter 10 Exercises#Exercise 10.3.3|DaoFP Exercise 10.3.3]]).

"Universal constructions are one of the most important themes of category theory: one gives some specified shape and says 'find me the best solution!'; category theory asks 'approximate from the left or the right?'" (7 Sketches §3.6). "A sculptor subtracts irrelevant stone until a sculpture emerges" (DaoFP Ch. 10).

````tabs
tab: Julia
**Docs:** [FinCats](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.FinCats) · [C-set morphisms](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.CSets) · [Data migration](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.FunctorialDataMigrations) · [ACSets API](https://algebraicjulia.github.io/ACSets.jl/stable/api/) · [Graphs](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/graphs/)
```julia
# Catlab: adjunctions appear as Σ ⊣ Δ ⊣ Π data migrations and as free/forgetful constructions
# (checked with Catlab 0.16.20)
using Catlab
@present SchDDS(FreeSchema) begin State::Ob; next::Hom(State, State) end
@acset_type DDS(SchDDS)
F = FinFunctor(Dict(:V => :State, :E => :State), Dict(:src => id(SchDDS[:State]), :tgt => :next),
               FinCat(SchGraph), FinCat(SchDDS))
Δ = DeltaMigration(F); Σ = SigmaMigrationFunctor(F, Graph, DDS)
G = cycle_graph(Graph, 3)          # Σ(G) is the free DDS on G: here the 3-cycle
I = @acset DDS begin State = 3; next = [2, 3, 1] end
# the adjunction Σ ⊣ Δ:  Hom(Σ G, I) ≅ Hom(G, Δ I)
length(homomorphisms(Σ(G), I)) == length(homomorphisms(G, migrate(Graph, I, Δ)))  # true (3 = 3)
# (on path_graph(Graph, 3) the free DDS is infinite — the last state needs a fresh `next` — and the chase does not terminate)
```
tab: Lean
```lean
#check CategoryTheory.Adjunction          -- structure: homEquiv, unit, counit, triangle laws; notation F ⊣ G
#check CategoryTheory.Adjunction.mkOfHomEquiv
#check CategoryTheory.Adjunction.mkOfUnitCounit
#check CategoryTheory.Adjunction.leftAdjointPreservesColimits
#check CategoryTheory.Adjunction.rightAdjointPreservesLimits
#check CategoryTheory.Adjunction.comp     -- composition of adjunctions
#check CategoryTheory.Adjunction.toMonad
-- currying: (- × B) ⊣ (B ⟶ -) in a cartesian closed category
#check CategoryTheory.exp.adjunction
```
tab: Haskell
```haskell
{-# LANGUAGE MultiParamTypeClasses, FunctionalDependencies #-}
-- DaoFP §10.3/§10.5: an adjunction between endofunctors, hom-set form and unit/counit form
class (Functor left, Functor right) => Adjunction left right | left -> right, right -> left where
  ltor   :: (left x -> y) -> (x -> right y)
  rtol   :: (x -> right y) -> (left x -> y)
  unit   :: x -> right (left x)
  counit :: left (right x) -> x
  ltor g = fmap g . unit
  rtol f = counit . fmap f

-- the currying adjunction (- , r) ⊣ (r -> -)
data L r x = L (x, r) deriving Functor
data R r x = R (r -> x) deriving Functor
instance Adjunction (L r) (R r) where
  unit x = R (\r -> L (x, r))
  counit (L (R f, r)) = f r

-- triangle identities (should be identities):
triangle  :: L r x -> L r x
triangle  = counit . fmap unit
triangle' :: R r x -> R r x
triangle' = fmap counit . unit
```
````
