## Type class

(explanations and exercises took from [Book Of Monads](https://www.amazon.com/Book-Monads-practice-applied-problems-ebook/dp/B07JNZHYLT))

A way to abstract common functionality.

```Haskell
class Eq a where
  (==) :: a -> a -> Bool
```

Few things to be said

 * that's a type class
 * `a` is a type parameter, refers to any possible instance that could be used in the methods
 * both `a`s in the signature refer to the same thing

Then you define an `instance` that works over the `Eq` type

```Haskell
instance Eq Bool where
  True  == True  = True
  False == False = True
  _     == _     = False
```

What before was `a` is now `Bool`.

> Exercise 0.2 Define the `Eq` instance for tuples `(a, b)`. In case you need more than one
prerequisite in the declaration, the syntax is: `instance (Prereq1, Prereq2), ...) => Eq (a, b)`

This is my solution, but I'm not really sure it's correct, I also get a "Pattern match is
redundant" warning that puzzles me:

```Haskell
instance (Num a, Num b) => Equ (a, b) where
  a == b = True
  _ == _ = False
```

[repl.it](https://repl.it/@lazywithclass/MagnificentRespectfulKilobyte)

```Haskell
class Container c where
  empty  :: c a
  insert :: a -> c a -> c a
```

> Exercise 0.5 Write the List instance for the `Container` type class

```Haskell
instance Container [] where
  empty       = []
  insert x xs = xs:x
  -- or also insert = (:)
```