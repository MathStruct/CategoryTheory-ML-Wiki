#definition #example #theorem #proof

Let $L : \mathcal{C} \to \mathcal{D}$ and $R : \mathcal{D} \to \mathcal{C}$ be [[Functor|functors]]. $L$ is **left adjoint** to $R$ (and $R$ **right adjoint** to $L$), written $L \dashv R$, if for all $c \in \mathcal{C}$, $d \in \mathcal{D}$ there is an isomorphism of hom-sets
$$\alpha_{c,d} : \mathcal{C}(c, R(d)) \xrightarrow{\ \cong\ } \mathcal{D}(L(c), d)$$
natural in $c$ and $d$ (as functors $\mathcal{C}^{\mathrm{op}} \times \mathcal{D} \to \mathbf{Set}$: for $f : c' \to c$ and $g : d \to d'$, $\alpha_{c',d'}(f \mathbin{;} h \mathbin{;} R g) = L f \mathbin{;} \alpha_{c,d}(h) \mathbin{;} g$). The image $\alpha_{c,d}(f)$ of $f : c \to R d$ is its **mate** (DaoFP: **transpose**), and vice versa. In diagrams the $\Rightarrow$ points in the direction of the left adjoint.

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}
\mathcal{C} \arrow[r, bend left, "L"] & \mathcal{D} \arrow[l, bend left, "R"]
\end{tikzcd}
\end{document}
```

> Sources: 7 Sketches §3.4.2 (Definition 3.70, Examples 3.71–3.74, Exercise 3.73), §3.4.3; DaoFP Chapter 10 (§10.1–10.11), §15 (monads from adjunctions); Kittenlab (implicitly: Lecture 5 discrete/codiscrete, Lecture 7 free monoid unit). Preorder case: [[Galois Connection]] ("Galois connections and adjunctions between the corresponding categories are exactly the same thing", Example 3.71: hom-sets with one or zero elements).

## Examples

- **Currying** (Example 3.72, DaoFP §10.1): $(- \times B) \dashv (-)^B$ on $\mathbf{Set}$, $\mathbf{Set}(A \times B, C) \cong \mathbf{Set}(A, C^B)$; "if you give me just $a$, I'll return a function $B \to C$ waiting for the $B$ input". Defines the [[Exponential Object]] and [[Cartesian Closed Category|cartesian closed categories]]; $\mathbf{Cat}$ is one too, with $[\mathcal{D}, \mathcal{E}]$ ([[7S Exercise 3.73]]: on morphisms $f \times B$ and $f^B = (- \mathbin{;} f)$; currying $+$ gives $p(3) = (n \mapsto n + 3)$).
- **Sum and product** (DaoFP §10.2): $(+) \dashv \Delta \dashv (\times)$ with the [[Diagonal Functor]]; more generally $\mathrm{Colim} \dashv \Delta \dashv \mathrm{Lim}$ (§10.4).
- **Free/forgetful** (Example 3.74, DaoFP §10.9): free [[Group|group]], [[Monoid|monoid]], ring, vector space $\dashv$ underlying set; [[Free Category|free category]] and free preorder on a graph $\dashv$ underlying graph; [[Discrete Category|discrete]] $\dashv$ underlying $\dashv$ [[Codiscrete Category|codiscrete]] (preorders, graphs, categories, topological spaces); abelianization $\dashv$ inclusion $\mathbf{Ab} \hookrightarrow \mathbf{Grp}$; [[Preorder Reflection]] $\dashv$ inclusion. See [[Free-Forgetful Adjunction]].
- **Data migration** (§3.4.3): $\Sigma_F \dashv \Delta_F \dashv \Pi_F$ ([[Data Migration Functor]]); [[Kan Extension|Kan extensions]] generalize.
- [[Dependent Sum|$\Sigma_f$]] $\dashv f^* \dashv$ [[Dependent Product|$\Pi_f$]] (DaoFP Ch. 11), [[Direct Image, Preimage, and Dual Image|$\exists_f \dashv f^* \dashv \forall_f$]] for subsets; [[Monoidal Closed Category|$(- \otimes a) \dashj [a, -]$]].
- Every adjunction gives a [[Monad]] $RL$ and a [[Comonad]] $LR$ (DaoFP Ch. 15–16); every monad arises this way ([[Eilenberg-Moore Category]], [[Kleisli Category]]).

## Equivalent formulations (DaoFP §10.5–10.6)

- **Unit and counit**: $\eta : \mathrm{Id} \Rightarrow RL$ with $\eta_c := \alpha^{-1}(\mathrm{id}_{Lc})$, and $\varepsilon : LR \Rightarrow \mathrm{Id}$ with $\varepsilon_d := \alpha(\mathrm{id}_{Rd})$ (the Yoneda trick), satisfying the **triangle identities** $(\varepsilon \circ L) \cdot (L \circ \eta) = \mathrm{id}_L$ and $(R \circ \varepsilon) \cdot (\eta \circ R) = \mathrm{id}_R$. Conversely such $\eta, \varepsilon$ give the hom-set bijection: $f : c \to Rd \mapsto \varepsilon_d \circ Lf$ and $g : Lc \to d \mapsto Rg \circ \eta_c$ ([[DaoFP Exercise 10.5.2]]). This definition works in any [[2-Category]]. See [[Unit and Counit of an Adjunction]].
- **Universal arrows**: $L \dashv R$ iff for every $d$ there is a terminal object $(Rd, \varepsilon_d)$ in the [[Comma Category]] $L \downarrow d$ — a [[Universal Arrow]] from $L$ to $d$; dually initial objects $(Lc, \eta_c)$ in $c \downarrow R$. An adjunction is a "half-equivalence": if $\eta, \varepsilon$ are isomorphisms it is an [[Equivalence of Categories]].

## Properties

- [[Right Adjoints Preserve Limits]] and left adjoints preserve colimits (DaoFP §10.7; preorder version [[Right Adjoints Preserve Meets]]). Hence e.g. distributivity $(b + c) \times a \cong b \times a + c \times a$, since $(- \times a)$ is a left adjoint.
- Adjoints are unique up to natural isomorphism ([[7S Exercise 1.110]] for preorders).
- Adjunctions compose: $L' \dashv R'$ and $L \dashj R$ give $(L' \circ L) \dashv (R \circ R')$; categories and adjunctions form $\mathbf{Adj}(\mathbf{Cat})$ (DaoFP §10.10).
- Existence: the [[Adjoint Functor Theorem]] (Freyd) — a limit-preserving functor from a complete category with a solution set has a left adjoint; preorder case [[Adjoint Functor Theorem for Preorders]]; programming instance: [[Defunctionalization]].
- $Lx$ [[Representable Functor|represents]] the co-presheaf $y \mapsto \mathcal{C}(x, Ry)$ and $Ry$ represents the presheaf $x \mapsto \mathcal{D}(Lx, y)$ ([[DaoFP Exercise 10.3.2]], [[DaoFP Exercise 10.3.3]]).

"Universal constructions are one of the most important themes of category theory: one gives some specified shape and says 'find me the best solution!'; category theory asks 'approximate from the left or the right?'" (7 Sketches §3.6). "A sculptor subtracts irrelevant stone until a sculpture emerges" (DaoFP Ch. 10).

````tabs
tab: Julia
```julia
# Catlab: adjunctions appear as Σ ⊣ Δ ⊣ Π data migrations and as free/forgetful constructions
using Catlab
@present SchDDS(FreeSchema) begin State::Ob; next::Hom(State, State) end
@acset_type DDS(SchDDS)
F = FinFunctor(Dict(:V => :State, :E => :State), Dict(:src => id(SchDDS[:State]), :tgt => :next),
               FinCat(SchGraph), FinCat(SchDDS))
Δ = DeltaMigration(F); Σ = SigmaMigrationFunctor(F, Graph, DDS)
# the adjunction Σ ⊣ Δ:  Hom(Σ G, I) ≅ Hom(G, Δ I) — homomorphisms(Σ(G), I) vs homomorphisms(G, migrate(Graph, I, Δ))

# Kittenlab-style Galois connection check on preorders (thin adjunction): see [[Galois Connection]]
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
