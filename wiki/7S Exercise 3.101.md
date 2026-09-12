#exercise #solution

**Exercise 3.101.** Define the opposite $F^{\mathrm{op}} : \mathcal{C}^{\mathrm{op}} \to \mathcal{D}^{\mathrm{op}}$ of a functor $F$.

## Solution

$F^{\mathrm{op}}(c) := F(c)$ on objects; a morphism $f : c_1 \to c_2$ in $\mathcal{C}^{\mathrm{op}}$ is $f' : c_2 \to c_1$ in $\mathcal{C}$, so set $F^{\mathrm{op}}(f) := F(f')^{\mathrm{op}} : F(c_1) \to F(c_2)$ in $\mathcal{D}^{\mathrm{op}}$. It preserves identities and composites. See [[Opposite Category]].
