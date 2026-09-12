#exercise #solution #proof

**Exercise 13.2.2.** For the identity functor on $\mathbf{Set}$, show every object is a fixed point, $\varnothing$ is the least and $1$ the greatest fixed point ([[Initial Algebra]], [[Terminal Coalgebra]]).

## Solution

$\mathrm{Id}(X) = X$ for every $X$, so every set is a fixed point. The least fixed point must have an arrow to every fixed point: only $\varnothing$ has a (unique) map to every set. The greatest fixed point must receive an arrow from every fixed point: only the singleton $1$ receives a (unique) map from every set. (Any nonempty set receives maps from all sets, but not uniquely; the terminal one is $1$.)

> Sources: DaoFP Exercise 13.2.2.
