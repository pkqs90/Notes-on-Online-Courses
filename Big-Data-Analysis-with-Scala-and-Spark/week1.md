## **Distribution**

**Distribution** introduces **important concerns** beyond what we had to worry about when dealing with parallelism in the shared memory case:

1. *Partial failure*: crash failures of a subset of the machines involved in a distributed computation
2. *Latency*: certain operations have a much higher latency than other operations due to network communication



## **Why Spark?**

### Spark vs Hadoop/MapReduce

Hadoop/MapReduce: fault-tolerance in Hadoop/MapReduce comes at a cost. Between each map and reduce step, in order to recover from potential failures, Hadoop/MapReduce shuffles its data and write intermediate data to disk. (Reading/writing to disk: 100x slower than in-memory)

Latency numbers every programmer should know: <https://dzone.com/articles/every-programmer-should-know>



### Spark

1. Retains fault-tolerance
2. Different strategy for handling latency (latency significantly reduced)

**Idea:** Keep all data immutable and in-memory. All operations on data are just functional transformations, like regular Scala collections. Fault tolerance is achieved by replaying functional transformations over original dataset.

**Result:** Spark has been shown to be 100x more perfomant than Hadoop, while adding even more expressive APIs.

**Summarize:** Spark does it’s best to minimize any network traffic and turn to in-memory operations. While Hadoop/MapReduce uses on-disk & network instead.