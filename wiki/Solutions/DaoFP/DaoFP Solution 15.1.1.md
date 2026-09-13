#solution #program

**Solution to [[DaoFP Exercise 15.1.1|Exercise 15.1.1]].**

The second identity is $\varepsilon \circ L \,\cdot\, L \circ \eta = \mathrm{id}_L$: reading bottom-up, $L \xrightarrow{L \eta} L R L \xrightarrow{\varepsilon L} L$. As a string diagram: an $L$-string that dips into a cup ($\eta$, creating $R L$ to its right) and then rises through a cap ($\varepsilon$, annihilating $L R$) — a zigzag that pulls straight to the plain $L$-string. When $L$ is a Haskell endofunctor: `triangle2 :: forall x. L x -> L x; triangle2 = counit . fmap unit`, which must equal `id` (here `fmap` is $L$'s, instantiating $L \circ \eta$; `counit` at `L x` is $\varepsilon \circ L$).

> Sources: DaoFP Exercise 15.1.1.
