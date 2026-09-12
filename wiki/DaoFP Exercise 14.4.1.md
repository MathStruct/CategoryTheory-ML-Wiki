#exercise #solution #program

**Exercise 14.4.1.** Define `Functor` and `Monad` instances for `newtype E e a = E (e -> Maybe a)` ([[Reader Monad]] combined with [[Maybe Monad]]).

## Solution

```haskell
newtype E e a = E (e -> Maybe a)
runE :: E e a -> e -> Maybe a
runE (E f) e = f e
instance Functor (E e) where
  fmap f (E g) = E (fmap f . g)
instance Applicative (E e) where
  pure a = E (\_ -> Just a)
  ef <*> ea = ef >>= \f -> fmap f ea
instance Monad (E e) where
  ma >>= k = E (\e -> case runE ma e of
                        Nothing -> Nothing
                        Just a  -> runE (k a) e)
```
The environment is passed to both stages (reader) and failure short-circuits (maybe): `E e = ReaderT e Maybe` ([[Monad Transformer]]).

> Sources: DaoFP Exercise 14.4.1.
