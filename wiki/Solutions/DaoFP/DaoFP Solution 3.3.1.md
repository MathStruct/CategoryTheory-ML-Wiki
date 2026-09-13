#solution #proof

**Solution to [[DaoFP Exercise 3.3.1|Exercise 3.3.1]].**

Set $f^{-1} := \beta_a(\mathrm{id}_a) : b \to a$. Naturality with $x = a$, $h = \mathrm{id}_a$, $g : a \to y$ gives $\beta_y(g) = \beta_y(g \circ \mathrm{id}_a) = g \circ \beta_a(\mathrm{id}_a) = g \circ f^{-1}$, so $\beta_y = (- \circ f^{-1})$ ([[DaoFP Exercise 3.3.2]]); with $f := \beta_b^{-1}(\mathrm{id}_b)$ one checks the two are inverse. See [[Isomorphism]], [[Yoneda Lemma]].

> Sources: DaoFP Exercise 3.3.1.
