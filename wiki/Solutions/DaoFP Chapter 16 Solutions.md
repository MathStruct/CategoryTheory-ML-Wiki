#solution

Solutions to the exercises of DaoFP, Chapter 16: [[DaoFP Chapter 16 Exercises]]. Index: [[Map of Content]].

## Solution 16.0.1

#proof — [[DaoFP Chapter 16 Exercises#Exercise 16.0.1|Exercise 16.0.1]]

For $f : (a, e) \to b$, $g : (b, e) \to c$, $h : (c, e) \to d$:
`(h ∘ (g ∘ f)) (a, e) = h ((g ∘ f)(a, e), e) = h (g (f (a, e), e), e)` and
`((h ∘ g) ∘ f) (a, e) = (h ∘ g) (f (a, e), e) = h (g (f (a, e), e), e)`. Both pass the same environment $e$ to every stage, so they agree. The identity `idWithEnv (a, e) = a` is a two-sided unit.

> Sources: DaoFP Exercise 16.0.1.

## Solution 16.1.1

#program — [[DaoFP Chapter 16 Exercises#Exercise 16.1.1|Exercise 16.1.1]]

```haskell
duplicate :: Comonad w => w a -> w (w a)
duplicate = extend id
extend :: Comonad w => (w a -> b) -> w a -> w b
extend f = fmap f . duplicate
```
Dual to `join = (>>= id)` and `ma >>= k = join (fmap k ma)`.

> Sources: DaoFP Exercise 16.1.1.

## Solution 16.1.2

#program — [[DaoFP Chapter 16 Exercises#Exercise 16.1.2|Exercise 16.1.2]]

```haskell
data BiStream a = BStr [a] [a] deriving Functor
instance Comonad BiStream where
  extract (BStr _ (a : _)) = a
  duplicate s = BStr (tail (iterate left s)) (iterate right s)
    where left  (BStr (p : ps) fs) = BStr ps (p : fs)      -- move the cursor to the past
          right (BStr ps (f : fs)) = BStr (f : ps) fs      -- move the cursor to the future
```
`duplicate` places at each position the whole stream re-centred there.

> Sources: DaoFP Exercise 16.1.2.

## Solution 16.1.3

#program — [[DaoFP Chapter 16 Exercises#Exercise 16.1.3|Exercise 16.1.3]]

```haskell
lowPass :: BiStream Double -> Double
lowPass (BStr (p : _) (c : f : _)) = (p + c + f) / 3
smooth :: BiStream Double -> BiStream Double
smooth = extend lowPass

gauss :: BiStream Double -> Double                 -- weights 1 4 6 4 1 / 16
gauss (BStr (p1 : p2 : _) (c : f1 : f2 : _)) = (p2 + 4 * p1 + 6 * c + 4 * f1 + f2) / 16
```
`extend` performs the convolution of the kernel over the whole stream.

> Sources: DaoFP Exercise 16.1.3.

## Solution 16.3.1

#program — [[DaoFP Chapter 16 Exercises#Exercise 16.3.1|Exercise 16.3.1]]

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
