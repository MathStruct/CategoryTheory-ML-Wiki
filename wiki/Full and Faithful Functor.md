#definition #example #theorem

A [[Functor]] $F : \mathcal C \to \mathcal D$ induces, for every pair of objects, a function on hom-sets

$$
F_{c, c'} : \mathcal C(c, c') \to \mathcal D(Fc, Fc').
$$

$F$ is **faithful** if each $F_{c,c'}$ is [[Injection|injective]], **full** if each is [[Surjection|surjective]], and **fully faithful** if each is a bijection. A fully faithful functor is an *embedding up to objects*: $\mathcal C$ looks exactly like the full image of $F$ inside $\mathcal D$, except that different objects may be sent to isomorphic (or even equal) ones.

> Sources: CTfS Definition 4.3.4.12, Exercises 4.3.4.13–4.3.4.14, Proposition 4.3.4.15, Exercise 4.3.4.16, Theorems 4.2.1.3, 4.2.1.6, Proposition 4.2.1.17, §4.6.3 (full subcategories); 7 Sketches Remark 3.60; DaoFP §5 (the [[Yoneda Embedding]]).

## Examples

- **Full subcategories.** The inclusion of a full [[Subcategory]] is fully faithful: $\mathbf{Fin} \subseteq \mathbf{Set}$, $\mathbf{Grp} \subseteq \mathbf{Mon}$, $\Delta \subseteq \mathbf{FLin}$ ([[Simplex Category]]), partial orders $\subseteq$ preorders (CTfS §4.6.3). A non-full subcategory, such as sets with injections inside $\mathbf{Set}$, is faithful but not full.
- **Monoids, groups, preorders are categories.** The functors $\mathbf{Mon} \to \mathbf{Cat}$, $\mathbf{Grp} \to \mathbf{Cat}$ and $\mathbf{PrO} \to \mathbf{Cat}$ are fully faithful (CTfS Theorems 4.2.1.3, 4.2.1.6, Proposition 4.2.1.17): a functor between one-object categories *is* a monoid homomorphism, and a functor between thin categories *is* a monotone map. This is what licenses saying "a monoid is a one-object category".
- **Forgetful functors** $\mathbf{Mon} \to \mathbf{Set}$, $\mathbf{Grph} \to \mathbf{Set} \times \mathbf{Set}$ are faithful (a homomorphism is determined by its underlying function(s)) but not full (not every function is a homomorphism).
- **$\mathbf 2 \to \mathbf 1$** ([[CTfS Chapter 4 Exercises#Exercise 4.3.4.13|CTfS Exercise 4.3.4.13]]): from the discrete category on two objects $a, b$ to the terminal category. Faithful — every hom-set has at most one element — but *not* full: $\mathcal C(a, b) = \varnothing$ cannot surject onto $\mathbf 1(\ast, \ast) = \{\mathrm{id}\}$.
- **$\mathbf 0 \to \mathcal C$** ([[CTfS Chapter 4 Exercises#Exercise 4.3.4.14|CTfS Exercise 4.3.4.14]]): vacuously fully faithful, but an equivalence only when $\mathcal C$ is empty.
- **$\mathbb Z/2 \to \mathbf 1$** ([[CTfS Chapter 4 Exercises#Exercise 4.3.4.16|CTfS Exercise 4.3.4.16]]): full but not faithful — two morphisms collapse to one — hence not an equivalence.
- **The [[Yoneda Embedding]]** $\mathcal C \to \mathbf{Set}^{\mathcal C^{\mathrm{op}}}$ is fully faithful — that is the [[Yoneda Lemma]].

## Equivalences

**Proposition (CTfS 4.3.4.15).** An [[Equivalence of Categories]] is fully faithful.

*Proof sketch.* Let $G$ be a quasi-inverse with $\alpha : GF \cong \mathrm{id}_{\mathcal C}$. If $Ff = Ff'$ then $GFf = GFf'$, and by naturality $f = \alpha_{c'} \circ GFf \circ \alpha_c^{-1} = f'$, so $F$ is faithful; symmetrically $G$ is faithful. For fullness, given $g : Fc \to Fc'$ put $f := \alpha_{c'} \circ Gg \circ \alpha_c^{-1}$; then $GFf = Gg$ and faithfulness of $G$ gives $Ff = g$. $\blacksquare$

Conversely, a fully faithful functor that is **essentially surjective** (every $d \in \mathcal D$ is isomorphic to some $Fc$) is an equivalence (assuming choice) — this is how $\mathrm{Skel}(\mathcal C) \simeq \mathcal C$ is proved ([[Skeleton]]). Fully faithful functors reflect isomorphisms, and the [[Adjunction|right adjoint]] of an adjunction is fully faithful iff the counit is an isomorphism (a *reflective* subcategory, like preorders in categories: 7 Sketches' reachability preorder of a graph).

````tabs
tab: Julia
**Docs:** [FinCats](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.FinCats)
```julia
using Catlab
# Check fullness/faithfulness of a map on finite hom-sets: F_{c,c'} as a function
faithful(Fhom) = allunique(Fhom)
full(Fhom, target_size) = length(unique(Fhom)) == target_size
# CTfS Exercise 4.3.4.16: ℤ/2 → 1 sends both morphisms {id, σ} to id
faithful([:id, :id]), full([:id, :id], 1)                         # (false, true)
# CTfS Exercise 4.3.4.13: 2 → 1 on Hom(a, b) = ∅ → {id}
faithful(Symbol[]), full(Symbol[], 1)                              # (true, false)
# The forgetful functor Mon → Set is faithful but not full: not every function
# ℤ/2 → ℤ/2 is a homomorphism of (ℤ/2, +)
is_hom(f) = f[1] == 0 && all(f[mod(a + b, 2) + 1] == mod(f[a + 1] + f[b + 1], 2) for a in 0:1, b in 0:1)
count(is_hom, [[x, y] for x in 0:1, y in 0:1]), 2^2               # (2, 4): 2 of 4 functions
```
tab: Lean
```lean
import Mathlib
open CategoryTheory
#check @Functor.Full                     -- surjective on hom-sets
#check @Functor.Faithful                 -- injective on hom-sets
#check @Functor.FullyFaithful            -- bijective, with a chosen preimage
#check @Functor.IsEquivalence            -- full, faithful and essentially surjective
#check @Functor.EssSurj
#check @yoneda                            -- the Yoneda embedding is fully faithful
example {C : Type*} [Category C] : (yoneda : C ⥤ _).Full := inferInstance
```
tab: Haskell
```haskell
import Data.List (nub)

-- a functor's action on one finite hom-set, as a list of images
faithful :: Eq b => [b] -> Bool
faithful imgs = length (nub imgs) == length imgs

full :: Eq b => [b] -> [b] -> Bool          -- images cover the target hom-set
full imgs target = all (`elem` imgs) target

-- Z/2 → 1:  faithful ["id","id"] == False,  full ["id","id"] ["id"] == True
-- 2 → 1 on Hom(a,b) = ∅:  faithful [] == True,  full [] ["id"] == False
```
````
