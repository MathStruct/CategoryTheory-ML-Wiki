#solution #program

**Solution to [[DaoFP Exercise 10.5.3|Exercise 10.5.3]].**

`triangle = counit . fmap unit`: `fmap unit (L (2,'a')) = L (R (\r -> L (2, r)), 'a')`, then `counit` applies the function to `'a'`, giving `L (2, 'a')` — the identity, as required.

> Sources: DaoFP Exercise 10.5.3.
