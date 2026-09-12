#exercise #solution #proof

**Exercise 2.82.** Prove that a monoidal preorder is [[Monoidal Closed Preorder|closed]] iff $(- \otimes v)$ has a right adjoint $(v \multimap -)$ for every $v$: 1. $(- \otimes v)$ is monotone; 2. if closed, $(v \multimap w) \otimes v \leq w$; 3. $(v \multimap -)$ is monotone; 4. conclude.

## Solution

1. If $u \leq u'$ then $u \otimes v \leq u' \otimes v$ by monotonicity (a) with $v \leq v$.
2. Put $a := v \multimap w$ in (2.80): the right side $(v \multimap w) \leq (v \multimap w)$ holds by reflexivity, so $(v \multimap w) \otimes v \leq w$.
3. If $u \leq u'$ then $(v \multimap u) \otimes v \leq u \leq u'$, so by (2.80) $(v \multimap u) \leq (v \multimap u')$.
4. (2.80) is exactly the [[Galois Connection]] condition for $(- \otimes v) \dashv (v \multimap -)$, and 1, 3 supply the required monotonicity.
