## Applicative

(explanations and exercises took from [Book Of Monads](https://www.amazon.com/Book-Monads-practice-applied-problems-ebook/dp/B07JNZHYLT))

TODO What is an applicative?

```Haskell
class Functor f => Applicative f where
  pure  :: a -> f a
  (<*>) :: f (a -> b) -> f a -> f b
```

> Exercise 3.2 You do not need `fmap` at all in `Applicative`. Write `fmap` as a combination
of `pure` and `ap`. Hint: if `f` is a function with type `a -> b` then `pure f` has the type `m (a -> b)`.

```Haskell
-- write fmap as a combination of pure and ap

ap :: Monad m => m (b -> c) -> m b -> m c
ap mf x = do 
  f <- mf
  b <- x
  return (f b)

-- pure has type
-- pure :: Applicative f => a -> f a

fmap' :: Monad f => (a -> b) -> f a -> f b
fmap' f ma = pure f `ap` ma

main = print 42
```

TODO print something that makes sense, to show that the code it's actually working

https://repl.it/@lazywithclass/book-of-monads-exercise-32
