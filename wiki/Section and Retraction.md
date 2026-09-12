#definition #example

Given morphisms $f : A \to B$ and $g : B \to A$ with $f \mathbin{;} g = \mathrm{id}_A$ (i.e. $g \circ f = \mathrm{id}_A$) but not necessarily $g \mathbin{;} f = \mathrm{id}_B$, we call $f$ a **section** of $g$ and $g$ a **retraction** of $f$; $A$ is a **retract** of $B$. "$g$ and $f$ are almost — but not quite — inverses."

> Source: 7 Sketches Example 3.34 (functions $\underline{2} \rightleftarrows \underline{3}$); DaoFP §2.4–2.5.

**Example 3.34.** $f : \underline{2} \to \underline{3}$, $1 \mapsto 1, 2 \mapsto 2$ and $g : \underline{3} \to \underline{2}$, $1 \mapsto 1, 2 \mapsto 2, 3 \mapsto 2$: $f \mathbin{;} g = \mathrm{id}_2$ but $g \mathbin{;} f \neq \mathrm{id}_3$. A section is always a [[Monomorphism]] (split mono) and a retraction always an [[Epimorphism]] (split epi); a morphism that is both a section and a retraction of the same map is an [[Isomorphism]]. In $\mathbf{Set}$ every [[Surjection]] has a section (axiom of choice) and every injection out of a nonempty set has a retraction. The composite $g \mathbin{;} f : B \to B$ is idempotent, $(g\mathbin{;}f)\mathbin{;}(g\mathbin{;}f) = g\mathbin{;}f$ — a **split idempotent**, the categorical version of a [[Closure Operator]] together with its fixed points.

````tabs
tab: Lean
```lean
#check CategoryTheory.SplitMono   -- structure with retraction, id
#check CategoryTheory.SplitEpi    -- structure with section_, id
```
tab: Haskell
```haskell
-- a retract: embed then project is the identity on the smaller type
data Retract a b = Retract { embed :: a -> b, project :: b -> a }   -- project . embed = id
boolInInt :: Retract Bool Int
boolInInt = Retract (\b -> if b then 1 else 0) (/= 0)
```
````
