#exercise #solution #proof

**Exercise 1.101.** 1. Does $\lceil -/3 \rceil : \mathbb{R} \to \mathbb{Z}$ have a left adjoint $L : \mathbb{Z} \to \mathbb{R}$? 2. If not, why?

## Solution

Suppose $L \dashv \lceil -/3 \rceil$. Then $L(z) \leq r$ iff $z \leq \lceil r/3 \rceil$. Take $z = 1$, $r = 0.01$: $\lceil 0.01/3 \rceil = 1 \geq 1$, so $L(1) \leq 0.01$; similarly $L(1) \leq r$ for every $r > 0$, so $L(1) \leq 0$. But then $1 \leq \lceil 0/3 \rceil = 0$, a contradiction. So there is no left adjoint. (Equivalently: $\lceil -/3 \rceil$ does not preserve meets — $\bigwedge_{r > 0} \lceil r/3 \rceil = 1 \neq 0 = \lceil \bigwedge_{r>0} r / 3 \rceil$; see [[Adjoint Functor Theorem for Preorders]].)
