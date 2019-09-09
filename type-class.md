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

```Haskell
instance Eq ((a, b), (c, d)) where
  (==) a c && (==) b d = True
  _                    = False
```

https://repl.it/@lazywithclass/MagnificentRespectfulKilobyte