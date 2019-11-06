## Applicative

(explanations and exercises took from [Book Of Monads](https://www.amazon.com/Book-Monads-practice-applied-problems-ebook/dp/B07JNZHYLT))

An improvement upon Functor, an Applicative deals with a value *and* a function wrapped in a context.

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

main = print (fmap' (\x -> x + 1) (Just 41))
```

https://repl.it/@lazywithclass/book-of-monads-exercise-32

> `Applicative` pushes `Functor` aside. "Big boys can use functions with any number of arguments," it says. "Armed `<$>` and `<*>`, I can take any function that expects any number of unwrapped values. Then I pass it all wrapped values, and I get a wrapped value out! AHAHAHAHAH!"

```Haskell
> (+) <$> (Just 5)
Just (+5)
> Just (+5) <*> (Just 3)
Just 8
```

Where `<$>` is `fmap`.

Source: http://adit.io/posts/2013-04-17-functors,_applicatives,_and_monads_in_pictures.html#applicatives

Whenever we want to lift a pure (non monadic) function in a monadic context we "lift", so this applicative style has the form

```Haskell
fmap f x1 `ap` x2 `ap1` ... `ap` xN
-- or by using Haskell strange symbols
f <$> x1 <*> x2 <*> ... <*> nN
```
