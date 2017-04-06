---
layout: post
title: 
tags: [idris,dependent types,functional programming,fp,dependently typed]
date: 2017-04-07
---
```idris
data EqStr : (s1:String) -> (s2:String) -> Type where
  Phrase : (s:String) -> EqStr s s 

say : EqStr "Hello World!" "Hello World!" -> IO ()
say _ = printLn "Hello World!"

main : IO ()
main = say ( Phrase "Hello World!" )
```

