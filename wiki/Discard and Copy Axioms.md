#definition #example

Two optional axioms on a [[Symmetric Monoidal Preorder]] $(X, \leq, I, \otimes)$ that change the [[Wiring Diagram]] style:

- **(e) discard axiom**: $x \leq I$ for all $x \in X$ — every resource can be converted into nothing. New icon: a wire may **terminate**. Valid in manufacturing, not in chemistry.
- **(f) copy axiom**: $x \leq x \otimes x$ for all $x \in X$ — every resource can be duplicated. New icon: a wire may **split**. Valid for (classical) information, not for physical objects.

> Sources: 7 Sketches §2.2.3, Eqs. (2.25), (2.26).

**Examples.** With both axioms, the diagram "write email, copy it, send one copy to Alice and one to Bob" (2.26) makes sense. $\mathbf{Bool} = (\mathbb{B}, \leq, \mathsf{true}, \wedge)$ satisfies both ($x \leq \mathsf{true}$ and $x \leq x \wedge x$); so does any preorder whose $\otimes$ is the [[Meet]] and $I$ the top element (a *cartesian* monoidal preorder). [[Cost]] satisfies discard ($x \geq 0$) but not copy ($x \geq 2x$ fails for $0 < x < \infty$).

**Categorified.** In a [[Monoidal Category]] discard and copy become morphisms $\varepsilon_x : x \to I$ and $\delta_x : x \to x \otimes x$; when every object has them coherently (a **comonoid** structure, natural in $x$) the category is [[Cartesian Category|cartesian]] — this is Fox's theorem, and it is why $\mathbf{Set}$ with $\times$ can duplicate and delete data while a category of quantum processes or linear resources cannot. In a [[Hypergraph Category]] every object carries a [[Frobenius Monoid|Frobenius structure]]: copy, discard, and their mirror images merge and create.
