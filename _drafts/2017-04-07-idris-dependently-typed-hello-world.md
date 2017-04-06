---
layout: post
title: Dependently-typed hello world in Idris
tags: [idris,dependent types,functional programming,fp,dependently typed]
date: 2017-04-07
---

```idris
data Sentence : String -> Type where
  Phrase : (s:String) -> Sentence s 

say : Sentence "Hello World!" -> IO ()
say _ = printLn "Hello World!"

main : IO ()
main = say ( Phrase "Hello World!" )
```
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

Line five and six define our main routine that calls say with our Phrase "Hello World!" 
```idris
main : IO ()
main = say ( Phrase "Hello World!" )
```

Now if we are going to write any other String than "Hello World!" ...
```idris
main = say ( Phrase "Hallo Welt!" )
```
... we get a compile-time error. 
```bash
When checking argument s to constructor Main.Phrase:
        Type mismatch between
                "Hello World!" (Inferred value)
        and
                "Hallo Welt!" (Given value)
```

# Variant 1 - Built-in (=) type

```idris
say : (=) "Hello World!" "Hello World!" -> IO ()
say _ = printLn "Hello World!"

main : IO ()
main = say $ Refl { x = "Hello World!" } 
```

# Variant 2 - Built-in (=) type with auto implicits
```idris
say : (s:String) -> { auto ok : (=) s "Hello World!"} -> IO ()
say s = printLn s

main : IO ()
main = say "Hello World!"
```
