#example

The set of animal classifications ('tiger', 'mammal', 'sapiens', 'carnivore', …) is ordered by **specificity**: $\mathsf{tiger} \leq \mathsf{mammal}$ since a tiger is a type of mammal. The result is a [[Preorder]] which is a tree, the **tree of life**. The taxonomic ranks $\mathsf{species} \leq \mathsf{genus} \leq \mathsf{family} \leq \mathsf{order} \leq \mathsf{class} \leq \mathsf{phylum} \leq \mathsf{kingdom}$ form a [[Total Order]], and the map $F$ sending each classification to its rank is a [[Monotone Map]]: whenever there is a path $x \to y$ upstairs there is a path $F(x) \to F(y)$ downstairs.

> Source: 7 Sketches Example 1.61.

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}[row sep=small, column sep=tiny]
 & \mathsf{sapiens} \arrow[d] & & & \\
\mathsf{habilis} \arrow[r] & \mathsf{homo} \arrow[r] & \mathsf{primate} \arrow[dr] & & \\
\mathsf{lion} \arrow[r] & \mathsf{panthera} \arrow[r] & \mathsf{carnivore} \arrow[r] & \mathsf{mammal} & \\
\mathsf{tiger} \arrow[ur] & & & & \\
\mathsf{species} \arrow[r] & \mathsf{genus} \arrow[r] & \mathsf{family} \arrow[r] & \mathsf{order} \arrow[r] & \mathsf{class}
\end{tikzcd}
\end{document}
```

The map $F$ (tiger, lion, habilis, sapiens $\mapsto$ species; panthera, homo $\mapsto$ genus; …) is a monotone map from the upper preorder to the lower one. It is an [[Isomorphism]] of preorders? No — it collapses many classifications to one rank, so it is not injective; it is a typical "observation" in the sense of [[Generative Effect|generative effects]].
