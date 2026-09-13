#solution #proof

**Solution to [[DaoFP Exercise 13.2.3|Exercise 13.2.3]].**

$(\varnothing, \mathrm{id}_\varnothing)$ is an $\mathrm{Id}$-algebra. For any algebra $(a, \alpha : a \to a)$ the unique function $¡ : \varnothing \to a$ satisfies $¡ \circ \mathrm{id} = \alpha \circ ¡$ (both sides are the empty function), so it is an algebra morphism, and it is the only one. Dually, $(1, \mathrm{id}_1)$ is a coalgebra and for any $(a, \alpha)$ the unique $! : a \to 1$ satisfies $\mathrm{id}_1 \circ ! = ! \circ \alpha$ (both are the unique map $a \to 1$), so it is the unique coalgebra morphism. This is the "impedance mismatch": $\mu \mathrm{Id} = \varnothing \subsetneq 1 = \nu \mathrm{Id}$.

> Sources: DaoFP Exercise 13.2.3.
