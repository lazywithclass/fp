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
                           (r', i2) = relabel r i1  -- 4)
                       in  (Node l' r', i2)
```

Since I'm very new to Haskell I will try to write down what happens in each line, moving away from
the goal, which is to understand why this example is useful to understand when monads shine.

1) `relabel` is a function that takes a `Tree` containing items of type `a` (`a` is a
[type variable](http://www.learnyouahaskell.com/types-and-typeclasses#type-variables) and an `Int`,
and returns a `Tree` which now has a tuple consisting of an `Int` and an `a` instead of just an `a`
(exactly what we wanted)

2) pattern matching on the `Tree`, if we've found a `Leaf` then I create one the way I want it, so
the new label, `i`, and I return it in a tuple along with the next label to be used
The data structure of `Tree` is `data Tree a = Leaf a | Node (Tree a) (Tree a)`

3 - 4) in the case of a `Node`, so we have left and right to check, we use destructuring and recurse
on the rest of the data structure

### Working towards a cleaner solution

Dealing with the index is error prone, so we isolate it in a new type

```Haskell
type WithCounter a = Int -> (a, Int)
```

so `relabel`'s type becomes `Tree a -> WithCounter (Tree a)`, (`Tree a -> Int -> ((Tree a), Int)` is how it
would look after the subsitution I suppose).

Next, isolate `let` expression nesting

```Haskell
next :: WithCounter a -> (a -> WithCounte b) -> WithCounter b
f `next` f = \i -> let (r, i') = f i in g r i'
```

The second thing is returning a value with the counter unchanged

```Haskell
pure :: a -> WithCounter a
pure x = \i -> (x, i)
```

so that `relabel` becomes

```Haskell
relabel :: Tree a -> WithCounter a
relabel (Leaf x)   i = \i -> (Leaf (i, x), i + 1)
relabel (Node l r) i = relabel l `next` \l' ->
                       relabel r `next` \r' ->
                       pure (Node l' r')
```

We can maintain a state with a type alias like

```Haskell
type state s a = s -> (a, s)
```

where `s` is the state and `a` the type variable.

> Exercise 1.1 Rewrite the definitions of `pure` and `next` to work with an arbitrary 
stateful computation `State s a`. Hint: you only need to change the type signatures.

```Haskell
pure :: a -> State s a
pure x = \i -> (x, i)

next :: State s a -> (a -> State s b) -> State s b
f `next` g = \i -> let (r, i') = f i in g r i'
```

> Exercise 1.2 Write a function (`++`) that takes two lists and return its concatenation.
That is, given two lists `l1` and `l2`, `l1 ++ l2` contains the elements of `l1` followed
by the elements of `l2`

https://repl.it/@lazywithclass/book-of-monads-ex-12

```Haskell
(++) :: [a] -> [a] -> [a]
[] ++ ys = ys
(x:xs) ++ ys = x : xs ++ ys
```

> Exercise 1.3 Do you remember how `map` is defined? Try it out!

```Haskell
map :: (a -> b) -> [a] -> [b]
map f []     = []
map f (x:xs) = f x : map f xs
```

Getting closer to the definition...

```Haskell
singleton :: a -> [a]
singleton x = [x]

concat :: [[a]] -> [a]
concat []     = []
concat (x:xs) = x ++ concat xs
```

A list is a box that supports three operations:

 * `map`ping a funciton over all the elements contained in it
 * creating a box from a single element
 * flattening nested boxes of boxes into a single layer

There is an analogy with the `Maybe` type, it is possible to:

 * `map`ping over it to make changes
```Haskell
map :: (a -> b) -> Maybe a -> Maybe b
map f Nothing  = Nothing
map f (Just x) = Just (f x)
```
 * create a `Maybe` from a single element
```Haskell
singleton :: a -> Maybe a
singleton = Just
```
 * flattening boxes inside other boxes
```Haskell
flatten :: Maybe (Maybe a) -> Maybe a
flatten (Just (Just x)) = Just x
flatten _               = Nothing
```

