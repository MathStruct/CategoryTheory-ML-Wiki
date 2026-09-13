#definition #example

Let $\Phi : \mathcal{X} \nrightarrow \mathcal{Y}$ be a $\mathcal{V}$-[[Profunctor]]. Its **collage** $\mathrm{Col}(\Phi)$ (DaoFP: **cograph**) is the $\mathcal{V}$-[[Enriched Category|category]] with $\mathrm{Ob}(\mathrm{Col}(\Phi)) := \mathrm{Ob}(\mathcal{X}) \sqcup \mathrm{Ob}(\mathcal{Y})$ and

$$
\mathrm{Col}(\Phi)(a, b) := \begin{cases} \mathcal{X}(a, b) & a, b \in \mathcal{X} \\ \Phi(a, b) & a \in \mathcal{X},\ b \in \mathcal{Y} \\ \varnothing\ (= \bigvee\varnothing) & a \in \mathcal{Y},\ b \in \mathcal{X} \\ \mathcal{Y}(a, b) & a, b \in \mathcal{Y}, \end{cases}
$$

with **collage inclusions** $i_{\mathcal{X}} : \mathcal{X} \to \mathrm{Col}(\Phi)$, $i_{\mathcal{Y}} : \mathcal{Y} \to \mathrm{Col}(\Phi)$. Its Hasse diagram is the union of the two Hasse diagrams plus the bridges as arrows — "put a box around the whole picture and see a new preorder".

> Sources: 7 Sketches §4.3.3 (Definition 4.42, Example 4.43, Exercise 4.44), Example 4.11; DaoFP §17.1 ("Collages"), Exercises 17.1.1–17.1.2.

**Example 4.43** ($\mathbf{Cost}$): $\mathcal{X} = A \xrightarrow{2} B$, $\mathcal{Y} = x \rightleftarrows y$ (weights $3, 4$), bridge $A \xrightarrow{5} x$. Collage matrix (rows/cols $A, B, x, y$): $A$: $0, 2, 5, 8$; $B$: $\infty, 0, \infty, \infty$; $x$: $\infty, \infty, 0, 3$; $y$: $\infty, \infty, 4, 0$ (7 Sketches prints the empty hom-object as $0 = \bigvee\varnothing$, which in $\mathbf{Cost}$ is $\infty$).

DaoFP: the new morphisms across the collage are **heteromorphisms**, going only from $\mathcal{C}$ to $\mathcal{D}$; composition with them is by lifting along the profunctor. A profunctor $\mathcal{C}^{\mathrm{op}} \times \mathcal{C} \to \mathbf{Set}$ "should really be called an endo-profunctor" — it defines a collage of $\mathcal{C}$ with itself. There is a functor from any collage to the [[Walking Arrow]] ([[DaoFP Chapter 17 Exercises#Exercise 17.1.1|DaoFP Exercise 17.1.1]]), and conversely any category with a functor to the walking arrow splits as a collage ([[DaoFP Chapter 17 Exercises#Exercise 17.1.2|DaoFP Exercise 17.1.2]]): profunctors $\mathcal{C} \nrightarrow \mathcal{D}$ are the same as categories over $\mathbf{2}$ with fibres $\mathcal{C}$ and $\mathcal{D}$.

````tabs
tab: Julia
```julia
# collage of a V-profunctor between finite V-categories: block matrix [X Φ; 0 Y]
function collage(P::VProfunctor)
  V = P.X.base; zero = join(V, [])                    # ⋁∅
  hom = [P.X.hom P.Φ; fill(zero, size(P.Φ, 2), size(P.Φ, 1)) P.Y.hom]
  VCategory(V, vcat(P.X.objects, P.Y.objects), hom)
end
# Example 4.43
X = VCategory(CostPre(), [:A, :B], [0.0 2; Inf 0]); Y = VCategory(CostPre(), [:x, :y], [0.0 3; 4 0])
Φ = VProfunctor(X, Y, [5.0 8; Inf Inf])
is_vcategory(collage(Φ))     # true
```
tab: Haskell
```haskell
-- the collage of two categories along a profunctor: objects are a sum, heteromorphisms are p a b
data ColObj x y = InX x | InY y
data ColHom p x y a b where
  HomX :: (a -> b) -> ColHom p x y (InX a) (InX b)   -- schematic: homs within X
  Het  :: p a b -> ColHom p x y (InX a) (InY b)      -- heteromorphisms from X to Y (never back)
```
````
