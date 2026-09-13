#theorem #proof

**Theorem 2.49.** There is a one-to-one correspondence between [[Preorder|preorders]] and [[Bool (Monoidal Preorder)|$\mathbf{Bool}$]]-[[Enriched Category|categories]].

> Sources: 7 Sketches Theorem 2.49, Example 2.47, 2.70, Remark 2.71, Exercise 2.50; DaoFP §20.1 ("Preorders").

"Here we find ourselves in the primordial ooze": preorders are $\mathbf{Bool}$-categories, while $\mathbf{Bool}$ is itself a preorder — every preorder, including $\mathbf{Bool}$, is enriched in $\mathbf{Bool}$.

*Proof.* A $\mathbf{Bool}$-category $\mathcal{X}$ is a set $\mathrm{Ob}(\mathcal{X})$ with, for each $x, y$, a truth value $\mathcal{X}(x, y)$. Declare $x \leq y$ iff $\mathcal{X}(x,y) = \mathsf{true}$. Axiom (a), $\mathsf{true} \leq \mathcal{X}(x,x)$, forces $\mathcal{X}(x,x) = \mathsf{true}$: reflexivity. Axiom (b), $\mathcal{X}(x,y) \wedge \mathcal{X}(y,z) \leq \mathcal{X}(x,z)$, has force only when both are $\mathsf{true}$, and then forces $\mathcal{X}(x,z) = \mathsf{true}$: transitivity. Conversely (Example 2.47) a preorder gives a $\mathbf{Bool}$-category with $\mathcal{X}(x,y) = \mathsf{true}$ iff $x \leq y$. The two constructions are inverse ([[7S Chapter 2 Exercises#Exercise 2.50|7S Exercise 2.50]]). $\blacksquare$

DaoFP's version: in the monoidal walking arrow, composition $\mathcal{C}(b,c) \otimes \mathcal{C}(a,b) \to \mathcal{C}(a,c)$ cannot go from $\mathsf{True}$ to $\mathsf{False}$, hence $b \leq c \wedge a \leq b \Rightarrow a \leq c$; identity $j_a : \mathsf{True} \to \mathcal{C}(a,a)$ forces $a \leq a$. Cycles $a \leq b \leq a$ with $a \neq b$ are allowed.

**Extension to maps** (Example 2.70): [[Monotone Map|monotone maps]] are exactly $\mathbf{Bool}$-[[Enriched Functor|functors]], since $\mathcal{X}(x_1,x_2) \leq \mathcal{Y}(F x_1, F x_2)$ in $\mathbb{B}$ says "$x_1 \leq x_2$ implies $F x_1 \leq F x_2$". This gives an [[Equivalence of Categories]] $\mathbf{Preord} \simeq \mathbf{Bool}\text{-}\mathbf{Cat}$ (Remark 2.71, 3.59). The [[Product Preorder]] is the $\mathbf{Bool}$-[[Product of Enriched Categories|product]], and [[Category|categories]] are to $\mathbf{Set}$ as preorders are to $\mathbf{Bool}$.
