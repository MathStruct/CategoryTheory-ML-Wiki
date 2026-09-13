#definition

The **diagonal functor** $\Delta : \mathcal{C} \to \mathcal{C} \times \mathcal{C}$ sends $x \mapsto (x, x)$ and $f \mapsto (f, f)$. Since $\mathcal{C} \times \mathcal{C} \cong [\mathbf{2}, \mathcal{C}]$, it is the same as the [[Constant Functor]] construction $x \mapsto \Delta_x$ (DaoFP §10.2: uncurrying $\mathcal{C} \to [\mathbf{2}, \mathcal{C}]$ gives $\mathcal{C} \times \mathbf{2} \to \mathcal{C}$ ignoring the second argument).

> Sources: DaoFP §10.2 ("The diagonal functor", "The sum adjunction", "The product adjunction"), §10.4.

The [[Coproduct]] and [[Product]] functors are its adjoints:

$$
(+) \dashv \Delta \dashv (\times),
$$

i.e. $\mathcal{C}(a + b, x) \cong (\mathcal{C} \times \mathcal{C})((a,b), \Delta x)$ and $(\mathcal{C} \times \mathcal{C})(\Delta x, (a, b)) \cong \mathcal{C}(x, a \times b)$ — "we could impress it in clay, in a modern version of cuneiform". The [[Unit and Counit of an Adjunction|unit]] of the sum adjunction is the pair of injections and the counit of the product adjunction the pair of projections ([[DaoFP Exercise 10.5.1]]). For a general indexing category, $\mathrm{Colim} \dashv \Delta \dashv \mathrm{Lim}$ ([[Limit]], [[Colimit]]). In a preorder, $\Delta$ is the map $x \mapsto (x, x)$ and its adjoints are [[Join]] and [[Meet]].

````tabs
tab: Lean
```lean
#check CategoryTheory.Functor.diag    -- diag C : C ⥤ C × C
#check CategoryTheory.Limits.colimConstAdj   -- colim ⊣ const
#check CategoryTheory.Limits.constLimAdj     -- const ⊣ lim
```
tab: Haskell
```haskell
diag :: a -> (a, a)
diag x = (x, x)
-- (+) ⊣ Δ:  (Either a b -> x)  ≅  (a -> x, b -> x)      (`either`)
-- Δ ⊣ (×):  (x -> a, x -> b)  ≅  (x -> (a, b))          (`(&&&)` / `fanout`)
```
````
