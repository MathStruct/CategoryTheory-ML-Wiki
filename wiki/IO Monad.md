#example #annotation

The **IO monad** handles interaction with the external world by letting the program *generate a script* that the runtime executes: values `IO a` are opaque (there is no `runIO`), and the program's result is `main :: IO ()`. Primitives are Kleisli arrows or `IO` objects (Kleisli arrows from `()`): `getLine :: IO String`, `putStrLn :: String -> IO ()`; `main = getLine >>= putStrLn` echoes a line. "The IO monad is the ultimate procrastinator": Kleisli composition piles up tasks for later execution. Because of laziness, pure code is evaluated on demand *driven by* the IO script — "if it weren't for I/O, nothing would ever be evaluated". Even IO code can branch on results (`if s == "yes" then ... else ...`), which [[Applicative Functor|applicatives]] cannot; the inspection is postponed until the runtime interprets the script.

> Sources: DaoFP §14.1 ("Input/Output"), §14.4, §14.5 ([[Do Notation]]), §14.9 ("Monads and applicatives").

````tabs
tab: Lean
```lean
-- Lean's IO monad works the same way: main : IO Unit is a script
def main : IO Unit := do
  let s ← (← IO.getStdin).getLine
  IO.println s
```
tab: Haskell
```haskell
main :: IO ()
main = do
  s1 <- getLine
  s2 <- getLine
  putStrLn ("Hello " ++ s1 ++ " " ++ s2)
```
````
