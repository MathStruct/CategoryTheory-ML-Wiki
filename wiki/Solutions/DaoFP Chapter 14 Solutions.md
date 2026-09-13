#solution

Solutions to the exercises of DaoFP, Chapter 14: [[DaoFP Chapter 14 Exercises]]. Index: [[Map of Content]].

## Solution 14.4.1

#program — [[DaoFP Chapter 14 Exercises#Exercise 14.4.1|Exercise 14.4.1]]

```haskell
newtype E e a = E (e -> Maybe a)
runE :: E e a -> e -> Maybe a
runE (E f) e = f e
instance Functor (E e) where
  fmap f (E g) = E (fmap f . g)
instance Applicative (E e) where
  pure a = E (\_ -> Just a)
  ef <*> ea = ef >>= \f -> fmap f ea
instance Monad (E e) where
  ma >>= k = E (\e -> case runE ma e of
                        Nothing -> Nothing
                        Just a  -> runE (k a) e)
```
The environment is passed to both stages (reader) and failure short-circuits (maybe): `E e = ReaderT e Maybe` ([[Monad Transformer]]).

> Sources: DaoFP Exercise 14.4.1.

## Solution 14.5.1

#program — [[DaoFP Chapter 14 Exercises#Exercise 14.5.1|Exercise 14.5.1]]

```haskell
ap :: Monad m => m (a -> b) -> m a -> m b
ap fs as = do
  f <- fs
  a <- as
  return (f a)
```
This is the splat of the [[Applicative Functor]] derived from any monad.

> Sources: DaoFP Exercise 14.5.1.

## Solution 14.5.2

#program — [[DaoFP Chapter 14 Exercises#Exercise 14.5.2|Exercise 14.5.2]]

```haskell
pairs :: [a] -> [b] -> [(a, b)]
pairs as bs = as >>= \a -> bs >>= \b -> return (a, b)
-- = concat (fmap (\a -> fmap (\b -> (a, b)) bs) as)
```

> Sources: DaoFP Exercise 14.5.2.

## Solution 14.8.1

#program — [[DaoFP Chapter 14 Exercises#Exercise 14.8.1|Exercise 14.8.1]]

```haskell
toFree :: Rose a -> FreeMonad [] a
toFree (Leaf a)  = Pure a
toFree (Rose rs) = Free (fmap toFree rs)

fromFree :: FreeMonad [] a -> Rose a
fromFree (Pure a)   = Leaf a
fromFree (Free frs) = Rose (fmap fromFree frs)
```
`Pure` ↔ `Leaf`, `Free` ↔ `Rose`: the free monad on the list functor *is* the type of rose trees.

> Sources: DaoFP Exercise 14.8.1.

## Solution 14.8.2

#program — [[DaoFP Chapter 14 Exercises#Exercise 14.8.2|Exercise 14.8.2]]

```haskell
data Bin a = Bin a a deriving Functor
data BTree a = BLeaf a | BNode (BTree a) (BTree a)

toFreeB :: BTree a -> FreeMonad Bin a
toFreeB (BLeaf a)   = Pure a
toFreeB (BNode l r) = Free (Bin (toFreeB l) (toFreeB r))

fromFreeB :: FreeMonad Bin a -> BTree a
fromFreeB (Pure a)          = BLeaf a
fromFreeB (Free (Bin l r))  = BNode (fromFreeB l) (fromFreeB r)
```

> Sources: DaoFP Exercise 14.8.2.

## Solution 14.8.3

#program — [[DaoFP Chapter 14 Exercises#Exercise 14.8.3|Exercise 14.8.3]]

```haskell
newtype Const c a = Const { getConst :: c } deriving Functor

showAlg :: MAlg StackF (Const String) a
showAlg = (stop, go)
  where
    stop _ = Const "return\n"
    go (Push n k) = Const ("push " ++ show n ++ "\n" ++ getConst k)
    go (Pop k)    = Const ("pop\n" ++ getConst k)
    go (Add k)    = Const ("add\n" ++ getConst k)
    go (Top ik)   = Const ("top\n" ++ getConst (ik 0))   -- one representative branch

pretty :: FreeMonad StackF a -> String
pretty = getConst . mcata showAlg
```
`Top` has a branch for every `Int`; the printer follows one representative (`ik 0`). The same program `calc` is thus interpreted by two different algebras — execution and printing.

> Sources: DaoFP Exercise 14.8.3.

## Solution 14.9.1

#program — [[DaoFP Chapter 14 Exercises#Exercise 14.9.1|Exercise 14.9.1]]

```haskell
instance Monoidal [] where
  unit = [()]
  as >*< bs = [ (a, b) | a <- as, b <- bs ]     -- cartesian product
```
(The zip version `zip as bs` with `unit = repeat ()` is another lax monoidal structure.)

> Sources: DaoFP Exercise 14.9.1.

## Solution 14.9.2

#program — [[DaoFP Chapter 14 Exercises#Exercise 14.9.2|Exercise 14.9.2]]

```haskell
liftA3 :: Applicative f => (a -> b -> c -> d) -> f a -> f b -> f c -> f d
liftA3 g as bs cs = g <$> as <*> bs <*> cs
```

> Sources: DaoFP Exercise 14.9.2.

## Solution 14.9.3

#proof — [[DaoFP Chapter 14 Exercises#Exercise 14.9.3|Exercise 14.9.3]]

- *Identity*: `zipWith ($) (repeat id) v = map id v = v`.
- *Homomorphism*: `zipWith ($) (repeat f) (repeat x) = repeat (f x) = pure (f x)`.
- *Interchange*: `zipWith ($) u (repeat y) = map ($ y) u = zipWith ($) (repeat ($ y)) u`.
- *Composition*: `zipWith ($) (zipWith ($) (zipWith ($) (repeat (.)) u) v) w` zips position-wise to `[(u_i . v_i) w_i]`, and `zipWith ($) u (zipWith ($) v w) = [u_i (v_i w_i)]`; these agree, and both are truncated to the shortest list.

So `ZipList` is a lawful applicative, though not a monad (there is no `join` compatible with `repeat`).

> Sources: DaoFP Exercise 14.9.3.
