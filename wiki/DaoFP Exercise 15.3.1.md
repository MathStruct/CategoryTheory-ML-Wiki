#exercise #solution #proof

**Exercise 15.3.1.** Show that $U \circ F$ for the pointed-object adjunction is the [[Maybe Monad]].

## Solution

$F a = (1 + a, \mathsf{Left})$ and $U$ forgets the point, so $U F a = 1 + a = $ `Maybe a`. The unit $\eta_a : a \to 1 + a$ is $\mathsf{Right}$ (`Just`); the counit at a pointed object $(b, p)$ is the point-preserving map $[p, \mathrm{id}_b] : 1 + b \to b$; whiskering gives $\mu_a = [\mathsf{Left}, \mathrm{id}] : 1 + (1 + a) \to 1 + a$, i.e. `join Nothing = Nothing; join (Just m) = m` — exactly the Maybe monad. The adjunction: a point-preserving map $(1 + a, \mathsf{Left}) \to (b, p)$ is determined by its restriction to $a$, so $1/\mathcal{C}(F a, (b, p)) \cong \mathcal{C}(a, b)$.

> Sources: DaoFP Exercise 15.3.1.
