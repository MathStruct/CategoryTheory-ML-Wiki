#solution #program

**Solution to [[DaoFP Exercise 10.5.4|Exercise 10.5.4]].**

The result is a function, so call it: `let R f = triangle' (R (+1)) in f 5` gives `6`, agreeing with `(+1) 5`. `unit (R g) = R (\r -> L (R g, r))`, and `fmap counit` turns each `L (R g, r)` into `g r`.

> Sources: DaoFP Exercise 10.5.4.
