---
layout: post
title: Use Scala regex to pattern match simple logical expressions in a string.
tags: [scala,pattern matching,string,regex,logical expression,url,request parameter]
date: 2016-04-24
---
```scala
val filterStr = "39588A~~~FFD0BB"

val Or = "(.*)~{3}(.*)".r
val And = "(.*)_{3}(.*)".r

filterStr match {
  case Or(left,right) => left -> right
  case And(left,right) => left -> right
}
// gives (39588A,FFD0BB)
```

```scala
val filterStr = "39588A~~~FFD0BB"

val || = "(.*)~{3}(.*)".r
val && = "(.*)_{3}(.*)".r

filterStr match {
  case ||(left,right) => left -> right
  case &&(left,right) => left -> right
}
// gives (39588A,FFD0BB)
```

```scala
val filterStr = "39588A~~~FFD0BB"

val || = "(.*)~{3}(.*)".r
val && = "(.*)_{3}(.*)".r

filterStr match {
  case ||(left,right) => left -> right
  case &&(left,right) => left -> right
}
// gives (39588A,FFD0BB)
```

```scala
val filterStr = "39588A~~~FFD0BB"

val || = "(.*)~{3}(.*)".r
val && = "(.*)_{3}(.*)".r

filterStr match {
  case left || right => left -> right
  case left && right => left -> right
}
// gives (39588A,FFD0BB)
```
