#solution #proof

**Solution to [[DaoFP Exercise 13.2.1|Exercise 13.2.1]].**

$(F t, F \tau)$ is a coalgebra, so terminality gives a unique coalgebra morphism $h : F t \to t$, i.e. $\tau \circ h = F h \circ F \tau$. Pasting this square with the (trivially commuting) square of $\tau$ shows $h \circ \tau : t \to t$ is a coalgebra morphism $(t, \tau) \to (t, \tau)$; so is $\mathrm{id}_t$; by uniqueness $h \circ \tau = \mathrm{id}_t$. Then $\tau \circ h = F h \circ F \tau = F(h \circ \tau) = \mathrm{id}_{F t}$. Hence $\tau^{-1} = h$ and $F t \cong t$.

> Sources: DaoFP Exercise 13.2.1.
