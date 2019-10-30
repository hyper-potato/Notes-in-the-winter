

# Spark and RDD

## What is Spark?

Spark is an open source cluster computing framework.

- automates distribution of data and computations on a cluster of computers
- provides a fault-tolerant abstraction to distributed datasets
- is based on functional programming primitives
- provides two abstractions to data, list-like (RDDs) and table-like (Datasets)



Spark vs MapReduce, why spark is faster?

- Functional APIs that generalize MapReduce • Arbitrary DAG of tasks rather than simply map -> reduce 

  DAG (Direct Acyclic Graph) - Spark & Tez; Spark runs in-memory, which is 10 times faster than mapreduce running on disk(HDFS)

- faster data format, partite org with compression

- LLAP (live long and process) reduces overhead, improves the performance further



## Resilient Distributed Datasets (RDDs)

Resilient Distributed Datasets are the primary abstraction in Spark – a fault-tolerant collection of elements that can be operated on in parallel

RDDs:

- are immutable
- reside (mostly) in memory
- are transparently distributed
- feature all FP programming primitives
  - in addition, more to minimize shuffling

In practice, `RDD[A]` works like Scala’s `List[A]`

![](https://img-blog.csdn.net/20180107012856671?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTA5NDQ1NA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

###  Two types:

 • *parallelized collections*– take an existing single-node collection and parallel it 

• *Hadoop datasets: files on HDFS or other compatible storage* 

### Operations on RDDs

- Transformations   f(RDD) => RDD

  § **Lazy** (not computed immediately) 

  §  转化操作是返回一个新的RDD的操作，比如map() 和filter()

- Actions

  § Triggers computation

  § E.g. “count”, “saveAsTextFile” 

  行动操作则是向驱动器(driver)程序返回结果或把结果写入外部系统的操作，会触发实际的计算，比如count() 和first()。

  

Spark 使用惰性求值，这样就可以把一些操作合并到一起来减少计算数据的步骤。在类似Hadoop MapReduce 的系统中，开发者常常花费大量时间考虑如何把操作组合到一起，以减少MapReduce 的周期数。而在Spark 中，写出一个非常复杂的映射并不见得能比使用很多简单的连续操作获得好很多的性能。因此，用户可以用更小的操作来组织他们的程序，这样也使这些操作更容易管理。