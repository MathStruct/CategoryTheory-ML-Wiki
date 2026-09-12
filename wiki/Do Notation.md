#definition #example #program

**Do notation** is syntactic sugar for nested [[Monad|binds]] and lambdas: the line `x <- mx` ("x gets the result of mx") followed by `rest` desugars to `mx >>= \x -> rest`; the last line must be a monadic value (often `return e`). It lets one name intermediate results of [[Kleisli Category|Kleisli arrows]] instead of composing them point-free with `<=<`.

```haskell
main = do                          -- desugars to:  getLine >>= \s1 ->
  s1 <- getLine                    --                 getLine >>= \s2 ->
  s2 <- getLine                    --                   putStrLn ("Hello " ++ s1 ++ " " ++ s2)
  putStrLn ("Hello " ++ s1 ++ " " ++ s2)
```

> Sources: DaoFP §14.5 ("Do Notation"), Exercises 14.5.1–14.5.2; §14.6 (CPS via `Cont` in do notation); §14.9 (`ApplicativeDo`).

- `pairs as bs = do { a <- as; b <- bs; return (a, b) }` in the [[List Monad]]; `ap fs as = do { f <- fs; a <- as; return (f a) }` for any monad ([[DaoFP Exercise 14.5.1]]).
- The final `return` typically needs variables bound in *outer* lambdas — this depends on the monad being [[Functorial Strength|strong]], which every Haskell functor is.
- `ApplicativeDo` lets the compiler use [[Applicative Functor|applicative]] combinators where no line depends on an earlier result, enabling parallelism. Imperative coroutines (C++) mimic do notation for hard-coded monads.

````tabs
tab: Julia
```julia
# a tiny "do" as a macro over a bind function
macro mdo(bind, block)
    lines = filter(x -> !(x isa LineNumberNode), block.args)
    ex = lines[end]
    for l in reverse(lines[1:end-1])
        if l isa Expr && l.head == :call && l.args[1] == :<--
            ex = :($bind($(l.args[3]), $(l.args[2]) -> $ex))
        else
            ex = :($bind($l, _ -> $ex))
        end
    end
    esc(ex)
end
bindL(as, k) = reduce(vcat, (k(a) for a in as); init=Any[])
@mdo bindL begin
    a <-- [1, 2]
    b <-- ['x', 'y']
    [(a, b)]
end                                     # [(1,'x'), (1,'y'), (2,'x'), (2,'y')]
```
tab: Lean
```lean
-- Lean's do notation is the same sugar over bind
example : Option ℕ := do
  let a ← some 1
  let b ← some 2
  pure (a + b)
```
tab: Haskell
```haskell
ap :: Monad m => m (a -> b) -> m a -> m b
ap fs as = do
  f <- fs
  a <- as
  return (f a)
```
````
