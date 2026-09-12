#exercise #solution #proof

**Exercise 2.104.** In a [[Quantale]], prove 1. $I_X \ast M = M$; 2. $(M \ast N) \ast P = M \ast (N \ast P)$.

## Solution

First, $0 \otimes v = v \otimes \bigvee \varnothing = \bigvee_{a \in \varnothing} v \otimes a = 0$ by Proposition 2.87(b) and symmetry.
1. $(I_X \ast M)(x, y) = \bigvee_{x'} I_X(x,x') \otimes M(x', y) = (I \otimes M(x,y)) \vee \bigvee_{x' \neq x} (0 \otimes M(x',y)) = M(x,y) \vee 0 = M(x,y)$.
2. $((M \ast N) \ast P)(w,z) = \bigvee_y \big(\bigvee_x M(w,x) \otimes N(x,y)\big) \otimes P(y,z) = \bigvee_{x,y} M(w,x) \otimes N(x,y) \otimes P(y,z) = \bigvee_x M(w,x) \otimes \big(\bigvee_y N(x,y) \otimes P(y,z)\big) = (M \ast (N \ast P))(w,z)$, using distributivity of $\otimes$ over joins and associativity of $\otimes$.
