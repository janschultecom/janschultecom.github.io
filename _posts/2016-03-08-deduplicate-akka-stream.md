## TL;DR
Sometimes it's necessary to deduplicate your akka stream. Currently, akka streams currently does not yet provide a built-in functionality to filter duplicates. The small  [akvokolekta](http://github.com/janschultecom/akvokolekta) library adds this functionality<sup>1</sup> to the akka streams library.

Add Sonatype snapshot repository and akvokolekta library to your build.sbt
```scala
resolvers += Resolver.sonatypeRepo("snapshots")
libraryDependencies += "com.janschulte" %% "akvokolekta" % "0.1.0-SNAPSHOT"
```
Use deduplication in your stream
```scala
// import akvokolekta implicits
import com.janschulte.akvokolekta.StreamAdditions._

// scala dsl
val deduplicatedSource = Source(List(1, 2, 3, 4, 1, 2, 3, 4)).deduplicate()
val deduplicatedFlow = Flow[Int].deduplicate()

// graph api
import com.janschulte.akvokolekta.graph.Deduplicator
import GraphDSL.Implicits._

// within your flow building
val source = ...
val dedup = builder.add(Deduplicator[Int]())
val sink = ...

source ~> dedup ~> sink
```

## Stream deduplication
Deduplicating a stream requires tracking all elements as they pass through the stream. The easiest approach is to have a set of all the elements that have passed through the stream. When a new element arrives, it is checked for containment in the set. If so, it is discarded because it's a duplicate. If not, its a new element, let it pass and add it to the set.
Needless to say that this approach will sooner or later run out of memory if the number of elements continues to grow.

### Algorithms
The solution to this problem is to use sub-linear space algorithms, that is algorithms that "process the input stream using a small amount of space"<sup>1</sup>. The algorithm (or data structure) for filtering duplicates out of our stream is the [**bloom filter**](https://en.wikipedia.org/w/index.php?title=Bloom_filter&oldid=704138885) . From Wikipedia: "A Bloom filter is a space-efficient probabilistic data structure [...] that is used to test whether an element is a member of a set."<sup>2</sup> A simple explanation of the algorithm can be found directly on the [wikipedia page](https://en.wikipedia.org/wiki/Bloom_filter#Algorithm description), so I save myself from repeating the millionth bloom filter explanation. The thing to know is, that a bloom filter uses a bit array and a set of hash functions to track duplicates, which consumes far less memory than storing a set with all observed elements. This probabilistic data structure however comes with a trade-off: The risk of *false positives*.

The caveat to bloom filter is, that once an element has been added to the filter, it cannot be deleted anymore<sup>3</sup>. A newer and more sophisticated approach is the Cuckoo filter, which supports deletion and is even more efficient<sup>3</sup>.

## Akvokolekta
As akka streams (up until now) does not yet contain a built-in bloom or cuckoo filter for memory-bounded stream deduplication, I wrote a small library to add basic stream processing functionality on top of akka streams that is currently missing.

Using implicit class conversion a deduplication method is added to the akka streams scala dsl (see code above). For the graph api, I have created a Deduplicator building block that can be used within the flow builder.

The source code of akvokolekta can be found on [github](https://github.com/janschultecom/akvokolekta) and is published on sonatype. Akvokolekta basically is a project for me to learn more about memory-bounded, probabilistic streaming algorithms, but I am always happy about getting feedback and hearing from people actually using it. Currently, akvokolekta contains functionality for deduplication<sup>4</sup>, counting distinct elements and set counting operations<sup>5</sup>.


<sup>1</sup> amongst other stream algorithms

<sup>2</sup> http://www.cs.dartmouth.edu/~ac/Teach/CS49-Fall11/Notes/lecnotes.pdf

<sup>3</sup> https://www.cs.cmu.edu/~dga/papers/cuckoo-conext2014.pdf

<sup>4</sup> using ScalaNLP

<sup>5</sup> using Yahoo data sketches
