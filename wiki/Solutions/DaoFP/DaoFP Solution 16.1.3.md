#solution #program

**Solution to [[DaoFP Exercise 16.1.3|Exercise 16.1.3]].**

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
