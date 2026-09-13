#solution #program

**Solution to [[DaoFP Exercise 9.3.2|Exercise 9.3.2]].**

`safeHead . fmap reverse` and `fmap reverse . safeHead`, both of type `[[a]] -> Maybe [a]`; e.g. on `[[1,2],[3]]` both give `Just [2,1]`, on `[]` both give `Nothing`. Equal by naturality of `safeHead`.

> Sources: DaoFP Exercise 9.3.2.
