#solution #proof

**Solution to [[DaoFP Exercise 12.2.1|Exercise 12.2.1]].**

A morphism must satisfy `show . eval = pretty . fmap show`. On the node `PlusF 2 3 :: ExprF Int`: the left side gives `show (2 + 3) = "5"`, the right side gives `pretty (PlusF "2" "3") = "2 + 3"`. They differ, so the square does not commute.

> Sources: DaoFP Exercise 12.2.1.
