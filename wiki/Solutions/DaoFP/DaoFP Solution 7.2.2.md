#solution #annotation

**Solution to [[DaoFP Exercise 7.2.2|Exercise 7.2.2]].**

In $\mathbf{Set}$ there are uncountably many functions $L_a \to 1 + a$ (any assignment of `Nothing` or an element to each list), and only countably many are folds built from finitely describable `init` and `step`, so the recursor does not reach them all — an arbitrary mapping out of a recursive type contains infinite information. Haskell functions `[a] -> Maybe a` that are *parametric* in `a` are far fewer: they can only return `Nothing` or one of the list's elements chosen by position, and each such function (e.g. `safeHead`, `last`, the third element) is a fold.

> Sources: DaoFP Exercise 7.2.2.
