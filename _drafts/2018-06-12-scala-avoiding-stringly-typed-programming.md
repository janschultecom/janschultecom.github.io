---
layout: post
title: Six ways to avoid stringly typed programming in Scala
tags: [scala,newtypes,opaque types,opaque types,tagged types]
date: 2018-06-12
---
# tl;dr

# the problem

# solution I - type alias
Type aliases are the simplest form to avoid stringly typed programming and basically self-explanatory. You just create an alias for a type, which you use throughout your code:
{% scalafiddle %}
```scala
type Email = String

def send(email:Email):Unit = println(s"sending email to $email")

val email: Email = "test@example.com"

send(email)
// sending email to test@example.com
```
{% endscalafiddle %}
There is a catch though, type aliases provide no guarantee whatsoever - you can still pass any other String.
```scala
type Email = String

def send(email:Email):Unit = println(s"sending email to $email")

val email: Email = "test@example.com"

send(email)
// sending email to test@example.com

type Name = String
val name: Name = "Jan"

send(name)
// sending email to Jan
```

### Discussion
+ Easy to use
- No real guarantee
- Read or refactor unknown/legacy code base

# the basic - wrapper class

```scala
case class Username(unwrap:String)
val username = Username("max")
username.unwrap
```

Stephen Compall has written an excellent article about the [high cost of `AnyVal` subclasses](https://failex.blogspot.com/2017/04/the-high-cost-of-anyval-subclasses.html).

# newtype macro
[newtype macro](https://github.com/estatico/scala-newtype)

# tagged types
Miles Sabin's amazing [shapeless library](https://github.com/milessabin/shapeless) provides an implementation for tagged types. If you don't want to depend on shapeless, you can also easily create your own tagged type implementation, as it is rather trivial.

# the future - opaque types
https://docs.scala-lang.org/sips/opaque-types.html
in Dotty a.k.a Scala 3

# the mighty - refinement types
