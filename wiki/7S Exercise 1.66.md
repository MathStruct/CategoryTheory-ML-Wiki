#exercise #solution #proof

**Exercise 1.66.** For a preorder $P$ and $p \in P$: 1. show $\uparrow p := \{p' \mid p \leq p'\}$ is an upper set; 2. show $\uparrow : P^{\mathrm{op}} \to \mathcal{U}(P)$ is monotone; 3. show $p \leq p'$ iff $\uparrow p' \subseteq \uparrow p$; 4. draw $\uparrow$ for $P = (b \geq a \leq c)$.

## Solution

See [[Yoneda Lemma for Preorders]] for the proofs. For 4: $\uparrow a = \{a,b,c\}$, $\uparrow b = \{b\}$, $\uparrow c = \{c\}$; in $\mathcal{U}(P)$, $\{b\}, \{c\} \subseteq \{b, c\} \subseteq \{a,b,c\}$, and $\uparrow$ sends the bottom element $a$ of $P$ to the top element of $\mathcal{U}(P)$.
