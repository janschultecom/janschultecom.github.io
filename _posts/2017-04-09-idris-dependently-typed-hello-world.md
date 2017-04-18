---
layout: post
title: Dependently-typed hello world in Idris
tags: [idris,dependent types,functional programming,fp,dependently typed]
date: 2017-04-09
---
Inspired by the chapter on equality in [Edwin Brady's](https://edwinb.wordpress.com/) amazing book [Type-Driven Development with Idris](https://www.manning.com/books/type-driven-development-with-idris) (which I highly recommend you to read), I wanted to do the thought experiment of creating a [dependently-typed](https://en.wikipedia.org/w/index.php?title=Dependent_type&oldid=774017617) version of *Hello World!* in Idris. The basic idea was to have my hello world program only compile for the exact String *Hello World!* and nothing else. 

So here it is: 
```haskell
data Sentence : String -> Type where
  Phrase : (s:String) -> Sentence s 

say : Sentence "Hello World!" -> IO ()
say _ = printLn "Hello World!"

main : IO ()
main = say ( Phrase "Hello World!" )
```

Now if we are going to change line 18 from "Hello World!" to "Hallo Welt!"...
```haskell
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
```haskell
data Sentence : String -> Type where
```
... declares a data type. Just as a List can take a *type* like Int to create a more specific type List Int, our Sentence data type takes a *value* of type String to create another more specific type. 

Line two declares our only type constructor Phrase. 
```haskell
  Phrase : (s:String) -> Sentence s 
```
The Phrase constructor takes a String to create a type Sentence. Using Phrase and passing it some String is the only way to create a type Sentence.

Line three defines a function that takes a type Sentence "Hello World!" and returns an I/O action. 
```haskell
say : Sentence "Hello World!" -> IO ()
```
Note that we have a String value in our type declaration. 

Line five and six define our actual Hello World! program in Idris. Here we define our main routine to print out Hello World!. It does that by calling the function say with the Phrase "Hello World!" 
```haskell
main : IO ()
main = say ( Phrase "Hello World!" )
```
As described above, changing this line to another String results in a compile-time error.

## Variant - Built-in (=) type

An alternative approach to achieve the same result is to use the built-in (=) type of Idris. 
```haskell
say : (=) "Hello World!" "Hello World!" -> IO ()
say _ = printLn "Hello World!"

main : IO ()
main = say ( Refl { x = "Hello World!" } )
```
Our say function now expects an (=) type with the String *Hello World!*
```haskell
say : (=) "Hello World!" "Hello World!" -> IO ()
```
The only way to create it is by using creating a Refl with the String *Hello World!*.
```haskell
main = say ( Refl { x = "Hello World!" } )
```
We can take this approach one step further by introducing an implicit (=) parameter into our say funcion. 
```haskell
say : (s:String) -> { auto ok : (=) s "Hello World!"} -> IO ()
say s = printLn s

main : IO ()
main = say "Hello World!"
```
Calling the function say requires a String and an implicit proof that the input String is equal to *Hello World!*:
```haskell
say : (s:String) -> { auto ok : (=) s "Hello World!"} -> IO ()
```
The only way the compiler can create this proof (by construcing a Refl) is for the String *Hello World!*. Anything else will fail to compile.

