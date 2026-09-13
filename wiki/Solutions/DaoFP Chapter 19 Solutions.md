#solution

Solutions to the exercises of DaoFP, Chapter 19: [[DaoFP Chapter 19 Exercises]]. Index: [[Map of Content]].

## Solution 19.1.1

#program — [[DaoFP Chapter 19 Exercises#Exercise 19.1.1|Exercise 19.1.1]]

From $[G, H]_{\mathrm{Day}}\, a = \int_b [G b, H(a \times b)]$:
```haskell
{-# LANGUAGE RankNTypes #-}
type DayHom g h a = forall b. g b -> h (a, b)
```

> Sources: DaoFP Exercise 19.1.1.

## Solution 19.1.2

#program — [[DaoFP Chapter 19 Exercises#Exercise 19.1.2|Exercise 19.1.2]]

```haskell
ltor :: (forall a. Day f g a -> h a) -> (forall a. f a -> DayHom g h a)
ltor nat fa = \gb -> nat (Day id fa gb)

rtol :: Functor h => (forall a. f a -> DayHom g h a) -> (forall a. Day f g a -> h a)
rtol nat (Day abx fa gb) = fmap abx (nat fa gb)
```
`ltor` packages `fa` and `gb` into a Day product with the identity combiner; `rtol` applies the curried natural transformation and then maps the combining function over the result.

> Sources: DaoFP Exercise 19.1.2.

## Solution 19.3.1

#program — [[DaoFP Chapter 19 Exercises#Exercise 19.3.1|Exercise 19.3.1]]

```haskell
instance Functor (Ran p f) where
  fmap g (Ran h) = Ran (\k -> h (k . g))     -- k :: b' -> p e, g :: b -> b'
```

> Sources: DaoFP Exercise 19.3.1.

## Solution 19.3.2

#proof — [[DaoFP Chapter 19 Exercises#Exercise 19.3.2|Exercise 19.3.2]]

Whiskering $\sigma$ with $R$: $\sigma \circ R = (\alpha \circ L \circ R) \cdot (G \circ \eta \circ R) : G R \to L R$. Following with the counit $\varepsilon : L R \to \mathrm{Id}$ gives $\varepsilon \cdot (\alpha \circ L R) \cdot (G \circ \eta \circ R)$. By the interchange law, $\varepsilon \cdot (\alpha \circ L R) = \alpha \cdot (G \circ R \varepsilon)$ (slide $\alpha$ past $\varepsilon$: they act on different strings). So we get $\alpha \cdot (G \circ R \varepsilon) \cdot (G \circ \eta R) = \alpha \cdot (G \circ (R\varepsilon \cdot \eta R)) = \alpha \cdot (G \circ \mathrm{id}_R) = \alpha$ by the triangle identity $R \varepsilon \cdot \eta R = \mathrm{id}_R$. In [[String Diagram|string diagrams]]: the zigzag formed by the cup $\eta$ and the cap $\varepsilon$ on the $R$-string is pulled straight.

> Sources: DaoFP Exercise 19.3.2.

## Solution 19.3.3

#program — [[DaoFP Chapter 19 Exercises#Exercise 19.3.3|Exercise 19.3.3]]

```haskell
instance Functor (Codensity f) where
  fmap g (C h) = C (\k -> h (k . g))
```

> Sources: DaoFP Exercise 19.3.3.

## Solution 19.3.4

#program — [[DaoFP Chapter 19 Exercises#Exercise 19.3.4|Exercise 19.3.4]]

```haskell
instance Applicative (Codensity f) where
  pure x = C (\k -> k x)
  C hf <*> C hx = C (\k -> hf (\g -> hx (k . g)))
```
Run the function-producing computation with a continuation that runs the argument computation and feeds `k . g` to it.

> Sources: DaoFP Exercise 19.3.4.

## Solution 19.4.1

#program — [[DaoFP Chapter 19 Exercises#Exercise 19.4.1|Exercise 19.4.1]]

```haskell
instance Functor (Lan p f) where
  fmap g (Lan pe_b fe) = Lan (g . pe_b) fe
```

> Sources: DaoFP Exercise 19.4.1.

## Solution 19.4.2

#program — [[DaoFP Chapter 19 Exercises#Exercise 19.4.2|Exercise 19.4.2]]

```haskell
instance Functor (Density f) where
  fmap g (D fd_c fd) = D (g . fd_c) fd
instance Comonad (Density f) where
  extract (D fd_c fd) = fd_c fd                    -- apply the function to the hidden value
  duplicate (D fd_c fd) = D (D fd_c) fd            -- keep the same hidden f d, delay the application
```
This is the dual of the [[Codensity Monad]]. With `f = Identity`, `Density Identity c ≅ exists d. (d -> c, d) ≅ c` by co-Yoneda, so it collapses to the identity comonad.

> Sources: DaoFP Exercise 19.4.2.
