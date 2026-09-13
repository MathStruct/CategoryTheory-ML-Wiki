#definition #example #theorem #proof #program

Let $\mathcal{C}$ and $\mathcal{D}$ be [[Category|categories]]. A **functor** $F : \mathcal{C} \to \mathcal{D}$ consists of

(i) for every object $c \in \mathrm{Ob}(\mathcal{C})$, an object $F(c) \in \mathrm{Ob}(\mathcal{D})$;
(ii) for every morphism $f : c_1 \to c_2$ in $\mathcal{C}$, a morphism $F(f) : F(c_1) \to F(c_2)$ in $\mathcal{D}$;

such that

(a) $F(\mathrm{id}_c) = \mathrm{id}_{F(c)}$ for every object $c$ (**preserves identities**);
(b) $F(f \mathbin{;} g) = F(f) \mathbin{;} F(g)$ for all composable $f, g$ (**preserves composition**).

"If categories distill the essence of structure, then functors are mappings that preserve this structure" (DaoFP). Kittenlab: "category theory is all about studying the objects of a category by studying the morphisms between them; so the study of functors — the morphisms between categories — is critical."

> Sources: 7 Sketches Definition 3.35, Examples 3.36, 3.38, 3.41, 3.42, Exercises 3.37, 3.39, 3.40, 3.43; Kittenlab Lecture 4 ("Functors"), 5, 6; DaoFP §8.2 ("Functors between categories"), §8.3 ("Functors in Programming"), §8.5; the enriched version is [[Enriched Functor]].

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}
a \arrow[d, "f"'] \arrow[r, maps to] & F(a) \arrow[d, "F(f)"] \\
b \arrow[r, maps to] & F(b)
\end{tikzcd}
\end{document}
```

## Examples

- Functors $\underline{\mathbf{2}} \to \underline{\mathbf{3}}$ are determined by their action on objects (Example 3.36, six of them); in general they are not ([[7S Chapter 3 Exercises#Exercise 3.40|7S Exercise 3.40]]: $\bullet \to \bullet$ into $\bullet \rightrightarrows \bullet$).
- Functors between [[Presentation of a Category|presented categories]] must respect equations (Example 3.41): none from the commutative square to the free square matching objects.
- Functors between [[Preorder|preorders]] are [[Monotone Map|monotone maps]] (Example 3.42, Kittenlab Lecture 5); between [[Monoid|monoids]], monoid homomorphisms.
- $F : \mathsf{Fin} \to \mathbf{Mat}$, $f \mapsto$ the 0/1 matrix with $1$ at $(i, f(i))$ (Kittenlab Lecture 4): identities go to identity matrices, and $(AB)_{ik} = \sum_j A_{ij} B_{jk}$ is nonzero exactly when $k = g(f(i))$.
- **Set-valued functors** $\mathcal{C} \to \mathbf{Set}$: [[Database Schema|database instances]] / [[C-Set|C-sets]] (7 Sketches §3.3.3, Kittenlab Lecture 6) — [[Graph|graphs]], [[Petri Net|Petri nets]], [[Port Graph|port graphs]] are all functors out of small [[Free Category|path categories]]. The [[Hom Functor|hom-functors]] $\mathcal{C}(a, -)$ ("the world according to $a$") and $\mathcal{C}(-, b)$ ("$b$ as seen by the world") are [[Representable Functor|representable]].
- [[Constant Functor]] $\Delta_c$; identity functor; [[Free Category|Free]] $: \mathbf{Grph} \to \mathbf{Cat}$; forgetful functors $U : \mathbf{Mon} \to \mathbf{Set}$ and free ones $F : \mathbf{Set} \to \mathbf{Mon}$ (Kittenlab Lecture 5/7; [[Free-Forgetful Adjunction]]); [[Preorder Reflection]]; [[Discrete Category|discrete]] and [[Codiscrete Category|codiscrete]] functors $\mathbf{Set} \to \mathbf{Preord}$; $\mathcal{P} : \mathbf{Set}^{\mathrm{op}} \to \mathbf{Pos}$ and $\mathcal{P} : \mathbf{Set} \to \mathbf{Pos}$ (Kittenlab Lecture 14); [[Data Migration Functor|data migration]] $\Delta_F, \Sigma_F, \Pi_F$.
- In programming (DaoFP §8.3): [[Endofunctor|endofunctors]] `Maybe`, `List` (type constructors with `fmap`), [[Bifunctor|bifunctors]] `(,)`, `Either`, [[Contravariant Functor|contravariant functors]] `Predicate`, [[Profunctor|profunctors]] `(->)`; "you can think of a data type as a container of values" and `fmap` transforms the contents without changing the shape.

## Properties

- A functor may merge objects and arrows (any category maps to the one-object category $\underline{\mathbf{1}}$) and need not be surjective (a functor from $\underline{\mathbf{1}}$ picks an object). Functors "produce simplified views" — models of $\mathcal{C}$ inside $\mathcal{D}$; a [[Natural Transformation]] compares two such models.
- **Composition** (Kittenlab Lecture 4, [[7S Chapter 3 Exercises#Exercise 3.43|7S Exercise 3.43]]): $(G \circ F)(x) = G(F(x))$, $(G \circ F)(f) = G(F(f))$ is a functor: $G(F(\mathrm{id}_x)) = G(\mathrm{id}_{F x}) = \mathrm{id}_{GFx}$ and $G(F(s \circ r)) = G(F s \circ F r) = GFs \circ GFr$. With identity functors this makes the [[Category of Categories]] $\mathbf{Cat}$ (Kittenlab's `KittenC`).
- Full, faithful, essentially surjective functors; [[Equivalence of Categories]]; [[Yoneda Embedding]] is fully faithful.
- A functor out of a [[Free Category]] is determined freely by its values on the generating graph (Kittenlab Lecture 6); a [[Diagram]] is a functor $\mathcal{J} \to \mathcal{C}$.
- Functors preserving [[Limit|limits]] are *continuous*, preserving [[Colimit|colimits]] *cocontinuous*; [[Right Adjoints Preserve Limits]].
- [[Monoidal Functor|Monoidal functors]] additionally respect $\otimes$; [[Functorial Strength|strength]] relates to [[Enriched Functor|enrichment]] (every Haskell `Functor` is strong).

````tabs
tab: Julia
```julia
# Kittenlab src/Functors.jl
abstract type Functor{C<:Category, D<:Category} end
# ob_map(F::Functor{C,D}, x::ObC)::ObD
# hom_map(F::Functor{C,D}, f::HomC)::HomD
# Laws: dom(d, hom_map(F,f)) == ob_map(F, dom(c,f)); codom likewise;
#       compose(d, hom_map(F,f), hom_map(F,g)) == hom_map(F, compose(c,f,g));  id(d, ob_map(F,x)) == hom_map(F, id(c,x))

# Kittenlab Lecture 4: Fin → Mat, a function f:{1..n}→{1..m} to an n×m 0/1 matrix
struct FinToMat <: Functor{FinSetC, MatC} end
ob_map(::FinToMat, n::Int) = n
function hom_map(::FinToMat, f::Int𝔽Mor)
  M = zeros(Int, f.dom.n, f.codom.n)
  for i in 1:f.dom.n; M[i, f(i)] = 1; end
  M
end

# Catlab: a functor between finitely presented categories, given by generator maps
using Catlab
@present SchDDS(FreeSchema) begin State::Ob; next::Hom(State, State) end
F = FinFunctor(Dict(:V => :State, :E => :State),
               Dict(:src => id(SchDDS[:State]), :tgt => :next),
               FinCat(SchGraph), FinCat(SchDDS))       # Gr → DDS from 7 Sketches §3.4.1
is_functorial(F)   # true
```
tab: Lean
```lean
#check CategoryTheory.Functor    -- structure: obj, map, map_id, map_comp; notation C ⥤ D
open CategoryTheory in
example {C D E : Type} [Category C] [Category D] [Category E] (F : C ⥤ D) (G : D ⥤ E) : C ⥤ E := F ⋙ G
open CategoryTheory in
#check (Functor.id : C ⥤ C)
-- monotone maps as functors between preorders
#check @Monotone.functor
```
tab: Haskell
```haskell
-- DaoFP §8.3: endofunctors of Hask
class Functor f where
  fmap :: (a -> b) -> (f a -> f b)
  -- laws: fmap id = id; fmap (g . f) = fmap g . fmap f

instance Functor Maybe where
  fmap _ Nothing  = Nothing
  fmap g (Just a) = Just (g a)

newtype Identity a = Identity a
instance Functor Identity where fmap g (Identity a) = Identity (g a)

data Const c a = Const c                       -- the constant functor Δ_c
instance Functor (Const c) where fmap _ (Const c) = Const c

newtype Compose g f a = Compose (g (f a))      -- functor composition
instance (Functor g, Functor f) => Functor (Compose g f) where
  fmap h (Compose gfa) = Compose (fmap (fmap h) gfa)
```
````
