---
layout: post
title: Dependently-typed hello world in Idris
tags: [idris,dependent types,functional programming,fp,dependently typed]
date: 2017-04-07
---
Lately I have been playing around with the dependently-typed general-purpose programming language [Idris](https://www.idris-lang.org/). Without going into details about dependent types, let me briefly explain what these are. As [Wikipedia](https://en.wikipedia.org/w/index.php?title=Dependent_type&oldid=774017617) states it, 
> [...] a dependent type is a type whose definition depends on a value.

To give you an example: In Java you can parameterize a ```Vector<?>``` with some type like ```Integer``` to create a more specific type ```Vector<Integer>```. The compilet will complain if you pass a String into your vector. In Idris, you can go one step further and create types from values as well. E.g. you can create a ```Vector 5 Int``` to declare a Vector of type Int that has the length 5. The compiler will complain not only complain if you pass a String to it, but also if it has 4 or 6 elements. 

Inspired by the chapter on equality in [Edwin Brady's](https://edwinb.wordpress.com/) amazing book [Type-Driven Development with Idris](https://www.manning.com/books/type-driven-development-with-idris) (which I highly recommend you to read), I wanted to create a dependently-typed version of Hello World! in Idris. My idea was to have it only compile for the String Hello World!. 

So this is it: 
```idris
data Sentence : String -> Type where
  Phrase : (s:String) -> Sentence s 

say : Sentence "Hello World!" -> IO ()
say _ = printLn "Hello World!"

main : IO ()
main = say ( Phrase "Hello World!" )
```

Now if we are going to write any other String than "Hello World!" like "Hallo Welt!"...
```idris
main = say ( Phrase "Hallo Welt!" )
```
... we get a compile-time error:
```bash
When checking argument s to constructor Main.Phrase:
        Type mismatch between
                "Hello World!" (Inferred value)
        and
                "Hallo Welt!" (Given value)
```

## Explanation

Let's analyse the different steps. The first line ...
```idris
data Sentence : String -> Type where
```
... declares a data type. Just as a List can take a *type* like Int to create a more specific type List Int, our Sentence data type takes a *value* of type String to create another more specific type. 

Line two declares our only type constructor Phrase. 
```idris
  Phrase : (s:String) -> Sentence s 
```
The Phrase constructor takes a String and to create a type Sentence. Using Phrase and passing it some String is the only way to create a type Sentence.

Line three defines a function that takes a type Sentence "Hello World!" and returns an I/O action. 
```idris
say : Sentence "Hello World!" -> IO ()
```
Note that we have a String value in our type declaration. 

Line five and six define our actual Hello World! program in Idris. Here we define our main routine to print out Hello World!. It does that by calling the funcntion say with the Phrase "Hello World!" 
```idris
main : IO ()
main = say ( Phrase "Hello World!" )
```


## Variant 1 - Built-in (=) type

```idris
say : (=) "Hello World!" "Hello World!" -> IO ()
say _ = printLn "Hello World!"

main : IO ()
main = say $ Refl { x = "Hello World!" } 
```

## Variant 2 - Built-in (=) type with auto implicits
```idris
say : (s:String) -> { auto ok : (=) s "Hello World!"} -> IO ()
say s = printLn s

main : IO ()
main = say "Hello World!"
```
