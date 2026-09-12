#exercise #solution #example

**Exercise 11.4.1.** Let $A = \{0, 1\}$ and let $f$ map all of $B$ to $1$. How is the function on the right of the [[Dependent Product]] adjunction defined, and what does it do to the fiber over $0$?

## Solution

$f^{-1}(1) = B$ and $f^{-1}(0) = \varnothing$. The fiber of $\Pi_f E$ over $1$ is the set of sections of $E$ over all of $B$; the fiber over $0$ is the set of sections over the empty patch — a singleton (the empty section). Correspondingly $f^* G$ contains only the fiber of $G$ over $1$, replanted over every point of $B$, so a map $f^* G \to E$ is a family of sections indexed by $G_1$; the right-hand side $\phi^T : G \to \Pi_f E$ sends $G_1$ to those sections and sends the fiber $G_0$ to the unique point of $(\Pi_f E)_0$ — there is no other choice.

> Sources: DaoFP Exercise 11.4.1.
