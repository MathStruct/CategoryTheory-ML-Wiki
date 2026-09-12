#exercise #solution #proof

**Exercise 4.4.4.** Show that functoriality of the sum preserves composition: for $g : b \to b'$ and $g' : b' \to b''$, applying $g' \circ g$ equals applying $g$ (to get $a + b \to a + b'$) and then $g'$ ([[Sum Type]], [[Functor]]).

## Solution

The lifted arrows are $\langle \mathrm{id}, g \rangle = [\mathsf{Left}, \mathsf{Right} \circ g]$ and $\langle \mathrm{id}, g' \rangle = [\mathsf{Left}, \mathsf{Right} \circ g']$. Their composite, precomposed with the injections, gives $\mathsf{Left} \mapsto \mathsf{Left}$ and $\mathsf{Right} \mapsto \mathsf{Right} \circ g' \circ g$; so does $\langle \mathrm{id}, g' \circ g \rangle$. By uniqueness of the copairing they are equal. In Haskell: `bimap id g' . bimap id g = bimap id (g' . g)`, checked on both constructors.

> Sources: DaoFP Exercise 4.4.4.
