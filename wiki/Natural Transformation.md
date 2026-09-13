#definition #example #theorem #proof #program

Let $F, G : \mathcal{C} \to \mathcal{D}$ be [[Functor|functors]]. A **natural transformation** $\alpha : F \Rightarrow G$ consists of, for each object $c \in \mathcal{C}$, a morphism $\alpha_c : F(c) \to G(c)$ in $\mathcal{D}$ (the **$c$-component**), such that for every $f : c \to d$ in $\mathcal{C}$ the **naturality square** commutes:

$$
F(f) \mathbin{;} \alpha_d = \alpha_c \mathbin{;} G(f), \qquad\text{i.e.}\qquad G(f) \circ \alpha_c = \alpha_d \circ F(f).
$$

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}
F(c) \arrow[r, "\alpha_c"] \arrow[d, "F(f)"'] & G(c) \arrow[d, "G(f)"] \\
F(d) \arrow[r, "\alpha_d"'] & G(d)
\end{tikzcd}
\end{document}
```

If every component is an [[Isomorphism]], $\alpha$ is a **natural isomorphism** ([[Natural Isomorphism]]). "A natural transformation maps objects to arrows, and arrows to commuting squares" (DaoFP). Kittenlab: a natural transformation "really lives in $\mathcal{D}$" — the standard picture of $\alpha$ floating between $F$ and $G$ hides this asymmetry.

> Sources: 7 Sketches Definition 3.49, Examples 3.52, 3.53, 3.57, Exercises 3.55, 3.58, 3.64, §3.3.5; Kittenlab Lecture 6 (graph homomorphisms as a preview), 7; DaoFP §3.2 ("Naturality"), Chapter 9 (§9.1–9.3), §20.3 (enriched version). Related: [[Functor Category]], [[Graph Homomorphism]], [[Yoneda Lemma]].

## Examples

- **Graph homomorphisms** (7 Sketches §3.3.5, Kittenlab Lectures 6–7): for graphs $G, H : \mathsf{Gr} \to \mathbf{Set}$, a natural transformation is a pair $\alpha_V : G(V) \to H(V)$, $\alpha_E : G(E) \to H(E)$ and naturality for $\mathrm{src}$ and $\mathrm{tgt}$ says exactly "alpha-then-source equals source-then-alpha": sources and targets are preserved. Likewise for [[Petri Net|Petri nets]] (arcs preserved) and any [[C-Set]] — "the naturality condition is very… natural". [[7S Exercise 3.64]] computes one by hand.
- Between sets viewed as functors $\underline{\mathbf{1}} \to \mathbf{Set}$, a natural transformation is just a function (Example 3.53).
- Example 3.52: given $F, G : (1 \xrightarrow{f} 2) \to \mathcal{D}$, the components $\alpha_1, \alpha_2$ must be chosen so the one square commutes; $\alpha$ "relates two views of $\mathcal{C}$ inside $\mathcal{D}$".
- **Preorders**: between [[Monotone Map|monotone maps]] $F, G : \mathbb{N} \to \mathbb{N}$ (non-decreasing sequences) a natural transformation exists iff $F_n \leq G_n$ for all $n$, and is unique; so $\mathbb{N}^{\mathbb{N}}$ is a preorder (Example 3.57; Kittenlab Lecture 7 for $\mathbb{R} \to \mathbb{R}$). Into a preorder there is at most one natural transformation between any two functors; *out of* a preorder there may be many ([[7S Exercise 3.58]]).
- **Groups**: between homomorphisms $f, g : G \to H$, a natural transformation is $h \in H$ with $f(x) = h\,g(x)\,h^{-1}$ — conjugation (Kittenlab Lecture 7).
- $\eta : 1_{\mathbf{Set}} \Rightarrow UF$, $x \mapsto [x]$, the singleton list into the [[Free Monoid]]; naturality says mapping $f$ over $[x]$ equals $[f(x)]$ (Kittenlab Lecture 7) — the unit of the [[Free-Forgetful Adjunction]] and of the [[List Monad]].
- **Programming** (DaoFP §9.3): a natural transformation between endofunctors of $\mathbf{Hask}$ is a *parametrically polymorphic* function `forall a. f a -> g a`, e.g. `safeHead :: [a] -> Maybe a`, `reverse :: [a] -> [a]`. Parametricity makes naturality automatic ("theorems for free"), so `fmap h . alpha = alpha . fmap h` can be used to transform programs. Intuition: `fmap` transforms the *contents* of a container, a natural transformation *repackages* contents into another container without inspecting them; naturality says the two operations commute. (Filtering is *not* natural: it inspects the data.)
- The [[Unit and Counit of an Adjunction|unit and counit]] $\eta : \mathrm{Id} \Rightarrow RL$, $\varepsilon : LR \Rightarrow \mathrm{Id}$ of an [[Adjunction]]; the multiplication of a [[Monad]]; [[Cone|cones]] $\Delta_x \Rightarrow D$ and [[Cocone|cocones]] $D \Rightarrow \Delta_x$ (DaoFP §9.4–9.5: [[Cospan|cospans]] $\mathbf{2} \to \mathcal{C}$ are natural transformations $D \Rightarrow \Delta_x$).

## Composition

- **Vertical composition** ([[7S Exercise 3.55]], DaoFP §9.3): for $\alpha : F \Rightarrow G$, $\beta : G \Rightarrow H$, define $(\alpha \mathbin{;} \beta)_c := \alpha_c \mathbin{;} \beta_c$ ("for each object $c$, compose the $c$-components"); naturality follows by pasting two squares. The identity $(\mathrm{id}_F)_c := \mathrm{id}_{F(c)}$. This makes the [[Functor Category]] $\mathcal{D}^{\mathcal{C}} = [\mathcal{C}, \mathcal{D}]$.
- **Horizontal composition** (DaoFP §9.3): for $\alpha : F \Rightarrow F'$ ($\mathcal{C} \to \mathcal{D}$) and $\beta : G \Rightarrow G'$ ($\mathcal{D} \to \mathcal{E}$), $(\beta \circ \alpha)_x := \beta_{F'x} \circ G(\alpha_x) = G'(\alpha_x) \circ \beta_{Fx}$, the two being equal by naturality of $\beta$; in Haskell `beta . fmap alpha = fmap alpha . beta`. **Whiskering** is horizontal composition with an identity: $(\beta \circ F)_x = \beta_{Fx}$ (just a change of type signature) and $(G \circ \alpha)_x = G(\alpha_x)$ (`fmap alpha`); $(H \circ \beta \circ F)_x = H(\beta_{Fx})$. The **interchange law** says vertical-then-horizontal equals horizontal-then-vertical. These make $\mathbf{Cat}$ a [[2-Category]].

## Enriched version (DaoFP §20.3)

In a $\mathcal{V}$-category there are no individual arrows, so a component is a "global element" $\nu_a : I \to \mathcal{D}(Fa, Ga)$, and naturality is a commuting hexagon built from $\lambda^{-1}, \rho^{-1}$, $\nu \otimes F_{ab}$, $G_{ab} \otimes \nu$ and composition — equivalently $\mathcal{D}(Fa, \nu_b) \circ F_{ab} = \mathcal{D}(\nu_a, Gb) \circ G_{ab}$. See [[Enriched Natural Transformation]].

````tabs
tab: Julia
```julia
# Kittenlab src/NaturalTransformations.jl
abstract type NaturalTransformation{C<:Category, D<:Category} end
# component(α::NaturalTransformation{C,D}, x::ObC)::HomD

struct FunctorCat{C<:Category, D<:Category} <: Category{Functor{C,D}, NaturalTransformation{C,D}}
  c::C; d::D
end
struct ComposedNT{C,D,A<:NaturalTransformation{C,D},B<:NaturalTransformation{C,D}} <: NaturalTransformation{C,D}
  cats::FunctorCat{C,D}; α::A; β::B
end
component(γ::ComposedNT, x) = compose(γ.cats.d, component(γ.α, x), component(γ.β, x))   # vertical composition
struct IdTransformation{C,D} <: NaturalTransformation{C,D}
  f::Functor{C,D}
end
Categories.id(::FunctorCat, f::Functor) = IdTransformation(f)

# Catlab: natural transformations between C-sets = ACSet transformations (graph homomorphisms)
using Catlab
G = @acset Graph begin V = 3; E = 2; src = [1, 2]; tgt = [2, 3] end          # Example 3.63
H = @acset Graph begin V = 2; E = 3; src = [1, 1, 2]; tgt = [2, 2, 2] end
α = ACSetTransformation(G, H; V = [1, 2, 2], E = [2, 3])                       # Exercise 3.64: a ↦ d, b ↦ e
is_natural(α)                       # true: the naturality squares for src and tgt commute
homomorphisms(G, H)                 # all graph homomorphisms, by search
```
tab: Lean
```lean
#check CategoryTheory.NatTrans      -- structure NatTrans F G: app, naturality; notation F ⟶ G in C ⥤ D
open CategoryTheory in
example {C D : Type} [Category C] [Category D] (F G : C ⥤ D) (α : F ⟶ G) {X Y : C} (f : X ⟶ Y) :
    F.map f ≫ α.app Y = α.app X ≫ G.map f := α.naturality f
#check @CategoryTheory.NatTrans.vcomp
#check @CategoryTheory.NatTrans.hcomp
#check CategoryTheory.NatIso        -- natural isomorphisms F ≅ G
```
tab: Haskell
```haskell
{-# LANGUAGE RankNTypes #-}
-- DaoFP §9.3: a natural transformation is a parametrically polymorphic function
type Natural f g = forall a. f a -> g a

safeHead :: Natural [] Maybe
safeHead []      = Nothing
safeHead (a : _) = Just a

-- naturality (free by parametricity):  fmap h . safeHead == safeHead . fmap h

-- vertical composition is function composition; horizontal composition:
hcomp :: (Functor g) => Natural g g' -> Natural f f' -> (forall x. g (f x) -> g' (f' x))
hcomp beta alpha = beta . fmap alpha
```
````
