## Monad

(explanations and exercises took from [Book Of Monads](https://www.amazon.com/Book-Monads-practice-applied-problems-ebook/dp/B07JNZHYLT))

Context. Box.

Relabeling tree leaves, left to right, like so

```
        *
        
    *      z
    
 x      y

```

```
           *
        
       *      (3, z)

(1, x)   (2, y)

```

```Haskell
relabel :: Tree a -> Int -> (Tree (Int, a), Int)    -- 1)
relabel (Leaf x)   i = (Leaf (i, x), i+1)           -- 2)
relabel (Node l r) i = let (l', i1) = relabel l i   -- 3)
                           (r', i2) = relabel r i1  -- 3)
                       in  (Node l' r', i2)
```

Since I'm very new to Haskell I will try to write down what happens in each line, moving away from
the goal, which is to understand why this example is useful to understand when monads shine.

1) `relabel` is a function that takes a `Tree` containing items of type `a` (`a` is a
[type variable](http://www.learnyouahaskell.com/types-and-typeclasses#type-variables) and an `Int`,
and returns a `Tree` which now has a tuple consisting of an `Int` and an `a` instead of just an `a`
(exactly what we wanted)

2) pattern matching on the `Tree`, if we've found a `Leaf` then -- I dont understand what's happening 