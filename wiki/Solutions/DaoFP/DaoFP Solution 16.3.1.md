#solution #program

**Solution to [[DaoFP Exercise 16.3.1|Exercise 16.3.1]].**

```haskell
initial :: Store Int Cell
initial = St (\n -> if n == 0 then L else D) 0
gens :: [Store Int Cell]
gens = iterate (extend step) initial
render :: Store Int Cell -> String
render (St f _) = [ case f n of L -> '#'; D -> '.' | n <- [-8 .. 2] ]
-- mapM_ (putStrLn . render) (take 6 gens) prints the familiar rule-110 triangle growing to the left
```
Each generation is `extend step` of the previous one: every cell looks at its neighbourhood in the store.

> Sources: DaoFP Exercise 16.3.1.
