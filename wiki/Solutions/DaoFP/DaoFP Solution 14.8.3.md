#solution #program

**Solution to [[DaoFP Exercise 14.8.3|Exercise 14.8.3]].**

```haskell
newtype Const c a = Const { getConst :: c } deriving Functor

showAlg :: MAlg StackF (Const String) a
showAlg = (stop, go)
  where
    stop _ = Const "return\n"
    go (Push n k) = Const ("push " ++ show n ++ "\n" ++ getConst k)
    go (Pop k)    = Const ("pop\n" ++ getConst k)
    go (Add k)    = Const ("add\n" ++ getConst k)
    go (Top ik)   = Const ("top\n" ++ getConst (ik 0))   -- one representative branch

pretty :: FreeMonad StackF a -> String
pretty = getConst . mcata showAlg
```
`Top` has a branch for every `Int`; the printer follows one representative (`ik 0`). The same program `calc` is thus interpreted by two different algebras — execution and printing.

> Sources: DaoFP Exercise 14.8.3.
