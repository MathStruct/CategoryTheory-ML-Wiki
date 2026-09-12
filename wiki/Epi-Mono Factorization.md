#theorem #definition

In $\mathbf{Set}$ (and in any [[Topos]] or regular category) every morphism $f : A \to B$ factors as an [[Epimorphism]] followed by a [[Monomorphism]]:
$$A \twoheadrightarrow \mathrm{im}(f) \hookrightarrow B, \qquad \mathrm{im}(f) := \{f(a) \mid a \in A\},$$
and this factorization is unique up to unique isomorphism. The **image** $\mathrm{im}(f)$ is a [[Subobject]] of $B$ and a [[Quotient Set|quotient]] of $A$.

> Sources: 7 Sketches §1.4.2 (pulling back partitions "by taking the epi-mono factorization"); Kittenlab Lecture 14 (direct image); DaoFP §2.4–2.5.

Uses: the right adjoint $g^*$ in [[Pushforward and Pullback of Partitions]] composes a surjection with $g$ and takes the epi part; the [[Direct Image, Preimage, and Dual Image|direct image]] $f_!(A')$ is the image of the restriction of $f$ to $A'$; in a [[Topos]] the image gives the existential quantifier $\exists_f$ ([[Quantification]]).

````tabs
tab: Julia
```julia
using Catlab
f = FinFunction([2, 2, 3], 4)
e, m = epi_mono(f)          # e: FinSet(3) ↠ FinSet(2), m: FinSet(2) ↪ FinSet(4)
compose(e, m) == f          # true
```
tab: Lean
```lean
#check CategoryTheory.Limits.image        -- image f, with `factorThruImage f` (epi in nice categories) and `image.ι` (mono)
#check CategoryTheory.StrongEpiMonoFactorisation
```
tab: Haskell
```haskell
import Data.List (nub)
-- image of a function on a finite domain, and the epi/mono parts
epiMono :: Eq b => [a] -> (a -> b) -> ([b], a -> Int, Int -> b)
epiMono as f = (img, \a -> length (takeWhile (/= f a) img), (img !!))
  where img = nub (map f as)
```
````
