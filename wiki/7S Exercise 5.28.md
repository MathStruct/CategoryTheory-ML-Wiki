#exercise #solution #proof

**Exercise 5.28.** Show the [[Free Prop]] on the signature with one generator $\rho_{m,n}$ of every arity is the prop $\mathbf{PG}$ of [[Port Graph|port graphs]].

## Solution

Both have objects $\mathbb{N}$. A morphism of $\mathrm{Free}(G)$ is a $G$-labeled port graph; since $G$ has exactly one generator of each arity, the labeling $\ell$ is forced ($\ell(v) = \rho_{\mathrm{in}(v), \mathrm{out}(v)}$) and contributes nothing, so morphisms are exactly port graphs; composition and monoidal product are by definition those of $\mathbf{PG}$.
