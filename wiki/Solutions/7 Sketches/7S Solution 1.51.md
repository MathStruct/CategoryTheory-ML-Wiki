#solution

**Solution to [[7S Exercise 1.51|Exercise 1.51]].**

$\mathcal{P}(\varnothing) = \{\varnothing\}$: a single point. $\mathcal{P}\{1\}$: $\varnothing \to \{1\}$. $\mathcal{P}\{1,2\}$: a square $\varnothing \to \{1\}, \{2\} \to \{1,2\}$.

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}[row sep=small]
\varnothing & & \{1\} & & & \{1,2\} & \\
 & & \varnothing \arrow[u] & & \{1\} \arrow[ur] & & \{2\} \arrow[ul] \\
 & & & & & \varnothing \arrow[ul] \arrow[ur] &
\end{tikzcd}
\end{document}
```

> Sources: 7 Sketches, Exercise 1.51 and Solution A.1.
