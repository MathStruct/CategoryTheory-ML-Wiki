#definition #theorem #proof #example

**Construction 2.64.** Let $f : \mathcal{V} \to \mathcal{W}$ be a [[Monoidal Monotone Map]]. Given a $\mathcal{V}$-[[Enriched Category|category]] $\mathcal{C}$, the associated $\mathcal{W}$-category $\mathcal{C}_f$ has

(i) $\mathrm{Ob}(\mathcal{C}_f) := \mathrm{Ob}(\mathcal{C})$;
(ii) $\mathcal{C}_f(c, d) := f(\mathcal{C}(c, d))$.

> Sources: 7 Sketches §2.4.1, Construction 2.64, Example 2.65, Exercises 2.67, 2.68; DaoFP §20 (change of enriching category via a monoidal functor).

*Proof that $\mathcal{C}_f$ is a $\mathcal{W}$-category.* (a) $I_{\mathcal{W}} \leq f(I_{\mathcal{V}}) \leq f(\mathcal{C}(c,c)) = \mathcal{C}_f(c,c)$, using that $f$ is monoidal monotone and $\mathcal{C}$ a $\mathcal{V}$-category. (b) $\mathcal{C}_f(c,d) \otimes_{\mathcal{W}} \mathcal{C}_f(d,e) = f(\mathcal{C}(c,d)) \otimes_{\mathcal{W}} f(\mathcal{C}(d,e)) \leq f(\mathcal{C}(c,d) \otimes_{\mathcal{V}} \mathcal{C}(d,e)) \leq f(\mathcal{C}(c,e)) = \mathcal{C}_f(c,e)$. $\blacksquare$

**Example 2.65.** $f : [0, \infty] \to \mathbb{B}$, $f(x) = \mathsf{true}$ iff $x = 0$, is a monoidal monotone $\mathbf{Cost} \to \mathbf{Bool}$ (cf. [[7S Chapter 2 Exercises#Exercise 2.44|7S Exercise 2.44]]), so it converts [[Lawvere Metric Space|Lawvere metric spaces]] into [[Preorder|preorders]]: $x \leq y$ iff $d(x, y) = 0$. On the regions Boston, US, Spain this is the "is a part of" preorder: $\mathrm{Boston} \leq \mathrm{US}$ ([[7S Chapter 2 Exercises#Exercise 2.67|7S Exercise 2.67]]). The other monoidal monotone $u(x) = [x < \infty]$ gives the reachability preorder; the two disagree on the two-point space with $d(A,B) = d(B,A) = 5$ ([[7S Chapter 2 Exercises#Exercise 2.68|7S Exercise 2.68]]). Conversely $\mathbf{Bool} \to \mathbf{Cost}$ ($\mathsf{true} \mapsto 0$, $\mathsf{false} \mapsto \infty$) turns a preorder into a metric space with distances $0$ and $\infty$.

Change of base is a functor $\mathcal{V}\text{-}\mathbf{Cat} \to \mathcal{W}\text{-}\mathbf{Cat}$; in the categorical setting (DaoFP, Kelly) a lax [[Monoidal Functor]] induces it, and the underlying ordinary category $\mathcal{C}_0$ of a $\mathcal{V}$-category is change of base along $\mathcal{V}(I, -) : \mathcal{V} \to \mathbf{Set}$.

````tabs
tab: Julia
```julia
change_of_base(f, W, X::VCategory) = VCategory(W, X.objects, map(f, X.hom))

D = VCategory(CostPre(), [:Boston, :US, :Spain], [0.0 0 6000; 4000 0 6000; 7000 7000 0])
P = change_of_base(x -> x == 0, BoolPre(), D)    # the "is a part of" preorder
is_vcategory(P)   # true
```
tab: Lean
```lean
-- Mathlib: change of enriching category along a lax monoidal functor
#check CategoryTheory.TransportEnrichment   -- TransportEnrichment F C for F : LaxMonoidalFunctor V W
```
tab: Haskell
```haskell
changeOfBase :: (v -> w) -> VCat v o -> VCat w o
changeOfBase f (VCat os h) = VCat os (\x y -> f (h x y))

costToBool :: Cost -> All          -- "is the distance zero?"
costToBool (Fin 0) = All True
costToBool _       = All False
```
````
