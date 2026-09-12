#exercise #solution #program

**Exercise 9.8.3.** Implement `Representable` for `data Pair x = Pair x x`.

## Solution

```haskell
instance Representable Pair where
  type Key Pair = Bool
  tabulate g = Pair (g True) (g False)
  index (Pair a b) = \k -> if k then a else b
```
$x \times x \cong x^{\mathbf{2}}$; see [[Representable Functor]].
