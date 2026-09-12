#exercise #solution

**Exercise 3.64.** For $G = 1 \xrightarrow{a} 2 \xrightarrow{b} 3$ and $H = 4 \xrightarrow{c, d} 5 \circlearrowleft e$, the unique [[Graph Homomorphism]] with $\alpha_{\mathrm{Arrow}}(a) = d$: find the other values and check naturality.

## Solution

$\alpha_{\mathrm{Arrow}}(b) = e$; $\alpha_{\mathrm{Vertex}}(1) = 4$, $\alpha_{\mathrm{Vertex}}(2) = 5$, $\alpha_{\mathrm{Vertex}}(3) = 5$. Check: $\mathrm{source}(\alpha(a)) = \mathrm{source}(d) = 4 = \alpha(1) = \alpha(\mathrm{source}(a))$, and both paths from $a$ through target end at $5$; similarly for $b$.
