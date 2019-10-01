## Lifting

```Haskell
class Functor f where
  fmap :: (a -> b) -> (f a -> f b)
```

Note: it helps a lot in understanding what is going on if uses parens to highlight what is going on, in this
case "`fmap` accepts a function from `a` to `b` and returns a function that takes an `f a` and returns an `f b`", which
is slightly different from saying "`fmap` accepts a function from `a` to `b` and an `f a` and returns an `f b`.

If we call `(a -> b)` as `g` then, when we write `fmap g` we say that **`g` has been lifted into that functor**.

https://repl.it/@lazywithclass/book-of-monads-lifting
