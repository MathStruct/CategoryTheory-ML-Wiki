#definition #theorem #example #program

The **dependent sum** (sigma type) $\Sigma_{x : B}\, T(x)$ is the type of pairs $(x, y)$ with $x : B$ and $y : T(x)$ — a sum *tagged* by elements of $B$; e.g. counted vectors are pairs `(2, (64, 7))`, `(5, (8,21,14,-1,0))` of a length and a tuple. Categorically, for $f : b \to a$ the dependent sum is the **left adjoint of the [[Base Change Functor]]**:

$$
\Sigma_f \dashv f^*, \qquad \Sigma_f \langle s, q : s \to b \rangle := \langle s, f \circ q \rangle : \mathcal{C}/b \to \mathcal{C}/a,
$$

also written $f_!$ ("$f$ lower shriek"): a bundle finely fibered over $b$ is automatically, more coarsely, fibered over $a$ by post-composition. The adjunction reads

$$
(\mathcal{C}/b)\big(\langle s, q \rangle,\ f^* \langle F, p \rangle\big) \cong (\mathcal{C}/a)\big(\langle s, f \circ q \rangle,\ \langle F, p \rangle\big).
$$

> Sources: DaoFP §11.3 ("Dependent Sum", "Adding the atlas", "Existential quantification"), §11.2; 7 Sketches §7.4.4 (existential quantification as image); Kittenlab Lecture 13.

- **Special case $a = 1$**: $f^*F = B \times F$, and the adjunction $(\mathcal{C}/B)(\langle S, q \rangle, \langle B \times F, \pi_1 \rangle) \cong \mathcal{C}(S, F)$ exhibits $S$ as the [[Coproduct|sum]] of its fibers: the left side is a bunch of arrows, one per fiber of $S$, into $F$ ("$B$ copies of $F$" generalize the [[Diagonal Functor]] $\Delta$). For counted vectors $\phi^T$ is infinitely many functions, one per length, defined in practice by recursion (`sumV`).
- **Logic**: $\Sigma_{x : B} T(x)$ is the proposition $\exists_{x : B}\, T(x)$: a term is a witness $x$ together with a proof $y : T(x)$ ([[Quantification]]).
- The forgetful functor $U : \mathcal{C}/B \to \mathcal{C}$, $\langle S, q \rangle \mapsto S$, is $\Sigma_!$ for $! : B \to 1$.
- In $\mathbf{Set}$, $\Sigma_{x : B} T(x) = \bigsqcup_{x \in B} T(x)$; in Haskell without full dependent types, `data SomeVec a = forall n. SomeVec (SNat n) (Vec n a)` (an existential) plays the role of $\Sigma_n \mathrm{Vec}\, n\, a$.

````tabs
tab: Julia
```julia
using Catlab
# Σ_f on a bundle q : S → B along f : B → A is just post-composition
q = FinFunction([1, 2, 2, 3], 3)        # S = 4 fibered over B = 3
f = FinFunction([1, 1, 2], 2)           # B → A
Σq = compose(q, f)                      # S fibered over A: fibers of size 3 and 1
[length(preimage(Σq, y)) for y in 1:2]  # [3, 1]
```
tab: Lean
```lean
import Mathlib
#check @Sigma                            -- Σ x : B, T x  with ⟨x, y⟩
#check @Sigma.mk
#check @CategoryTheory.Over.map           -- Σ_f : Over X ⥤ Over Y for f : X ⟶ Y (post-composition)
#check @CategoryTheory.Over.mapPullbackAdj  -- Over.map f ⊣ Over.pullback f
example : Σ n : ℕ, Fin n := ⟨3, 1⟩      -- a dependent pair
```
tab: Haskell
```haskell
{-# LANGUAGE DataKinds, GADTs, ExistentialQuantification #-}
-- the sum over n of Vec n a, as an existential
data SomeVec a = forall n. SomeVec (SNat n) (Vec n a)
data SNat n where
  SZ :: SNat 'Z
  SS :: SNat n -> SNat ('S n)
-- a mapping out of the dependent sum: one function per fiber, defined by recursion
sumV :: Vec n Int -> Int
sumV VNil = 0
sumV (VCons n v) = n + sumV v
```
````
