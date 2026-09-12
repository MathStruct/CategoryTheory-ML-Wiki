#definition #example #proof

Let $\mathcal{V}$ be a [[Symmetric Monoidal Preorder]] and $\mathcal{X}, \mathcal{Y}$ be $\mathcal{V}$-[[Enriched Category|categories]]. Their **$\mathcal{V}$-product** $\mathcal{X} \times \mathcal{Y}$ has

(i) $\mathrm{Ob}(\mathcal{X} \times \mathcal{Y}) := \mathrm{Ob}(\mathcal{X}) \times \mathrm{Ob}(\mathcal{Y})$;
(ii) $(\mathcal{X} \times \mathcal{Y})((x, y), (x', y')) := \mathcal{X}(x, x') \otimes \mathcal{Y}(y, y')$.

> Sources: 7 Sketches Definition 2.74, Example 2.76, Exercises 2.75, 2.78; DaoFP §20.1 ("tensor product of $\mathcal{V}$-categories"); ordinary case: [[Product Category]], preorder case: [[Product Preorder]].

*It is a $\mathcal{V}$-category* ([[7S Exercise 2.75]]): $I = I \otimes I \leq \mathcal{X}(x,x) \otimes \mathcal{Y}(y,y)$; and
$$\mathcal{X}(x_1,x_2) \otimes \mathcal{Y}(y_1,y_2) \otimes \mathcal{X}(x_2,x_3) \otimes \mathcal{Y}(y_2,y_3) \cong \mathcal{X}(x_1,x_2) \otimes \mathcal{X}(x_2,x_3) \otimes \mathcal{Y}(y_1,y_2) \otimes \mathcal{Y}(y_2,y_3) \leq \mathcal{X}(x_1,x_3) \otimes \mathcal{Y}(y_1,y_3),$$
where **symmetry** is used exactly to swap the middle two factors — which is why $\mathcal{V}$ must be symmetric (DaoFP makes the same point for the tensor product of $\mathcal{V}$-categories, whose identity is $j_c \otimes j_d : I \otimes I \to \mathcal{C}(c,c) \otimes \mathcal{D}(d,d)$).

**Examples.** For $\mathcal{V} = \mathbf{Bool}$ the AND in "$(p_1, q_1) \leq (p_2, q_2)$ iff $p_1 \leq p_2$ and $q_1 \leq q_2$" is $\otimes$: the [[Product Preorder]]. For $\mathcal{V} = \mathbf{Cost}$ distances add: $d_{X \times Y}((x,y),(x',y')) = d_X(x,x') + d_Y(y,y')$ (Example 2.76: the product of the path $A \xrightarrow{2} B \xrightarrow{3} C$ with $p \rightleftarrows q$ (weights $5, 8$) is a $2 \times 3$ grid; [[7S Exercise 2.78]]: in $\mathbb{R} \times \mathbb{R}$, $d((5,6),(-1,4)) = 6 + 2 = 8$). In matrix terms the product is the **Kronecker product** of hom-matrices with $\otimes$ in place of multiplication — a block matrix with one $X$-shaped block, shifted by an entry of $Y$, for every entry of $Y$.

````tabs
tab: Julia
```julia
function vproduct(X::VCategory, Y::VCategory)
  V = X.base
  objs = [Symbol(x, "_", y) for y in Y.objects for x in X.objects]
  hom = [otimes(V, X.hom[i, i′], Y.hom[j, j′])
         for (j, i) in Iterators.product(eachindex(Y.objects), eachindex(X.objects)) |> collect |> vec,
             (j′, i′) in Iterators.product(eachindex(Y.objects), eachindex(X.objects)) |> collect |> vec]
  VCategory(V, objs, hom)
end
# For Cost this is `kron`-like with + in place of *: kron(X, Y)[(i,j),(i′,j′)] = X[i,i′] + Y[j,j′]
```
tab: Lean
```lean
-- Mathlib has products of ordinary categories (`CategoryTheory.prod`) and of preorders;
-- the enriched tensor product of V-categories is the componentwise ⊗ on Hom (by hand).
```
tab: Haskell
```haskell
vproduct :: MonoidalPreorder v => VCat v o -> VCat v o' -> VCat v (o, o')
vproduct (VCat os h) (VCat os' h') =
  VCat [ (x, y) | x <- os, y <- os' ] (\(x, y) (x', y') -> h x x' <> h' y y')
```
````
