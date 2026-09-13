#example #annotation

**Resource theories** study how resources are exchanged in a given arena — "what buys what", statically. 7 Sketches models a resource theory as a [[Symmetric Monoidal Preorder]]: $x \leq y$ means "$x$ can be converted into $y$", $x \otimes y$ means "$x$ and $y$ together", $I$ means "nothing". [[Wiring Diagrams for Monoidal Preorders|Wiring diagrams]] are recipes; the three questions are "is it possible?" ($\mathbf{Bool}$), "what is the minimum cost?" ([[Cost]]), and "what are the ways?" ($\mathbf{Set}$, [[Monoidal Category|monoidal categories]]).

> Sources: 7 Sketches §2.1, §2.2.3, Examples 2.19, 2.86, Exercise 2.21; following [CFS16; Fri17].

## Chemistry

Material collections such as $\mathrm{H_2O}$, $\mathrm{NaCl}$, $2\mathrm{NaOH}$, $\mathrm{CH_4} + 3\mathrm{O_2}$ form the preorder $(\mathrm{Mat}, \to, 0, +)$: $\to$ is the order (reaction), $+$ the monoidal product, $0$ the unit ([[7S Chapter 2 Exercises#Exercise 2.21|7S Exercise 2.21]]). E.g. $2\mathrm{H_2O} + 2\mathrm{Na} \to 2\mathrm{NaOH} + \mathrm{H_2}$ (reactant $\to$ product).

**Catalysis.** From the reactions

$$
y + k \to y' + k', \qquad x + y' \to z', \qquad z' + k' \to z + k \qquad (2.22)
$$

the monoidal preorder laws give the composite $x + y + k \to x + y' + k' \to z' + k' \to z + k$ (2.23). Here $k$ is a **catalyst**: it appears on both sides, and $x + y \to z$ is not derivable without it. The wiring diagram (2.24) has three interior boxes and exterior box $x + y + k \to z + k$. Ovens "catalyze" baking pies: a *means of production*.

Chemistry is not [[Monoidal Closed Preorder|closed]] (Example 2.86): closure would require a "substance" $2\mathrm{Na} \multimap (2\mathrm{NaOH} + \mathrm{H_2})$ obtainable from two water molecules — really a *potential reaction* unlocked by water.

## Manufacturing: the discard axiom

"You can trash anything you want, and it disappears from view." Adding the [[Discard and Copy Axioms|discard axiom]] $x \leq I$ lets wires terminate (egg shells, butter wrappers) — valid in manufacturing but *not* chemistry ($\mathrm{H_2O} + \mathrm{NaCl} \to \mathrm{H_2O}$ is not a legal equation: you cannot discard $10^{23}$ dissolved molecules).

## Informatics: the copy axiom

Information can be copied ("one cup of butter never becomes two, but a single email can be sent to two people"): the copy axiom $x \leq x \otimes x$ lets wires split. Copying a CD's *contents* is possible; literally duplicating the *disc* "is magic". Cell mitosis is a remarkable box $\mathsf{cell} \to \mathsf{cell} \otimes \mathsf{cell}$, not an axiom.

Resource theories are treated in much more depth in [CFS16; Fri17] (thermodynamics, communication channels, entanglement, asymptotic conversion rates). Database schemas are similarly "ad hoc" — formed for a particular purpose — and there is nothing wrong with that.
