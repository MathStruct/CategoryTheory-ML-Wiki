#definition #example

For an object $c$ of $\mathcal{D}$, the **constant functor** $\Delta_c : \mathcal{J} \to \mathcal{D}$ sends every object of $\mathcal{J}$ to $c$ and every morphism to $\mathrm{id}_c$. In Haskell it is `Const c` (functorial in its *second* argument, [[DaoFP Chapter 8 Exercises#Exercise 8.3.3|DaoFP Exercise 8.3.3]]).

> Sources: DaoFP §8.2, §8.3, §9.4 ("Picking objects"), §10.2 ("The diagonal functor"); 7 Sketches Definition 3.92 (cones); Kittenlab Lecture 9; CTfS Exercise 4.3.1.10, §5.1.2

- Picking an object of $\mathcal{C}$ is the same as a functor $\underline{\mathbf{1}} \to \mathcal{C}$, or a constant functor from any $\mathcal{J}$; picking a pair, a functor from the discrete $\mathbf{2}$; picking an arrow, a functor from the [[Walking Arrow]].
- A [[Cone]] over a [[Diagram]] $D : \mathcal{J} \to \mathcal{C}$ with apex $x$ is a [[Natural Transformation]] $\Delta_x \Rightarrow D$; a [[Cocone]] is $D \Rightarrow \Delta_x$ (DaoFP §9.5, Kittenlab Lecture 9: "$\Delta : \mathcal{C} \to \mathcal{C}^{\mathsf{D}}$ sends $X$ to the constant functor at $X$"). Hence [[Limit|limits]] and [[Colimit|colimits]] are adjoints to $\Delta_{(-)} : \mathcal{C} \to [\mathcal{J}, \mathcal{C}]$: $\mathrm{Colim} \dashv \Delta \dashv \mathrm{Lim}$.
- A function $f : X \to Y$ gives a natural transformation between constant functors $\Delta_X \Rightarrow \Delta_Y : \mathcal{J} \to \mathbf{Set}$ whose every component is $f$; naturality squares commute trivially because the functors send all arrows to identities ([[CTfS Chapter 4 Exercises#Exercise 4.3.1.10|CTfS Exercise 4.3.1.10]]). So $\Delta : \mathcal{C} \to [\mathcal{J}, \mathcal{C}]$ is itself a functor, and CTfS §5.1.2 obtains limits and colimits of *all* $\mathcal{J}$-shaped diagrams at once as its adjoints $\Sigma_t \dashv \Delta_t \dashv \Pi_t$.
- The [[Diagonal Functor]] $\Delta : \mathcal{C} \to \mathcal{C} \times \mathcal{C}$ is the constant functor curried: $\mathcal{C} \times \mathcal{C} \cong [\mathbf{2}, \mathcal{C}]$, "which is why we use the same symbol $\Delta$ for both".
- A constant $\mathbf{Set}$-valued functor is the most lossy model of a category (DaoFP §9.6); it is [[Representable Functor|represented]] by the [[Initial Object]] when the constant is $1$ ([[DaoFP Chapter 9 Exercises#Exercise 9.8.4|DaoFP Exercise 9.8.4]]; Kittenlab Lecture 10: $\mathbb{R}^0$ represents the constant singleton functor on $\mathbf{Vect}$).

````tabs
tab: Julia
**Docs:** [FinCats](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.FinCats) · [Categories & functors](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.Categories) — Kittenlab [Lecture 9](https://algebraicjulia.github.io/Kittenlab.jl/lecture9.html), [Lecture 10](https://algebraicjulia.github.io/Kittenlab.jl/lecture10.html)

**Builds on:** [[Category]] (`Category`), [[Functor]] (`Functor`) — run those notes' Julia code first.
```julia
struct ConstFunctor{C<:Category, D<:Category, Ob, Hom} <: Functor{C, D}
  d::D; c::Ob
end
ob_map(F::ConstFunctor, _) = F.c
hom_map(F::ConstFunctor, _) = id(F.d, F.c)
```
tab: Lean
```lean
#check CategoryTheory.Functor.const     -- (const J).obj X : J ⥤ C, the constant functor at X
#check CategoryTheory.Limits.Cone       -- structure Cone F: pt, π : (const J).obj pt ⟶ F
```
tab: Haskell
```haskell
data Const c a = Const c
instance Functor (Const c) where
  fmap _ (Const c) = Const c
```
````
