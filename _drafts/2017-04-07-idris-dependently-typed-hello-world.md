---
layout: post
title: Dependently-typed hello world in Idris
tags: [idris,dependent types,functional programming,fp,dependently typed]
date: 2017-04-07
---

```idris
data Sentence : (s:String) -> Type where
  Phrase : (s:String) -> Sentence s 

say : Sentence "Hello World!" -> IO ()
say _ = printLn "Hello World!"

main : IO ()
main = say ( Phrase "Hello World!" )
```

