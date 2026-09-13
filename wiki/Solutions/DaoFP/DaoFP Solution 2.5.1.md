#solution #proof

**Solution to [[DaoFP Exercise 2.5.1|Exercise 2.5.1]].**

Let $! : a \to 1$ and $g_1, g_2 : 1 \to c$ with $g_1 \circ ! = g_2 \circ !$. If $a$ has a [[Global Element]] $s : 1 \to a$ (so $! \circ s = \mathrm{id}_1$, i.e. $s$ is a [[Section and Retraction|section]] of $!$), then $g_1 = g_1 \circ ! \circ s = g_2 \circ ! \circ s = g_2$, so $!$ is epi. This is the intended reading (in $\mathbf{Set}$: every nonempty set surjects onto the point). Caveat: for $a = \varnothing$ the map $\varnothing \to 1$ is *not* epi in $\mathbf{Set}$, since the two maps $1 \to \underline{2}$ agree on $\varnothing$ — so the statement needs $a$ to have an element.

> Sources: DaoFP Exercise 2.5.1.
