#solution #program

**Solution to [[DaoFP Exercise 4.4.1|Exercise 4.4.1]].**

```haskell
import Data.Void (Void, absurd)
f :: Either a Void -> a
f (Left a)  = a
f (Right v) = absurd v          -- unreachable: Void has no terms

f_1 :: a -> Either a Void
f_1 = Left
```

`f . f_1 = id` by the computation rule; `f_1 . f = id` because the `Right` case never occurs. Categorically: arrows out of $a + 0$ are pairs $(x, \text{¡})$ with $\text{¡}$ unique, hence in natural bijection with arrows out of $a$.

> Sources: DaoFP Exercise 4.4.1.
