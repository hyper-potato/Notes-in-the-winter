# Hadoop

- Open-source software for reliable, scalable, distributed computing, Written in Java
- Hadoop is a distributed system 
  - A collection of servers running Hadoop software is called a **cluster**
  - Individual servers within a cluster are called **nodes**
      - Typically standard rack-mount servers running Linux
      - Each node both stores and processes data
          - Called “data locality”

- Add more nodes to the cluster to increase **scalability**
  - Scale to thousands of machines
    * Facebook and Yahoo are each running clusters in excess of 4400 nodes
  - Linear scalability (with good algorithm design):  if you have 2 machines, your job runs twice as fast (ideally)
    * Horizontal scaling simplifies capacity planning as well
    * **Horizontal** scaling: scale by adding more **machines** to the pool of resources
    * **Vertical** scaling: scale by adding more **power** CPU/RAM to an existing machine
    Therefore, powerful Hadoop clusters can be built from cheap commodity hardware, resulting in lowered cost!

- Uses simple programming model (MapReduce) 

- Fault tolerant (HDFS)
  + Can recover from machine/disk failure  (no need to restart computation)

- Cons: hadoop is designed for batch processing, it has **high latency compared to RDBMS.**


## HDFS – The Hadoop FileSystem

What can you use Hadoop for?
- Works for many types of analyses/tasks (but not all of them).
What if you want to write less code?
- There are tools to make it easier to write MapReduce program (Pig), or to query results (Hive)

HDFS started at Yahoo as an open source replica of the GFS paper, but since v2.0 it is different system.

| GFS | HDFS |
| :-- | :-- |
| Master | NameNode |
| Chunkserver | DataNode |
| operation log | journal |
| chunk | block |
| **random file writes** | **append-only** |
| multiple writer/reader | single writer, multiple readers |
| chunk: 64MB data, 32bit checksums | 128MB data, separate metadata file |

###  HDFS: ++H++a++d++oop ++F++ile ++S++ystem

-  Provides inexpensive and reliable storage for massive amounts of data
  - Optimized for a relatively **small number of large files **
    - Each file likely to exceed 100 MB, multi-gigabyte files are common
  - Store file in hierarchical directory structure • e.g.,/sales/reports/asia.txt
  - Cannot modify files once written 
    - Need to make changes? remove and recreate

### Architecture
HDFS files are broken into blocks(128MB by default), a smallest unit to read and write
+ An HDFS cluster has a master/slave architecture
  + HDFS **NameNode** (one or two)
    *   Manages namespace (file to block mappings) and **metadata** (block to machine mappings).
    *   Monitors dataNodes

  *  HDFS **DataNodes** (many)
    - Reads and writes the **actual data** blocks

![image.png](https://i.loli.net/2019/10/21/AMHNJyRBtciwxgd.png)


### HDFS access session
  - Use Hadoop specific utilities to access HDFS

```
# List directory
$ hadoop fs ls /
Found 7 items
drwxr-xr-x   - gousiosg sg          0 2017-10-04 08:23 /audioscrobbler
-rw-r--r--   3 gousiosg sg 1215803135 2017-10-04 08:25 /ghtorrent-logs.txt
-rw-r--r--   3 gousiosg sg   66773425 2017-10-04 08:23 /imdb.csv
-rw-r--r--   3 gousiosg sg     198376 2017-10-23 12:39 /important-repos.csv
-rw-r--r--   3 gousiosg sg     611300 2017-10-04 08:24 /odyssey.mb.txt
-rw-r--r--   3 gousiosg sg  388422973 2017-10-03 15:40 /pullreqs.csv

# Create a new file
$ dd if=/dev/zero of=foobar bs=1M count=100

# Upload file
$ hadoop fs -put foobar /
-rw-r--r--   3 gousiosg sg  104857600 2017-11-27 15:42 /foobar
```



## How does Hadoop scale up computation? 
Uses master-worker architecture, and a simple computation model called **MapReduce**  (popularized by Google’s paper)

Simple way to think about it:
1. **Divide** data and computation into smaller pieces; each machine works on one piece
2. **Combine** results to produce final results

More technically...
1. Map phase 
Master node divides data and computation into smaller pieces; each worker node (“mapper”) works on one piece independently in parallel
2. Shuffle phase (automatically done for you) 
Master sorts and moves results to “reducers”
3. Reduce phase 
Worker nodes (“reducers”) combines results independently in parallel


Example: Find words’ frequencies among text documents
Input
*   “Apple Orange Mango Orange Grapes Plum”
*   “Apple Plum Mango Apple Apple Plum” 

Output
Apple, 4 
Grapes, 1 
Mango, 2 
Orange, 2 
Plum, 3
![image.png](https://i.loli.net/2019/10/21/v9h3ITws5SWLZgR.png)

## Implementation
```
map(String key, String value): 
// key: document id 
// value: document contents 
for each word w in value:   emit(w, "1");
```

```
reduce(String key, Iterator values): 
// key: a word 
// values: a list of counts 
int result = 0; 
for each v in values:    result += ParseInt(v); 
Emit(AsString(result));
```


What if a machine dies? 
Replace it!
“map” and “reduce” jobs redistributed (for you) to other machines
Hadoop’s **HDFS** (Hadoop File System) enables this



A distribute file system
- Built on top of OS’s existing file system to provide redundancy and distribution
- HDFS hides complexity of distributed storage and redundancy from the programmer
In short, you don’t need to worry much about this!


## Hadoop 2.0

When compared to Hadoop 1.x, Hadoop 2 Architecture is designed completely different. It has added one new component : YARN and also updated HDFS and MapReduce component’s Responsibilities.

![### Hadoop 2.x Architecture](https://cdn.journaldev.com/wp-content/uploads/2015/08/hadoop2.x-components.png)
Hadoop 2.x has the following three Major Components:

*   HDFS
*   YARN (stands for Yet Another Resource Negotiator)
*   MapReduce

Remaining all Hadoop Ecosystem components work on top of these three major components: HDFS, YARN and MapReduce. 


### Hadoop 2.x Major Components


![Hadoop 2.x Components High-Level Architecture](https://cdn.journaldev.com/wp-content/uploads/2015/08/hadoop2.x-highlevel-architecture.png)
*   Separate MapReduce into a YARN layer and MR (for batch processing)
*   Support non-MapReduce applications, e.g. streaming and interactive applications (e.g. Impala, Spark, Storm, HBase, Giraph etc)
*   Improved nameNode availability





## Ecosystem

<img src ='https://www.cdn.geeksforgeeks.org/wp-content/uploads/HadoopEcosystem-min.png'>

<img src='https://www.edureka.co/blog/wp-content/uploads/2016/10/HADOOP-ECOSYSTEM-Edureka.png'>