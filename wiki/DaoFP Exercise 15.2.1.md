#exercise #solution #proof

**Exercise 15.2.1.** Draw string diagrams illustrating the monad laws (unit and associativity) for the [[Monads from Adjunctions|monad derived from an adjunction]].

## Solution

Replace every $T$-string by the parallel pair $L\,R$. Then $\eta$ is a cup and $\mu = R \varepsilon L$ is a cap between the inner $R$ and $L$ of two adjacent $L R$ pairs. *Left unit* $\mu \circ (\eta T)$: a cup on the left of an $L R$ pair followed by the cap joining the cup's $R$ with the pair's $L$ — the $R$... $L$ zigzag straightens by the first triangle identity, leaving $L R = T$. *Right unit*: symmetric, using the second triangle identity. *Associativity*: three $L R$ pairs with two caps; applying the caps left-first or right-first yields the same diagram since caps on disjoint strings can slide past each other (interchange law).

> Sources: DaoFP Exercise 15.2.1.
