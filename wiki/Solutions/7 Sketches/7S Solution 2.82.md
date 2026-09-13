#solution #proof

**Solution to [[7S Exercise 2.82|Exercise 2.82]].**

1. If $u \leq u'$ then $u \otimes v \leq u' \otimes v$ by monotonicity (a) with $v \leq v$.
2. Put $a := v \multimap w$ in (2.80): the right side $(v \multimap w) \leq (v \multimap w)$ holds by reflexivity, so $(v \multimap w) \otimes v \leq w$.
3. If $u \leq u'$ then $(v \multimap u) \otimes v \leq u \leq u'$, so by (2.80) $(v \multimap u) \leq (v \multimap u')$.
4. (2.80) is exactly the [[Galois Connection]] condition for $(- \otimes v) \dashv (v \multimap -)$, and 1, 3 supply the required monotonicity.

> Sources: 7 Sketches, Exercise 2.82 and Solution A.2.
