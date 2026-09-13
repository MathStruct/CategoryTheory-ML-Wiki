#solution #proof

**Solution to [[7S Exercise 6.7|Exercise 6.7]].**

1. $f(1_R) = 1_S$ and $f(r_1 *_R r_2) = f(r_1) *_S f(r_2)$.
2. The [[Natural Numbers]] rig $(\mathbb{N}, 0, +, 1, *)$. Given any rig $R$, a homomorphism $f : \mathbb{N} \to R$ must send $0 \mapsto 0_R$, $1 \mapsto 1_R$, and by additivity

$$
f(m) = f(1 + \cdots + 1) = 1_R +_R \cdots +_R 1_R \quad (m \text{ summands}),
$$

so $f$ is determined. This formula also preserves multiplication: by distributivity, $(1_R + \cdots + 1_R)$ ($m$ times) $*_R$ $(1_R + \cdots + 1_R)$ ($n$ times) expands to the sum of $mn$ copies of $1_R$, which is $f(m * n)$. Hence there is exactly one rig homomorphism $\mathbb{N} \to R$, i.e. $\mathbb{N}$ is initial.

````tabs
tab: Lean
```lean
import Mathlib
-- ℕ is the initial semiring: the unique ring hom is the canonical cast.
#check @Nat.castRingHom            -- (R : Type) [NonAssocSemiring R] : ℕ →+* R
#check @RingHom.eq_natCast         -- every f : ℕ →+* R equals Nat.cast
```
tab: Haskell
```haskell
-- the unique rig homomorphism from Nat into any semiring
fromNat :: Num r => Integer -> r
fromNat 0 = 0
fromNat n = 1 + fromNat (n - 1)
```
````

> Sources: 7 Sketches, Exercise 6.7 and Solution A.6.
