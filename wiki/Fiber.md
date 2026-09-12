#definition #example

For a [[Function]] $f : X \to Y$ and $y \in Y$, the **fiber** of $f$ over $y$ is the preimage $f^{-1}(y) = \{x \in X \mid f(x) = y\}$. A function is thus an *arrangement of $X$ over $Y$*: different $f$'s distribute the same elements into different fibers. Fibers are the [[Pullback|pullbacks]] of $f$ along the [[Global Element|global elements]] $y : 1 \to Y$; the fiber over a subset $U \subseteq Y$ is the pullback $f^{-1}(U)$.

> Sources: 7 Sketches §7.3.3 ("Extended example: sections of a function"), Eq. (7.37), Exercise 7.38; DaoFP §11.1 (a dependent type is a family of fibers, "a bundle"); Kittenlab Lecture 13 (typed sets).

- In Eq. (7.37) the fibers are $\{a_1, a_2\}$ over $a$, $\{b_1, b_2, b_3\}$ over $b$, $\{c_1\}$ over $c$, $\varnothing$ over $d$, $\{e_1, e_2\}$ over $e$ ([[7S Exercise 7.38]]).
- A *section* of $f$ over $U \subseteq Y$ picks one element of each fiber over $U$; these form the [[Sheaf of Sections]] $\mathrm{Sec}_f$.
- Fibers are the categorical form of a [[Dependent Type]] $x : Y \vdash f^{-1}(x)$ (DaoFP §11): $X = \sum_{y \in Y} f^{-1}(y)$ is the [[Dependent Sum]]. A [[Surjection]] has all fibers nonempty; an [[Injection]] has all fibers of size $\leq 1$; a [[Partition]] is the set of fibers of its classifying map.

````tabs
tab: Julia
```julia
using Catlab
f = FinFunction([1, 1, 2, 2, 2, 3, 5, 5], 5)    # Eq. (7.37): a,a,b,b,b,c,e,e
fiber(y) = preimage(f, y)
fiber.(1:5)                                     # [[1,2],[3,4,5],[6],[],[7,8]]
```
tab: Lean
```lean
import Mathlib
#check @Set.preimage           -- f ⁻¹' {y}
example (f : ℕ → ℕ) (y : ℕ) : Set ℕ := f ⁻¹' {y}
```
tab: Haskell
```haskell
fiber :: Eq y => [x] -> (x -> y) -> y -> [x]
fiber xs f y = [ x | x <- xs, f x == y ]
```
````
