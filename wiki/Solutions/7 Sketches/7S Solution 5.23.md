#solution #proof

**Solution to [[7S Exercise 5.23|Exercise 5.23]].**

1. Each morphism $q : y \to z$ has a domain $y$ and codomain $z$. 2. A functor restricts to $(f, g)$ on vertices and length-1 paths; conversely $(f, g)$ extends to paths by $F(v_0, a_1, \dots, a_n) := \mathrm{id}_{f(v_0)} \mathbin{;} g(a_1) \mathbin{;} \cdots \mathbin{;} g(a_n)$, and the two constructions are inverse (functoriality forces the action on all paths). 3. Yes, it is the underlying graph $U(\mathcal{C})$; part 2 says $\mathrm{Free} \dashv U : \mathbf{Grph} \rightleftarrows \mathbf{Cat}$ is an [[Adjunction]] ([[Free Category]]).

> Sources: 7 Sketches, Exercise 5.23 and Solution A.5.
