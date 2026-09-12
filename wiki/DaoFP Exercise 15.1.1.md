#exercise #solution #program

**Exercise 15.1.1.** Draw the [[String Diagram|string diagrams]] for the second triangle identity of an [[Adjunction]] and translate them to Haskell.

## Solution

The second identity is $\varepsilon \circ L \,\cdot\, L \circ \eta = \mathrm{id}_L$: reading bottom-up, $L \xrightarrow{L \eta} L R L \xrightarrow{\varepsilon L} L$. As a string diagram: an $L$-string that dips into a cup ($\eta$, creating $R L$ to its right) and then rises through a cap ($\varepsilon$, annihilating $L R$) — a zigzag that pulls straight to the plain $L$-string. When $L$ is a Haskell endofunctor: `triangle2 :: forall x. L x -> L x; triangle2 = counit . fmap unit`, which must equal `id` (here `fmap` is $L$'s, instantiating $L \circ \eta$; `counit` at `L x` is $\varepsilon \circ L$).

> Sources: DaoFP Exercise 15.1.1.
