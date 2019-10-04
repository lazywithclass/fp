## Lifting

Apply a function to whatever is inside container `f`.

```Haskell
class Functor f where
  fmap :: (a -> b) -> (f a -> f b)
```

Note: it helps a lot in understanding what is going on if uses parens to highlight what is going on, in this
case "`fmap` accepts a function from `a` to `b` and returns a function that takes an `f a` and returns an `f b`", which
is slightly different from saying "`fmap` accepts a function from `a` to `b` and an `f a` and returns an `f b`.

If we call `(a -> b)` as `g` then, when we write `fmap g` we say that **`g` has been lifted into that functor**.

For example we could lift `mysum` as follows 

```Haskell
lift2 :: Monad m => (a -> b -> c) -> m a -> m b -> m c
lift2 f x y = do a <- x
                 b <- y
                 return (f a b)

mysum :: Int -> Int -> Int
mysum x y = x + y

main = print (lift2 mysum (Just 41) (Just 1))
```

but then we would have to write a `liftN` function for each arity, instead we could use the following

```Haskell
-- using fmap and ap to lift functions with any arity

ap :: Monad m => m (b -> c) -> m b -> m c
ap mf x = do f <- mf
             b <- x
             return (f b)

-- and this is fmap's type, extracted typing
-- :type fmap
-- in the repl
-- fmap :: Functor f => (a -> b) -> f a -> f b

-- let's try with arity 2
mysum2 :: Int -> Int -> Int
mysum2 a b = a + b

-- uncomment this and comment the other main to see
-- it working for arity 2
-- main = print (fmap mysum2 (Just 1) `ap` (Just 1))

-- but also with arity 3 it will work
mysum3 :: Int -> Int -> Int -> Int
mysum3 a b c = a + b + c

main = print (fmap mysum3 (Just 1) `ap` (Just 1) `ap` (Just 1))
```

https://repl.it/@lazywithclass/book-of-monads-lifting


Why does this work? (legit question I don't know at the time I am writing this words)

I'll try reasoning using types, so we have

```Haskell
fmap mysum3 (Just 1) `ap` (Just 1)
```

which has these types if we decompose it in smaller pieces (I'm using `:type` to help me)

```Haskell
-- fmap mysum3 has type
fmap mysum3 :: Functor f => f Int -> f (Int -> Int -> Int)
```

so it's giving me something that accepts an `Int` wrapped in a `Functor` and gives back a function
that takes two `Int`s and returns an `Int`, wrapped in a `Functor`; so basically all it does is wrapping `mysum3`
in `Functor`s.
