#exercise #solution #program

**Exercise 10.5.4.** How to test the second triangle identity `triangle' = fmap counit . unit :: R r x -> R r x`?

## Solution

The result is a function, so call it: `let R f = triangle' (R (+1)) in f 5` gives `6`, agreeing with `(+1) 5`. `unit (R g) = R (\r -> L (R g, r))`, and `fmap counit` turns each `L (R g, r)` into `g r`.
