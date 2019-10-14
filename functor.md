## Functor

(explanations took from [Book Of Monads](https://www.amazon.com/Book-Monads-practice-applied-problems-ebook/dp/B07JNZHYLT))

Functors encompass the simplest notion of a "magic box", that we can only inspect by means of function but never
unwrap or generate anew.

```Haskell
class Functor f where
  fmap :: (a -> b) -> f a -> f b
```

or

```Haskell
fmap :: Functor f => (a -> b) -> (f a -> f b)
```

which highlights that you can transform a function `g` (`(a -> b)`) into one that operates on elements
contained by the functor, so for example

```Haskell
addOne :: Int -> Int
addOne x = x + 1 
```

could be applied to arrays or `Maybe`s by lifting it like so `fmap (addone) [1]`, `fmap (Just) 1`. Of course that 
does not work for `fmap (addOne) 1`, because `1` is not contained in a "box".

https://repl.it/@lazywithclass/book-of-monads-functor

Every monad is also a functor, so by using monads, we also gain the capabilities of functors.

Let's see it with some examples (credit: http://adit.io/posts/2013-04-17-functors,_applicatives,_and_monads_in_pictures.html)

> A Functor is any data type that defines how fmap applies to it.

```Haskell
fmap (+3) (Just 2) -- Just 5

-- or

instance Functor Maybe where
  fmap func (Just val) = Just (func val)
  fmap func Nothing = Nothing

-- or (function composition)

instance Functor ((->) r) where
  fmap f g = f . g

-- or (lists are functors!)

instance Functor [] where
  fmap = map
```
