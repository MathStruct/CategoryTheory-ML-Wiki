#solution #proof

**Solution to [[7S Exercise 4.32|Exercise 4.32]].**

As in [[7S Exercise 2.104]]: $((\Phi \mathbin{;} \Psi) \mathbin{;} \Upsilon)(p,s) = \bigvee_r \big(\bigvee_q \Phi(p,q) \otimes \Psi(q,r)\big) \otimes \Upsilon(r,s) = \bigvee_{q,r} \Phi(p,q) \otimes \Psi(q,r) \otimes \Upsilon(r,s) = \bigvee_q \Phi(p,q) \otimes \big(\bigvee_r \Psi(q,r) \otimes \Upsilon(r,s)\big) = (\Phi \mathbin{;} (\Psi \mathbin{;} \Upsilon))(p,s)$, using distributivity of $\otimes$ over $\vee$ (closedness) and skeletality to turn $\cong$ into $=$.

> Sources: 7 Sketches, Exercise 4.32 and Solution A.4.
