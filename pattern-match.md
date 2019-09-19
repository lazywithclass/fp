## Patter match

Here's an example, `length` uses pattern match to recursively arrive to the lenght of a list.

```Haskell
length :: [a] -> Integer 
length []     = 0
lenght [x:xs] = 1 + length xs
```
