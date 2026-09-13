#solution

Solutions to the exercises of DaoFP, Chapter 13: [[DaoFP Chapter 13 Exercises]]. Index: [[Map of Content]].

## Solution 13.2.1

#proof — [[DaoFP Chapter 13 Exercises#Exercise 13.2.1|Exercise 13.2.1]]

$(F t, F \tau)$ is a coalgebra, so terminality gives a unique coalgebra morphism $h : F t \to t$, i.e. $\tau \circ h = F h \circ F \tau$. Pasting this square with the (trivially commuting) square of $\tau$ shows $h \circ \tau : t \to t$ is a coalgebra morphism $(t, \tau) \to (t, \tau)$; so is $\mathrm{id}_t$; by uniqueness $h \circ \tau = \mathrm{id}_t$. Then $\tau \circ h = F h \circ F \tau = F(h \circ \tau) = \mathrm{id}_{F t}$. Hence $\tau^{-1} = h$ and $F t \cong t$.

> Sources: DaoFP Exercise 13.2.1.

## Solution 13.2.2

#proof — [[DaoFP Chapter 13 Exercises#Exercise 13.2.2|Exercise 13.2.2]]

$\mathrm{Id}(X) = X$ for every $X$, so every set is a fixed point. The least fixed point must have an arrow to every fixed point: only $\varnothing$ has a (unique) map to every set. The greatest fixed point must receive an arrow from every fixed point: only the singleton $1$ receives a (unique) map from every set. (Any nonempty set receives maps from all sets, but not uniquely; the terminal one is $1$.)

> Sources: DaoFP Exercise 13.2.2.

## Solution 13.2.3

#proof — [[DaoFP Chapter 13 Exercises#Exercise 13.2.3|Exercise 13.2.3]]

$(\varnothing, \mathrm{id}_\varnothing)$ is an $\mathrm{Id}$-algebra. For any algebra $(a, \alpha : a \to a)$ the unique function $¡ : \varnothing \to a$ satisfies $¡ \circ \mathrm{id} = \alpha \circ ¡$ (both sides are the empty function), so it is an algebra morphism, and it is the only one. Dually, $(1, \mathrm{id}_1)$ is a coalgebra and for any $(a, \alpha)$ the unique $! : a \to 1$ satisfies $\mathrm{id}_1 \circ ! = ! \circ \alpha$ (both are the unique map $a \to 1$), so it is the unique coalgebra morphism. This is the "impedance mismatch": $\mu \mathrm{Id} = \varnothing \subsetneq 1 = \nu \mathrm{Id}$.

> Sources: DaoFP Exercise 13.2.3.
