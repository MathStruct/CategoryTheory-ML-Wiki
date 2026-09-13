#solution #program

**Solution to [[DaoFP Exercise 14.9.2|Exercise 14.9.2]].**

```haskell
liftA3 :: Applicative f => (a -> b -> c -> d) -> f a -> f b -> f c -> f d
liftA3 g as bs cs = g <$> as <*> bs <*> cs
```

> Sources: DaoFP Exercise 14.9.2.
