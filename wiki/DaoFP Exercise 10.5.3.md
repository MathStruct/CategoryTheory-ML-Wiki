#exercise #solution #program

**Exercise 10.5.3.** Test the first triangle identity for currying: `triangle (L (2, 'a'))`.

## Solution

`triangle = counit . fmap unit`: `fmap unit (L (2,'a')) = L (R (\r -> L (2, r)), 'a')`, then `counit` applies the function to `'a'`, giving `L (2, 'a')` — the identity, as required.
