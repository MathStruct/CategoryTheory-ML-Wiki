#definition #example

A **bialgebra** in a [[Symmetric Monoidal Category]] is an object carrying both a [[Monoid Object|monoid]] $(\mu, \eta)$ and a comonoid $(\delta, \epsilon)$ structure that interact by the *bialgebra laws*: $\delta \circ \mu = (\mu \otimes \mu) \circ (\mathrm{id} \otimes \sigma \otimes \mathrm{id}) \circ (\delta \otimes \delta)$, $\epsilon \circ \mu = \epsilon \otimes \epsilon$, $\delta \circ \eta = \eta \otimes \eta$, $\epsilon \circ \eta = \mathrm{id}_I$ (the comultiplication is a monoid homomorphism). A **Hopf algebra** is a bialgebra with an *antipode* $s : H \to H$ satisfying $\mu \circ (s \otimes \mathrm{id}) \circ \delta = \eta \circ \epsilon = \mu \circ (\mathrm{id} \otimes s) \circ \delta$ — the analogue of inverses in a group.

> Sources: 7 Sketches §5.3.3 (Theorem 5.60: the equations of [[Graphical Linear Algebra]] include the bialgebra laws for the black/white pairs; the antipode is the scalar $-1$), §5.4.2, §6.3.1 (contrast: [[Frobenius Monoid|Frobenius]] laws); DaoFP §5.3, §16.2 (comonoids).

- Contrast with a [[Frobenius Monoid]]: in a Frobenius structure the monoid and comonoid satisfy $\delta \circ \mu = (\mu \otimes \mathrm{id}) \circ (\mathrm{id} \otimes \delta)$ (they "commute past each other"), whereas in a bialgebra they *distribute*. In the [[Prop of Matrices|prop $\mathbf{Mat}(R)$]], each colour of copy/add nodes is Frobenius on its own, and the two colours together form a bialgebra (7 Sketches Theorem 5.60); with the $-1$ scalar as antipode, $\mathrm{Mat}(\mathbb{Z})$ is a Hopf algebra — "graphical linear algebra".
- Classical examples: the group algebra $k[G]$ with $\delta(g) = g \otimes g$, $s(g) = g^{-1}$; the universal enveloping algebra of a Lie algebra; the algebra of functions on a finite group.

````tabs
tab: Julia
```julia
using Catlab
# the bialgebra law inside the free hypergraph/symmetric monoidal theory of Mat(ℤ) is the
# "copy then add" = "add then copy" equation; here we check it on actual matrices:
copy_(v) = vcat(v, v); add_(v) = v[1:end÷2] .+ v[end÷2+1:end]
v = [1, 2]; w = [3, 4]
copy_(add_(vcat(v, w))) == let (v1, v2, w1, w2) = (v, v, w, w); vcat(add_(vcat(v1, w1)), add_(vcat(v2, w2))) end   # true
```
tab: Lean
```lean
import Mathlib
#check @Bialgebra           -- class Bialgebra R A
#check @HopfAlgebra         -- with antipode
#check @MonoidAlgebra       -- the group algebra k[G], a Hopf algebra
```
tab: Haskell
```haskell
-- bialgebra structure on Int-vectors: copy and add distribute
copy :: [Int] -> ([Int], [Int])
copy v = (v, v)
add :: ([Int], [Int]) -> [Int]
add (v, w) = zipWith (+) v w
-- copy (add (v, w)) == (add (v, w), add (v, w)) == let (v1,v2)=copy v; (w1,w2)=copy w in (add (v1,w1), add (v2,w2))
```
````
