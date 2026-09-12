#exercise #solution #proof

**Exercise 11.2.4.** Define the action of the [[Base Change Functor]] $f^*$ on a morphism $h : \langle e', p' \rangle \to \langle e, p \rangle$ of $\mathcal{C}/a$.

## Solution

Let $g' : f^* e' \to e'$ and $g : f^* e \to e$ be the pullback legs. The composite $g' \mathbin{;} h : f^* e' \to e$ and the projection $f^* p' : f^* e' \to b$ satisfy $p \circ (h \circ g') = p' \circ g' = f \circ f^* p'$, so they form a commuting square over $b \xrightarrow{f} a \xleftarrow{p} e$. By the universal property of the pullback $f^* e$ there is a unique $f^* h : f^* e' \to f^* e$ with $g \circ f^* h = h \circ g'$ and $f^* p \circ f^* h = f^* p'$ — the latter saying $f^* h$ is a morphism in $\mathcal{C}/b$. Uniqueness gives functoriality ($f^*(h \circ k) = f^* h \circ f^* k$, $f^* \mathrm{id} = \mathrm{id}$).

> Sources: DaoFP Exercise 11.2.4.
