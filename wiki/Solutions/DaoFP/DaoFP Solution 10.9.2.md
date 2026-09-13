#solution #program

**Solution to [[DaoFP Exercise 10.9.2|Exercise 10.9.2]].**

```haskell
import Data.Monoid (Sum(..), Product(..))
sumL, prodL :: [Int] -> Int
sumL  = getSum     . foldMap Sum
prodL = getProduct . foldMap Product
-- sumL [1,2,3,4] == 10, prodL [1,2,3,4] == 24
```
The same "program" (the list) run by two interpreters. See [[Free Monoid]].

> Sources: DaoFP Exercise 10.9.2.
