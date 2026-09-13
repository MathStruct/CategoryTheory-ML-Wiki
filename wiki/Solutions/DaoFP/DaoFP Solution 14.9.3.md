#solution #proof

**Solution to [[DaoFP Exercise 14.9.3|Exercise 14.9.3]].**

- *Identity*: `zipWith ($) (repeat id) v = map id v = v`.
- *Homomorphism*: `zipWith ($) (repeat f) (repeat x) = repeat (f x) = pure (f x)`.
- *Interchange*: `zipWith ($) u (repeat y) = map ($ y) u = zipWith ($) (repeat ($ y)) u`.
- *Composition*: `zipWith ($) (zipWith ($) (zipWith ($) (repeat (.)) u) v) w` zips position-wise to `[(u_i . v_i) w_i]`, and `zipWith ($) u (zipWith ($) v w) = [u_i (v_i w_i)]`; these agree, and both are truncated to the shortest list.

So `ZipList` is a lawful applicative, though not a monad (there is no `join` compatible with `repeat`).

> Sources: DaoFP Exercise 14.9.3.
