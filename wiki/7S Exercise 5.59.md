#exercise #solution #proof

**Exercise 5.59.** Prove Proposition 5.56 in general: construct $g : m \to n$ with $S(g) = M$ as four layers.

## Solution

Layer 1: $g_1 := c_n + \cdots + c_n : m \to mn$ where $c_n : 1 \to n$ makes $n$ copies (composite of copies with identities). Layer 2: $g_2 := \sum_{i,j} s_{M(i,j)} : mn \to mn$, scalars in row-major order. Layer 3: $g_3$, a permutation of swaps and identities sending the $(i-1)n + j$-th wire to the $(j-1)m + i$-th. Layer 4: $g_4 := a_m + \cdots + a_m : mn \to n$ where $a_m : m \to 1$ adds $m$ inputs. By Proposition 5.54 there is exactly one path from input $i$ to output $j$, carrying scalar $M(i,j)$, so $S(g_1 \mathbin{;} g_2 \mathbin{;} g_3 \mathbin{;} g_4) = M$.
