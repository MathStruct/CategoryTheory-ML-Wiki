#definition #theorem #example #program

"All concepts are Kan extensions" (Mac Lane). Given $F : \mathcal{E} \to \mathcal{C}$ and $P : \mathcal{E} \to \mathcal{B}$ (a possibly lossy, non-surjective "squishing" of $\mathcal{E}$ into $\mathcal{B}$), a **Kan extension** extends $F$ along $P$ to all of $\mathcal{B}$. Equality $K \circ P = F$ is too much to ask, even a natural iso; one settles for a one-way natural transformation, whose direction distinguishes right from left:

- **Right Kan extension** $(\mathrm{Ran}_P F, \varepsilon)$: $\varepsilon : \mathrm{Ran}_P F \circ P \to F$ universal — for every $(G, \alpha : G \circ P \to F)$ there is a unique $\sigma : G \to \mathrm{Ran}_P F$ with $\alpha = \varepsilon \cdot (\sigma \circ P)$. If it exists for all $F$: $(- \circ P) \dashv \mathrm{Ran}_P$, i.e. $[\mathcal{E}, \mathcal{C}](G \circ P, F) \cong [\mathcal{B}, \mathcal{C}](G, \mathrm{Ran}_P F)$.
- **Left Kan extension** $(\mathrm{Lan}_P F, \eta)$: $\eta : F \to \mathrm{Lan}_P F \circ P$ universal — for every $(G, \alpha : F \to G \circ P)$ a unique $\sigma : \mathrm{Lan}_P F \to G$ with $\alpha = (\sigma \circ P) \cdot \eta$; $\mathrm{Lan}_P \dashv (- \circ P)$.

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}
\mathcal{E} \arrow[r, "F"] \arrow[d, "P"'] & \mathcal{C} \\
\mathcal{B} \arrow[ur, "\mathrm{Ran}_P F\text{ or }\mathrm{Lan}_P F"', dashed] &
\end{tikzcd}
\end{document}
```

> Sources: DaoFP Chapter 19 ("Kan Extensions": §19.2 "Inverting a functor", §19.3 "Right Kan extension" incl. "as an end", "in Haskell", "Limits as Kan extensions", "Left adjoint as a right Kan extension", "Codensity monad"; §19.4 "Left Kan extension" incl. "as a coend", "Colimits as Kan extensions", "Right adjoint as a left Kan extension", "Day convolution as a Kan extension"; §19.5 "Useful Formulas"), Exercises 19.3.1–19.4.2; 7 Sketches §3.4 ([[Data Migration Functor|$\Sigma_F$, $\Pi_F$]] are $\mathrm{Lan}_F$, $\mathrm{Ran}_F$), §3.6.

## Intuition: fractions

[[Adjunction|Adjoints]] behave like inverses; Kan extensions like *fractions* $F / P$: undo $P$ (modulo $F$) and follow with $F$. If $P$ has a left adjoint $P^{-1}$ then $\mathrm{Ran}_P F \cong F \circ P^{-1}$; if a right adjoint, $\mathrm{Lan}_P F \cong F \circ P^{-1}$. The more $F$ discards, the easier the inversion.

## Formulas ([[End]]/[[Coend]], §19.5)

$$(\mathrm{Ran}_P F)\, b \cong \int_e \mathcal{B}(b, P e) \pitchfork F e \qquad (\mathrm{Lan}_P F)\, b \cong \int^e \mathcal{B}(P e, b) \cdot F e$$
generalizing the [[Ninja Yoneda Lemma|ninja (co-)Yoneda lemmas]] ($P = \mathrm{Id}$). Here $A \pitchfork c$ is the **power** ($\mathcal{C}(b, A \pitchfork c) \cong \mathbf{Set}(A, \mathcal{C}(b, c))$, "multiply $A$ copies of $c$": $\mathbf{2} \pitchfork c = c \times c$) and $A \cdot b$ the **copower** ($\mathcal{C}(A \cdot b, c) \cong \mathbf{Set}(A, \mathcal{C}(b, c))$, $\mathbf{2} \cdot b = b + b$); in $\mathbf{Set}$ both decay to the exponential/product, giving `Ran p f b = forall e. (b -> p e) -> f e` and `Lan p f b = exists e. (p e -> b, f e)`. The proofs write themselves: pull ends out of hom-sets by continuity, apply the (co)power definition, integrate with Yoneda.

## Everything is a Kan extension

| concept | as Kan extension |
|---|---|
| [[Limit]] of $D : \mathcal{J} \to \mathcal{C}$ | $\lim D = \mathrm{Ran}_{!} D$ along $! : \mathcal{J} \to \mathbf{1}$ (a cone is $\gamma : X \circ ! \to D$) |
| [[Colimit]] | $\mathrm{colim}\, D = \mathrm{Lan}_{!} D$ |
| left adjoint of $R$ | $L \cong \mathrm{Ran}_R \mathrm{Id}$ (with $\sigma = (\alpha \circ L) \cdot (G \circ \eta)$, [[DaoFP Exercise 19.3.2]]); conversely $\mathrm{Ran}_R \mathrm{Id}$ is a left adjoint iff preserved by $R$ |
| right adjoint of $L$ | $R \cong \mathrm{Lan}_L \mathrm{Id}$ |
| [[Codensity Monad]] | $T^F = \mathrm{Ran}_F F$ ($F/F$); the *density comonad* is $\mathrm{Lan}_F F$ |
| [[Day Convolution]] | $F \star G \cong \mathrm{Lan}_\otimes (F \bar\otimes G)$ for the external product $(F \bar\otimes G)\langle a, b\rangle = F a \times G b$ |
| [[Data Migration Functor|data migration]] | $\Sigma_F = \mathrm{Lan}_F$, $\Pi_F = \mathrm{Ran}_F$ on [[C-Set|C-sets]] |
| [[Dependent Sum]] / [[Dependent Product]] | $\Sigma_f, \Pi_f$ along a function of sets (discrete categories) |

## In Haskell

```haskell
newtype Ran p f b = Ran (forall e. (b -> p e) -> f e)
counit :: Ran p f (p e') -> f e'                        -- ε: instantiate at e = e' with id
counit (Ran h) = h id
type Alpha p f g = forall e. g (p e) -> f e             -- α : G ∘ P → F
sigma :: Functor g => Alpha p f g -> forall b. g b -> Ran p f b
sigma alpha gb = Ran (\b_pe -> alpha (fmap b_pe gb))

data Lan p f b where Lan :: (p e -> b) -> f e -> Lan p f b
unit :: f e' -> Lan p f (p e')                          -- η: pick e = e', id
unit fe = Lan id fe
sigmaL :: Functor g => (forall e. f e -> g (p e)) -> forall b. Lan p f b -> g b
sigmaL alpha (Lan pe_b fe) = fmap pe_b (alpha fe)
```

````tabs
tab: Julia
```julia
using Catlab
# Kan extensions of C-sets along a schema functor: Σ_F = Lan_F, Π_F = Ran_F (data migration)
@present SchDDS(FreeSchema) begin State::Ob; next::Hom(State, State) end
@acset_type DDS(SchDDS)
F = FinFunctor(Dict(:V => :State, :E => :State),
               Dict(:src => id(SchDDS[:State]), :tgt => :next), FinCat(SchGraph), FinCat(SchDDS))
G = cycle_graph(Graph, 3)                          # (a path graph would generate an infinite DDS)
Σ = SigmaMigrationFunctor(F, Graph, DDS)          # left Kan extension along F
D = Σ(G); nparts(D, :State), D[:next]              # (3, [2, 3, 1]): the freely generated DDS
```
tab: Lean
```lean
import Mathlib
open CategoryTheory
#check @CategoryTheory.Functor.lan              -- left Kan extension functor along P
#check @CategoryTheory.Functor.ran
#check @CategoryTheory.Functor.lanAdjunction    -- lan ⊣ (whiskeringLeft P)
#check @CategoryTheory.Functor.ranAdjunction
#check @CategoryTheory.Functor.LeftExtension    -- (Lan_P F, η) as a universal left extension
#check @CategoryTheory.Functor.LeftExtension.IsPointwiseLeftKanExtension   -- the colimit (coend) formula
```
tab: Haskell
```haskell
{-# LANGUAGE RankNTypes, GADTs #-}
newtype Ran p f b = Ran (forall e. (b -> p e) -> f e)
instance Functor (Ran p f) where                        -- Exercise 19.3.1
  fmap g (Ran h) = Ran (\k -> h (k . g))

data Lan p f b where
  Lan :: (p e -> b) -> f e -> Lan p f b
instance Functor (Lan p f) where                        -- Exercise 19.4.1
  fmap g (Lan pe_b fe) = Lan (g . pe_b) fe

-- limits as right Kan extensions along ! : J → 1, e.g. products: Ran along the functor from 2
```
````
