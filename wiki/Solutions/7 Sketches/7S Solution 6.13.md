#solution #proof

**Solution to [[7S Exercise 6.13|Exercise 6.13]].**

A preorder is a category with at most one morphism between any two objects, so every diagram commutes. Unfolding the definition of coproduct: $p + q$ is an element with $p \leq p + q$ and $q \leq p + q$ (the inclusions), such that whenever $p \leq x$ and $q \leq x$ we have $p + q \leq x$ (the unique copairing). That is exactly the least upper bound $p \vee q$.

Dually [[Product]]s are [[Meet]]s, and the [[Initial Object]] is the bottom element.

> Sources: 7 Sketches, Exercise 6.13 and Solution A.6.
