#definition #example

A **metric space** $(X, d)$ consists of a [[Set]] $X$ of **points** and a function $d : X \times X \to \mathbb{R}_{\geq 0}$, the **distance**, such that for all $x, y, z$:

(a) $d(x, x) = 0$;
(b) if $d(x, y) = 0$ then $x = y$;
(c) $d(x, y) = d(y, x)$ (**symmetry**);
(d) $d(x, y) + d(y, z) \geq d(x, z)$ (**triangle inequality**).

Allowing $d : X \times X \to [0, \infty]$ gives an **extended metric space**.

> Sources: 7 Sketches Definition 2.51, Example 2.54, Exercises 2.52, 2.73; §2.3.3.

The triangle inequality says that a route $x \to z$ never costs more than going via an intermediate $y$; in a triangle with sides $3, 5, 7.2$ it can be invoked six ways ($3 + 5 \geq 7.2$, $5 + 7.2 \geq 3$, …). **Example 2.54.** $\mathbb{R}$ with $d(x, y) = |y - x|$.

Conditions (a) and (d) "wonderfully capture something about distance", but (b) and (c) are too restrictive:
- *effort* to travel in a hilly neighbourhood is asymmetric;
- *regions* (US, Spain, Boston) with $d(A, B) =$ "worst-case distance to get from somewhere in $A$ to anywhere in $B$" ($\sup_{a \in A} \inf_{b \in B} d(a, b)$, the asymmetric [[Hausdorff Distance]]): $d(\mathrm{US}, \mathrm{Spain}) > d(\mathrm{Spain}, \mathrm{US})$ ([[7S Exercise 2.52]]) and $d(\mathrm{Boston}, \mathrm{US}) = 0 \neq d(\mathrm{US}, \mathrm{Boston})$;
- infinite distances: "from here to Pluto is $\infty$".

Dropping (b), (c) and allowing $\infty$ yields the [[Lawvere Metric Space]] — a category enriched in [[Cost]]. Extended metric spaces are exactly the *skeletal dagger* $\mathbf{Cost}$-categories ([[7S Exercise 2.73]]), just as sets are skeletal dagger preorders: "preorders are to sets as Lawvere metric spaces are to extended metric spaces".

````tabs
tab: Lean
```lean
#check MetricSpace          -- dist : α → α → ℝ with dist_self, eq_of_dist_eq_zero, dist_comm, dist_triangle
#check EMetricSpace         -- extended: edist : α → α → ℝ≥0∞
#check PseudoEMetricSpace   -- drops (b): the symmetric Lawvere case
example : MetricSpace ℝ := inferInstance
example (x y : ℝ) : dist x y = |x - y| := Real.dist_eq x y
```
tab: Haskell
```haskell
-- a metric space as a distance function (laws unenforced)
newtype Metric a = Metric (a -> a -> Double)

realLine :: Metric Double
realLine = Metric (\x y -> abs (y - x))
```
````
