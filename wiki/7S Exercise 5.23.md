#exercise #solution #proof

**Exercise 5.23.** Let $\mathcal{G} = \mathrm{Free}(G)$ for a graph $G = (V, A, s, t)$ and $\mathcal{C}$ a category. 1. Interpret $\mathrm{dom}, \mathrm{cod} : \mathrm{Mor}(\mathcal{C}) \to \mathrm{Ob}(\mathcal{C})$. 2. Show functors $\mathcal{G} \to \mathcal{C}$ correspond to pairs $(f : V \to \mathrm{Ob}, g : A \to \mathrm{Mor})$ with $\mathrm{dom}(g(a)) = f(s(a))$, $\mathrm{cod}(g(a)) = f(t(a))$. 3. Is $(\mathrm{Mor}(\mathcal{C}), \mathrm{Ob}(\mathcal{C}), \mathrm{dom}, \mathrm{cod})$ a graph? Use the word "adjunction".

## Solution

1. Each morphism $q : y \to z$ has a domain $y$ and codomain $z$. 2. A functor restricts to $(f, g)$ on vertices and length-1 paths; conversely $(f, g)$ extends to paths by $F(v_0, a_1, \dots, a_n) := \mathrm{id}_{f(v_0)} \mathbin{;} g(a_1) \mathbin{;} \cdots \mathbin{;} g(a_n)$, and the two constructions are inverse (functoriality forces the action on all paths). 3. Yes, it is the underlying graph $U(\mathcal{C})$; part 2 says $\mathrm{Free} \dashv U : \mathbf{Grph} \rightleftarrows \mathbf{Cat}$ is an [[Adjunction]] ([[Free Category]]).
