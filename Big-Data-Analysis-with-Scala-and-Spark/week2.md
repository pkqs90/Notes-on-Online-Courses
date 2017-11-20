[TOC]

## Reduction Operations

Generally, reduction operaitons walk through a collection and combine neighboring elements of the collection together to produce a single combined result (rather than another collection).



**foldLeft `def foldLeft[B](z: B)(func: (B, A) => B): B`**

1. Not parallelizable.
2. **Exists in scala, but not in spark (since doing things serially is difficult)**.



**fold `def fold(z: A)(func: (A, A) => A): A`**

1. Parallelizable
2. Can’t change the return type.



**aggregate `def aggregate[B](z: => B)(seqop: (B, A) => B, comob: (B, B) => B): B`**

1. Parallelizable
2. Possible to change the return type.



In fact, in Spark, *aggregate* is a more desirable reduction operator a majority of the time. Because much of the time when working with large-scale data, our goal is to **project down from larger/more complex data types**.



## Pair RDDs

In short: Pair RDDs are **distributed key-value pairs**.

Why? Because is it very common in world of big data processing to **operate on data in the form of key-value pairs (pattern from mapreduce)**.

Useful because: Pair RDDs allow you to act on each key in parallel or regroup data across the network.



## Transformations and Actions on Pair RDDs

**groupByKey `def groupByKey(): RDD[(K, Iterable[V])]`**

1. Transformation
2. Groups all values that have the same key.



**reduceByKey `def reduceByKey(func: (V, V) => V): RDD[(K, V)]`**

1. Transformation.
2. Can be thought of as a combination of groupByKey and reduce-ing on all the values per key. But it’s more efficient though, than using separately.



**mapValues `def mapValues[U](func: V => U): RDD[(K, U)]`**

1. Transformation
2. Doing map operations on pair RDDs.



**keys `def keys: RDD[K]`**

1. Transformation
2. Return an RDD with the keys for each tuple.



**countByKey `def countByKey(): Map[K, Long]`**

1. **Action (important!)**
2. Counts the number of elements per key in a Pair RDD, returning a normal Scala Map mapping from keys to counts.



## Joins

**join `def join[W](other: RDD[(K, W)]): RDD[(K, (V, W))]`**

1. Transformation
2. Returns a new RDD containing combined pairs whose keys are present in both input RDDs (inner join).



**leftOuterJoin `def leftOuterJoin[W](other: RDD[(K, W)]): RDD[(K, (V, Option[W]))]`**

1. Transformation
2. Returns a new RDD containing combined pairs whose keys don’t have to be present in both input RDDs.
3. If the key doesn’t exist in the second RDD, a `None` will be used.



**rightOuterJoin `def rightOuterJoin[W](other: RDD[(K, W)]): RDD[(K, (Option[V], W))]`**

1. Transformation
2. Returns a new RDD containing combined pairs whose keys don’t have to be present in both input RDDs.
3. If the key doesn’t exist in the first RDD, a `None` will be used.