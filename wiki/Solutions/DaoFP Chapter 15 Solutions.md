#solution

Solutions to the exercises of DaoFP, Chapter 15: [[DaoFP Chapter 15 Exercises]]. Index: [[Map of Content]].

## Solution 15.1.1

#program — [[DaoFP Chapter 15 Exercises#Exercise 15.1.1|Exercise 15.1.1]]

The second identity is $\varepsilon \circ L \,\cdot\, L \circ \eta = \mathrm{id}_L$: reading bottom-up, $L \xrightarrow{L \eta} L R L \xrightarrow{\varepsilon L} L$. As a string diagram: an $L$-string that dips into a cup ($\eta$, creating $R L$ to its right) and then rises through a cap ($\varepsilon$, annihilating $L R$) — a zigzag that pulls straight to the plain $L$-string. When $L$ is a Haskell endofunctor: `triangle2 :: forall x. L x -> L x; triangle2 = counit . fmap unit`, which must equal `id` (here `fmap` is $L$'s, instantiating $L \circ \eta$; `counit` at `L x` is $\varepsilon \circ L$).

> Sources: DaoFP Exercise 15.1.1.

## Solution 15.2.1

#proof — [[DaoFP Chapter 15 Exercises#Exercise 15.2.1|Exercise 15.2.1]]

Replace every $T$-string by the parallel pair $L\,R$. Then $\eta$ is a cup and $\mu = R \varepsilon L$ is a cap between the inner $R$ and $L$ of two adjacent $L R$ pairs. *Left unit* $\mu \circ (\eta T)$: a cup on the left of an $L R$ pair followed by the cap joining the cup's $R$ with the pair's $L$ — the $R$... $L$ zigzag straightens by the first triangle identity, leaving $L R = T$. *Right unit*: symmetric, using the second triangle identity. *Associativity*: three $L R$ pairs with two caps; applying the caps left-first or right-first yields the same diagram since caps on disjoint strings can slide past each other (interchange law).

> Sources: DaoFP Exercise 15.2.1.

## Solution 15.3.1

#proof — [[DaoFP Chapter 15 Exercises#Exercise 15.3.1|Exercise 15.3.1]]

$F a = (1 + a, \mathsf{Left})$ and $U$ forgets the point, so $U F a = 1 + a = $ `Maybe a`. The unit $\eta_a : a \to 1 + a$ is $\mathsf{Right}$ (`Just`); the counit at a pointed object $(b, p)$ is the point-preserving map $[p, \mathrm{id}_b] : 1 + b \to b$; whiskering gives $\mu_a = [\mathsf{Left}, \mathrm{id}] : 1 + (1 + a) \to 1 + a$, i.e. `join Nothing = Nothing; join (Just m) = m` — exactly the Maybe monad. The adjunction: a point-preserving map $(1 + a, \mathsf{Left}) \to (b, p)$ is determined by its restriction to $a$, so $1/\mathcal{C}(F a, (b, p)) \cong \mathcal{C}(a, b)$.

> Sources: DaoFP Exercise 15.3.1.
