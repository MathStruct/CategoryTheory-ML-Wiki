#theorem #proof #example #program

**Theorem.** If $X$ and $Y$ are [[Finite Set|finite sets]] and $X$ has larger [[Cardinality]] than $Y$, then for any function $f : X \to Y$ there exist distinct $x, x' \in X$ with $f(x) = f(x')$. In other words, $f$ is not [[Injection|injective]].

> Source: Kittenlab Lecture 2.

*Proof.* Suppose no such pair exists. Writing $X = \{x_1, \dots, x_n\}$ with the $x_i$ distinct, the values $f(x_1), \dots, f(x_n)$ are all distinct, so $Y$ has at least $n$ distinct elements — contradicting that $Y$ is smaller. $\blacksquare$

**Example.** Any five points on the unit sphere have four lying in a common closed hemisphere: pick two, they determine a great circle; at least two of the remaining three lie on one of the hemispheres it bounds.

````tabs
tab: Julia
```julia
# Kittenlab Lecture 2: find the collision
function pigeonhole(f::𝔽Mor)
  @assert length(unique!([f.dom...])) > length(unique!([f.codom...]))
  holes = Dict(y => Any[] for y in f.codom)
  for pigeon in f.dom
    push!(holes[f(pigeon)], pigeon)
  end
  for hole in values(holes)
    length(hole) > 1 && return hole
  end
end
```
tab: Lean
```lean
#check @Fintype.exists_ne_map_eq_of_card_lt
-- (f : α → β) (h : Fintype.card β < Fintype.card α) : ∃ x y, x ≠ y ∧ f x = f y
```
tab: Haskell
```haskell
import qualified Data.Map as M
pigeonhole :: Ord b => [a] -> (a -> b) -> Maybe [a]
pigeonhole xs f =
  let holes = M.fromListWith (++) [(f x, [x]) | x <- xs]
  in case filter ((> 1) . length) (M.elems holes) of
       (h:_) -> Just h
       []    -> Nothing
```
````
