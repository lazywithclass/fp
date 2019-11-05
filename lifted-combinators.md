## Lifted combinators

(explanations and exercises took from [Book Of Monads](https://www.amazon.com/Book-Monads-practice-applied-problems-ebook/dp/B07JNZHYLT))

> Exercise 4.1 Write implementations for `zipWithM` and `replicateM` in terms of their pure counterparts `zipWith` and `replicate` composed with `sequence`. What goes wrong when you try to do the same with `filterM`?

```Haskell
zipWithM :: Monad m => (a -> b -> m c) -> [a] -> [b] -> m [c]
zipWithM f as bs = sequence' (zipWith f as bs)

-- replicate has type
-- replicate :: Int -> a -> [a]
replicateM :: Monad m => Int -> m a -> m [a]
replicateM n ma = sequence' (replicate n ma)

-- filter has type
-- filter :: (a -> Bool) -> [a] -> [a]
--filterM :: Monad m => (a -> m Bool) -> [a] -> m [a]
--filterM f as = sequence' (filter f as)
-- I don't know why it does not work :(
```

> A pure expression has no side effects. It just compoutes its output value. In contrast, monadic
actions are executed for both their side effects and their resulting values.

