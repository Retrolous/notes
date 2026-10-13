---
title:  Advice from the PPTs
---


### - DO NOT USE MAGIC NUMBERS

As an example, see the code below:
``` haskell
alphaChr :: Int -> Char
alphaChr n
 | n < 0 || n > 25 = error "Not within range for alphabetic characters (1-26)"
 | otherwise = chr (ord ’a’ + n)

(<+>) :: Char -> Char -> Char
a <+> b = alphaChr ((alphaOrd a + alphaOrd b) ‘mod‘ 26)

(<->) :: Char -> Char -> Char
a <-> b = alphaChr ((alphaOrd a - alphaOrd b) ‘mod‘ 26)
```
Sure, within the context of the functions (and the fact that they deal with the alphabet) the numbers 0, 25 and 26 may make sense. However, this is still an unexplained and hardcoded numerical value in source code. 

As such we would be better off using named constants or obvious method calls.

For example, we could use a constant
``` haskell 
noOfLettersInAlphabet :: Int
noOfLettersInAlphabet = 26
```

...and substitute it into our code like so:

``` haskell
alphaChr :: Int -> Char
alphaChr n
 | n < 0 || n > (noOfLettersInAlphabet - 1) = error "Not within range for alphabetic characters (1-26)"
 | otherwise = chr (ord ’a’ + n)

(<+>) :: Char -> Char -> Char
a <+> b = alphaChr ((alphaOrd a + alphaOrd b) ‘mod‘ noOfLettersInAlphabet)

(<->) :: Char -> Char -> Char
a <-> b = alphaChr ((alphaOrd a - alphaOrd b) ‘mod‘ noOfLettersInAlphabet)
```

Yes, this may seem superfluous, but for more complex examples it will contribute significantly to the maintainability and readability of your code.

### - TEST CASES ARE NOT COMPREHENSIVE; ADD YOUR OWN

Self-explanatory. Despite the resounding full marks given when you run `cabal test`, there may be edge cases that have not been accounted for (think n = 0, n is negative when no constraint has been given on n, etc.) 

Ultimately, the test cases you write depend on the specific function, so spend more time thinking about how you can break more complex functions.

### - USE PATTERN-MATCHING INSTEAD OF GUARDS

Consider the code:
``` haskell 
geometricSeries :: Double -> Double -> Int -> Double
geometricSeries a r n
 | r == 1 = a * (fromIntegral n + 1)
 | otherwise = a * (1 - r ^ (n + 1)) / (1 - r)
 ```

While it works, it doesn't seem overly clean, isn't as readable as could be, and there is room for error (consider we write the wrong comparison.)

Instead, we can have

``` haskell 
geometricSeries :: Double -> Double -> Int -> Double
geometricSeries a 1 n = a * (fromIntegral n + 1)
geometricSeries a r n = (1 - r ^ (n + 1)) / (1 - r)
```
Obviously not the same kind of thinking as with imperative languages, but much cleaner.

