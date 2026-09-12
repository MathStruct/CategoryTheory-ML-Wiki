#definition #example

Let $\mathcal{V} = (V, \leq, I, \otimes)$ be a [[Symmetric Monoidal Preorder]]. A **$\mathcal{V}$-category** $\mathcal{X}$ consists of

(i) a set $\mathrm{Ob}(\mathcal{X})$ of **objects**;
(ii) for every two objects $x, y$, an element $\mathcal{X}(x, y) \in V$, the **hom-object** ("hom" is short for *homomorphism*, an important jargon word; "mapping object" would be more descriptive),

such that

(a) $I \leq \mathcal{X}(x, x)$ for every object $x$ (**identity**), and
(b) $\mathcal{X}(x, y) \otimes \mathcal{X}(y, z) \leq \mathcal{X}(x, z)$ for all $x, y, z$ (**composition**).

$\mathcal{V}$ is the **base of enrichment**; $\mathcal{X}$ is **enriched in** $\mathcal{V}$. A $\mathcal{V}$-category with a finite set of objects can be displayed as a square **matrix** of hom-objects. The definition makes sense without symmetry, but symmetry is needed e.g. for [[Product of Enriched Categories|products]].

> Sources: 7 Sketches §2.3 (Definition 2.46, Examples 2.47, 2.54, Theorem 2.49, Definition 2.53), §2.4, §4.4.4 (Definition 4.44: enrichment in a symmetric monoidal *category*), Remark 2.89; DaoFP Chapter 20; Kittenlab (implicitly: $\mathbf{Set}$-categories).

## Examples

| base $\mathcal{V}$ | $\mathcal{V}$-category | hom-object $\mathcal{X}(x,y)$ |
|---|---|---|
| [[Bool (Monoidal Preorder)|$\mathbf{Bool}$]] | [[Preorder]] ([[Preorders are Bool-Categories]]) | is $x \leq y$? |
| [[Cost]] | [[Lawvere Metric Space]] | distance $d(x, y) \in [0, \infty]$ |
| $\mathbf{NMY}$ | points with a no/maybe/yes answer to "can I get from $x$ to $y$?" | [[7S Exercise 2.61]] |
| $(\mathcal{P}(M), \subseteq, M, \cap)$ | modes of transport that get you from $x$ to $y$ | [[7S Exercise 2.62]] |
| $(\mathbb{N} \cup \{\infty\}, \leq, \infty, \min)$ | weight limits on routes | [[7S Exercise 2.63]] |
| $(\mathbf{Set}, \times, 1)$ | ordinary (locally small) [[Category]] | the hom-*set* |
| $(\mathbf{Cat}, \times, \mathbf{1})$ | a (strict) [[2-Category]] | the hom-category (DaoFP §20.1) |
| any closed $\mathcal{V}$ | $\mathcal{V}$ itself (self-enrichment, Remark 2.89) | $v \multimap w$ |

**Example 2.47.** The preorder $p \leq q, r \leq s \leq t$ (with $q, r$ incomparable) as a $\mathbf{Bool}$-category has matrix with $\mathcal{X}(x,y) = \mathsf{true}$ iff $x \leq y$: e.g. $\mathcal{X}(s,t) = \mathsf{true}$, $\mathcal{X}(t,s) = \mathsf{false}$.

## Enrichment in a monoidal category (DaoFP Ch. 20, 7 Sketches §4.4.4)

Replace the preorder $\mathcal{V}$ by a [[Symmetric Monoidal Category]] $(\mathcal{V}, \otimes, I, \alpha, \lambda, \rho)$. A $\mathcal{V}$-category $\mathcal{C}$ has hom-*objects* $\mathcal{C}(a, b) \in \mathcal{V}$, **composition** morphisms $\circ : \mathcal{C}(b, c) \otimes \mathcal{C}(a, b) \to \mathcal{C}(a, c)$ and **identity** morphisms $j_a : I \to \mathcal{C}(a, a)$ in $\mathcal{V}$, making the associativity pentagon (using $\alpha$) and unit triangles (using $\lambda$, $\rho$) commute — all diagrams in $\mathcal{V}$, "so we still fall back on set theory, but at a different level". Ordinary categories are enriched in $(\mathbf{Set}, \times, 1)$, with composition "defined in bulk" as a function between hom-sets and identity as $j_a : 1 \to \mathcal{C}(a, a)$. The two inequalities (a), (b) above are exactly this data when $\mathcal{V}$ is thin.

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}[column sep=small]
(\mathcal{C}(c,d) \otimes \mathcal{C}(b,c)) \otimes \mathcal{C}(a,b) \arrow[rr, "\alpha"] \arrow[d, "\circ \otimes \mathrm{id}"'] & & \mathcal{C}(c,d) \otimes (\mathcal{C}(b,c) \otimes \mathcal{C}(a,b)) \arrow[d, "\mathrm{id} \otimes \circ"] \\
\mathcal{C}(b,d) \otimes \mathcal{C}(a,b) \arrow[dr, "\circ"'] & & \mathcal{C}(c,d) \otimes \mathcal{C}(a,c) \arrow[dl, "\circ"] \\
 & \mathcal{C}(a,d) &
\end{tikzcd}
\end{document}
```

DaoFP's motivations: category theory "reluctantly draws upon set theory" through hom-sets; enrichment replaces structureless hom-sets by objects whose richness lives in the morphisms of $\mathcal{V}$ ("having fewer morphisms often means having more structure"). "Enrichment doesn't always mean adding more stuff" — enriching in the walking arrow $\mathbb{B}$ *impoverishes* to preorders. Every $\mathcal{V}$-category has an **underlying ordinary category** $\mathcal{C}_0$ whose hom-sets are the global elements $\mathcal{V}(I, \mathcal{C}(a, b))$ ([[DaoFP Exercise 20.1.2]]). Any [[Monoidal Closed Category|monoidal closed category]] is **self-enriched** via internal homs $[a, b]$, with composition built from the evaluation counit $\varepsilon$ and identity from $\lambda$; this is why a Haskell `Functor` (whose `fmap :: (a -> b) -> (f a -> f b)` acts on *internal* homs) is really an [[Enriched Functor]], and why every Haskell functor is [[Functorial Strength|strong]].

## Constructions

[[Change of Base]] along a [[Monoidal Monotone Map]] / monoidal functor; [[Enriched Functor|$\mathcal{V}$-functors]]; [[Enriched Natural Transformation|$\mathcal{V}$-natural transformations]]; the [[Opposite Enriched Category|opposite]] $\mathcal{X}^{\mathrm{op}}(x, y) := \mathcal{X}(y, x)$, [[Dagger Preorder|dagger]] and skeletal $\mathcal{V}$-categories ([[7S Exercise 2.73]]); [[Product of Enriched Categories|products]] $\mathcal{X} \times \mathcal{Y}$; $\mathcal{V}$-[[Profunctor|profunctors]] (Chapter 4); presentation by [[Weighted Graph|$\mathcal{V}$-weighted graphs]] computed by [[Matrix Multiplication in a Quantale]] when $\mathcal{V}$ is a [[Quantale]]; generalized [[Hausdorff Distance]]; and in DaoFP, enriched [[Yoneda Lemma]], [[Weighted Limit|weighted limits]], enriched [[End|ends]] and [[Kan Extension|Kan extensions]]. The authoritative reference is Kelly [Kel05].

````tabs
tab: Julia
```julia
# a V-category with finitely many objects as a matrix of hom-objects over a monoidal preorder V
struct VCategory{T, V<:Preorder{T}}
  base::V
  objects::Vector{Symbol}
  hom::Matrix{T}            # hom[i, j] = X(objects[i], objects[j])
end

function is_vcategory(X::VCategory)
  V, n = X.base, length(X.objects)
  ident = all(leq(V, munit(V), X.hom[i, i]) for i in 1:n)
  comp  = all(leq(V, otimes(V, X.hom[i, j], X.hom[j, k]), X.hom[i, k]) for i in 1:n, j in 1:n, k in 1:n)
  ident && comp
end

# Example 2.47 as a Bool-category
X = VCategory(BoolPre(), [:p, :q, :r, :s, :t], Bool[
  1 1 1 1 1;
  0 1 0 1 1;
  0 0 1 1 1;
  0 0 0 1 1;
  0 0 0 0 1])
is_vcategory(X)   # true
```
tab: Lean
```lean
-- Mathlib: categories enriched in a monoidal category V
#check CategoryTheory.EnrichedCategory
-- class EnrichedCategory (V) [MonoidalCategory V] (C : Type) where
--   Hom : C → C → V
--   id (X) : 𝟙_ V ⟶ Hom X X
--   comp (X Y Z) : Hom X Y ⊗ Hom Y Z ⟶ Hom X Z
--   + assoc, id_comp, comp_id
#check CategoryTheory.EnrichedCategory.Hom
-- self-enrichment of a monoidal closed category:
#check CategoryTheory.MonoidalClosed
```
tab: Haskell
```haskell
-- a V-category on a finite object set, V a monoidal preorder, given by a hom table
data VCat v o = VCat { objects :: [o], hom :: o -> o -> v }

isVCat :: (MonoidalPreorder v, Eq o) => VCat v o -> Bool
isVCat (VCat os h) =
  and [ leq mempty (h x x) | x <- os ] &&
  and [ leq (h x y <> h y z) (h x z) | x <- os, y <- os, z <- os ]

-- Haskell's own categories are enriched in Hask: the internal hom is (->)
class EnrichedFunctorHask f where
  fmapE :: (a -> b) -> (f a -> f b)     -- acts on hom-*objects*, i.e. this is Functor
```
````
