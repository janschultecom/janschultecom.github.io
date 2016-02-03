---
layout: post
title: Removing META-INF directory from sbt assembly
tags: [scala,sbt]
date: 2014-03-20
---
I recently had a look into sbt assembly, since I had to create a fat .jar for my project. Unfortunately I couln't get it running, getting the following error all the time:

```sh
[info] Merging files...
java.lang.RuntimeException: deduplicate: different file contents found in the following:
<<PATH>>\<<DEPENDENCY>>.jar:META-INF/DEPENDENCIES
<<PATH>>\<<DEPENDENCY>>.jar:META-INF/MANIFEST.MF
```

The problem is, that all dependencies usually contain a META-INF directory with e.g. a DEPENDENCIES or a MANIFEST.MF file. So when sbt assembly tries to merge the different jars and finds already existing files, it's default behaviour is to use a deduplication strategy, which is described as follows<sup>1</sup>:

<pre>If multiple files share the same relative path (e.g. a resource named application.conf in multiple dependency JARs), the default strategy is to verify that all candidates have the same contents and error out otherwise.</pre>

So sbt assembly stops with an error. The default strategy should filter it out:

```scala
case PathList("META-INF", xs @ _*) =>
```

However, it somehow doesn't work. It took me quite some time to figure out how to circumvent this problem, so I want to share the solution with you here:

{% gist 9660937 %}
[gist https://gist.github.com/janschultecom/9660937 /]

<sup>1</sup> [Sbt assembly documentation](https://github.com/sbt/sbt-assembly#Merge Strategy "sbt assembly documentation")
