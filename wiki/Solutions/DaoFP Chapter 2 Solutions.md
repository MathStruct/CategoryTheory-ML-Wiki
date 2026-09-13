#solution

Solutions to the exercises of DaoFP, Chapter 2: [[DaoFP Chapter 2 Exercises]]. Index: [[Map of Content]].

## Solution 2.1.1

#proof — [[DaoFP Chapter 2 Exercises#Exercise 2.1.1|Exercise 2.1.1]]

Both sides send $h$ to $g \circ (f \circ h) = (g \circ f) \circ h$ by associativity. Post-composition is thus a [[Functor]]: the covariant [[Hom Functor]] $\mathcal{C}(x, -)$.

> Sources: DaoFP Exercise 2.1.1.

## Solution 2.1.2

[[DaoFP Chapter 2 Exercises#Exercise 2.1.2|Exercise 2.1.2]]

$(h \circ -) \circ ((g \circ -) \circ (f \circ -))$ and $((h \circ -) \circ (g \circ -)) \circ (f \circ -)$ both send $k$ to $h \circ g \circ f \circ k$, by associativity of $\circ$ in the category. See [[Function Composition]].

> Sources: DaoFP Exercise 2.1.2.

## Solution 2.1.3

#proof — [[DaoFP Chapter 2 Exercises#Exercise 2.1.3|Exercise 2.1.3]]

Applied to $h : c \to x$: the right side gives $(h \circ g) \circ f$, the left $h \circ (g \circ f)$; equal by associativity. Pre-composition is a [[Contravariant Functor]] — the contravariant [[Hom Functor]] $\mathcal{C}(-, x)$.

> Sources: DaoFP Exercise 2.1.3.

## Solution 2.3.1

[[DaoFP Chapter 2 Exercises#Exercise 2.3.1|Exercise 2.3.1]]

Nothing: $\mathrm{id}_a \circ h = h$ and $k \circ \mathrm{id}_a = k$ by the identity laws. See [[Identity Function]], [[Category]].

> Sources: DaoFP Exercise 2.3.1.

## Solution 2.4.1

#proof — [[DaoFP Chapter 2 Exercises#Exercise 2.4.1|Exercise 2.4.1]]

Let $x : 1 \to a$ and $g_1, g_2 : c \to 1$ with $x \circ g_1 = x \circ g_2$. Since $1$ is terminal, $g_1 = g_2 = !_c$ regardless. So $x$ is mono.

> Sources: DaoFP Exercise 2.4.1.

## Solution 2.5.1

#proof — [[DaoFP Chapter 2 Exercises#Exercise 2.5.1|Exercise 2.5.1]]

Let $! : a \to 1$ and $g_1, g_2 : 1 \to c$ with $g_1 \circ ! = g_2 \circ !$. If $a$ has a [[Global Element]] $s : 1 \to a$ (so $! \circ s = \mathrm{id}_1$, i.e. $s$ is a [[Section and Retraction|section]] of $!$), then $g_1 = g_1 \circ ! \circ s = g_2 \circ ! \circ s = g_2$, so $!$ is epi. This is the intended reading (in $\mathbf{Set}$: every nonempty set surjects onto the point). Caveat: for $a = \varnothing$ the map $\varnothing \to 1$ is *not* epi in $\mathbf{Set}$, since the two maps $1 \to \underline{2}$ agree on $\varnothing$ — so the statement needs $a$ to have an element.

> Sources: DaoFP Exercise 2.5.1.
