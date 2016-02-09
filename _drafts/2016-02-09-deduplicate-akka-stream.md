##TL;DR
Deduplicate an akka stream using the [akvokolekta](http://github.com/janschultecom/akvokolekta) akka stream additions:
```scala
// add Sonatype snapshot repository and akvokolekta library to your build.sbt
resolvers += "Sonatype OSS Snapshots" at "https://oss.sonatype.org/content/repositories/snapshots"
libraryDependencies += "com.janschulte" %% "akvokolekta" % "0.1.0-SNAPSHOT"
```
Deduplicate the akka stream:
```scala
import com.janschulte.akvokolekta.StreamAdditions._

// scala dsl
val deduplicatedSource = Source(List(1, 2, 3, 4, 1, 2, 3, 4)).deduplicate()
val deduplicatedFlow = Flow[Int].deduplicate()

// graph api
import com.janschulte.akvokolekta.graph.Deduplicator
import GraphDSL.Implicits._

val partial = GraphDSL.create() { implicit builder =>
  val source = builder.add(Broadcast[Int](1))
  val dedup = builder.add(Deduplicator[Int]())
  val sink = builder.add(Merge[Int](1))

  source ~> dedup ~> sink

  FlowShape(source.in, sink.out)
}
```

## Stream deduplication

## Akvokolekta

## Caveats