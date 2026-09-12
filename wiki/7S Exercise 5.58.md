#exercise #solution

**Exercise 5.58.** Draw signal flow graphs for 1. $\begin{pmatrix}0\\1\\2\end{pmatrix}$; 2. $\begin{pmatrix}0&0\\0&0\end{pmatrix}$; 3. $\begin{pmatrix}1&2&3\\4&5&6\end{pmatrix}$.

## Solution

1. Three inputs: discard the first, pass the second, amplify the third by 2, add all into one output (or, minimally: discard input 1, amplify input 3 by 2, add). 2. Discard both inputs; two zero outputs. 3. Copy each of the two inputs three times, amplify by $1, 2, 3$ resp. $4, 5, 6$, permute, and add pairwise into three outputs (the four-layer normal form of [[Prop of Matrices|Proposition 5.56]]).
