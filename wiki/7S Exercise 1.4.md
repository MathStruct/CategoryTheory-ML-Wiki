#exercise #solution

**Exercise 1.4.** What is the result of joining the two systems (partitions of $\{11,12,13,21,22,23\}$): $\{11,12\},\{13\},\{21,22,23\}$ and $\{11\},\{12,13\},\{21\},\{22\},\{23\}$?

> 7 Sketches §1.1.1; see [[Join]], [[Preorder of Partitions]].

## Solution

Take the transitive closure of the union of connections: $11 \sim 12$ and $12 \sim 13$ give $\{11, 12, 13\}$, and the second system adds nothing to the bottom row, so the join is $\{11,12,13\},\{21,22,23\}$.
