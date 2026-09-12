#exercise #solution #proof

**Exercise 3.9.** Check that $\mathrm{Free}(G)$ satisfies unitality and associativity.

## Solution

Define a path as $(v, a_1, \dots, a_n)$ with $s(a_1) = v$, $t(a_i) = s(a_{i+1})$; source $v$, target $t(a_n)$ (or $v$ if $n = 0$); concatenation appends the arrow lists. Concatenating with a length-0 path $(v)$ returns the same tuple, and both bracketings of a triple concatenation give $(v, a_1..a_m, b_1..b_n, c_1..c_o)$. See [[Free Category]].
