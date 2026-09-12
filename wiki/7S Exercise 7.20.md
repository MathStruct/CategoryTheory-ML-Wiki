#exercise #solution #example

**Exercise 7.20.** Define $P \Rightarrow Q$ to mean $P = (P \wedge Q)$. 1. Write the truth table of $P = (P \wedge Q)$. 2. Does it agree with your idea of implication? 3. What is the characteristic function of $P \Rightarrow Q$? 4. Which subobject does it classify?

## Solution

1.
| $P$ | $Q$ | $P \wedge Q$ | $P = (P \wedge Q)$ |
|---|---|---|---|
| t | t | t | t |
| t | f | f | f |
| f | t | f | t |
| f | f | f | t |

2. Yes — this is material implication.
3. $\Rightarrow : \mathbb{B} \times \mathbb{B} \to \mathbb{B}$ given by the last column.
4. It classifies $\{(\mathsf{t},\mathsf{t}), (\mathsf{f},\mathsf{t}), (\mathsf{f},\mathsf{f})\} \subseteq \mathbb{B} \times \mathbb{B}$ — the [[Equalizer]] of $\wedge$ and $\pi_1$ ([[Internal Logic of a Topos]]).

> Sources: 7 Sketches, Exercise 7.20 and Solution A.7.
