#solution #proof

**Solution to [[DaoFP Exercise 4.4.4|Exercise 4.4.4]].**

The lifted arrows are $\langle \mathrm{id}, g \rangle = [\mathsf{Left}, \mathsf{Right} \circ g]$ and $\langle \mathrm{id}, g' \rangle = [\mathsf{Left}, \mathsf{Right} \circ g']$. Their composite, precomposed with the injections, gives $\mathsf{Left} \mapsto \mathsf{Left}$ and $\mathsf{Right} \mapsto \mathsf{Right} \circ g' \circ g$; so does $\langle \mathrm{id}, g' \circ g \rangle$. By uniqueness of the copairing they are equal. In Haskell: `bimap id g' . bimap id g = bimap id (g' . g)`, checked on both constructors.

> Sources: DaoFP Exercise 4.4.4.
