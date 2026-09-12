#exercise #solution #proof

**Exercise 1.77.** Show that $\Phi$ ("is $\bullet$ connected to $\ast$?") is a [[Monotone Map]] $\mathrm{Prt}(\{\ast, \bullet, \circ\}) \to \mathbb{B}$.

## Solution

Let $P \leq Q$ be partitions, i.e. $P$ is finer: $x \sim_P y$ implies $x \sim_Q y$. If $\Phi(P) = \mathsf{true}$ then $\bullet \sim_P \ast$, hence $\bullet \sim_Q \ast$, so $\Phi(Q) = \mathsf{true}$. Thus $\Phi(P) \leq \Phi(Q)$. It nonetheless has a [[Generative Effect]].
