#definition #example

Given a [[Metric Space]] $(X, d)$ and subsets $U, V \subseteq X$, the (asymmetric, "Lawvere") **Hausdorff distance** is
$$d_L(U, V) := \sup_{u \in U} \inf_{v \in V} d(u, v)$$
— "put me in the worst part of $U$; how far must I go to get anywhere in $V$?". The usual symmetric Hausdorff metric is $\max(d_L(U,V), d_L(V,U))$; 7 Sketches finds the unsymmetrized notion "more interesting". It makes the regions US, Spain, Boston a [[Lawvere Metric Space]]: $d(\mathrm{Boston}, \mathrm{US}) = 0$, $d(\mathrm{US}, \mathrm{Boston}) \neq 0$, and $d(\{r < 0\}, \{0\}) = \infty$.

> Sources: 7 Sketches §2.3.3 (footnote 3), Exercise 2.52, Remark 2.97.

**Generalization (Remark 2.97).** For any [[Quantale]] $\mathcal{V}$ and $\mathcal{V}$-category $\mathcal{X}$ with subsets $U, V$ of objects,
$$\mathcal{X}(U, V) := \bigwedge_{u \in U} \bigvee_{v \in V} \mathcal{X}(u, v).$$
For $\mathcal{V} = \mathbf{Bool}$ this asks "can I get into $V$ from every $u \in U$?", i.e. $\forall u \in U.\ \exists v \in V.\ u \leq v$; for $\mathcal{V} = \mathcal{P}(M)$ (modes of transportation) it gives the modes that get you into $V$ from every point of $U$. (In $\mathbf{Cost}$, $\bigvee = \inf$ and $\bigwedge = \sup$ because of the reversed order.)

````tabs
tab: Julia
```julia
# Lawvere-Hausdorff distance between index sets U, V of a finite metric matrix D
hausdorff(D, U, V) = maximum(minimum(D[u, v] for v in V) for u in U)
D = [0.0 4 3; 3 0 6; 7 4 0]
hausdorff(D, [1, 2], [3]), hausdorff(D, [3], [1, 2])   # (6.0, 4.0): asymmetric
```
tab: Lean
```lean
#check EMetric.hausdorffEdist     -- the symmetric extended Hausdorff distance in Mathlib
#check EMetric.infEdist           -- inf_{v ∈ V} edist u v, the inner part of d_L
```
tab: Haskell
```haskell
hausdorffL :: (a -> a -> Double) -> [a] -> [a] -> Double
hausdorffL d us vs = maximum [ minimum [ d u v | v <- vs ] | u <- us ]
```
````
