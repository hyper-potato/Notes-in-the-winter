https://blog.eduonix.com/bigdata-and-hadoop/learn-use-partitioning-hive-improve-query-performance/

An error

When Insert into partition table I was stuck of with these error..

**Error**

 Execution Error, return code 2 from org.apache.hadoop.hive.ql.exec.mr.MapRedTask

....
[Fatal Error] Operator FS\_2 (id=2): Number of dynamic partitions exceeded hive.exec.max.dynamic.partitions.pernode.

above error in first line was little confusing, if you scroll up the console you may get the second line which wss the real cause of the exception.

\*\*Solution\*\*

 bdalab@solai:/opt$ hive

 ```bash
 hive\\\> set hive.exec.max.dynamic.partitions.pernode=500
 ```

by default hive.exec.max.dynamic.partitions.pernode set to 100, if the partition will exceeds the limit, you will get an error. Just change the default value based on the requirement to rid out of these.





## Dynamic vs  Static Partition

Table partitioning is supported on both external and internal tables. In the next sections we will demonstrate how this is done.


* Static Partition (SP) columns: in DML/DDL involving multiple partitioning columns, the columns whose values are known at COMPILE TIME (given by user).
* Dynamic Partition (DP) columns: columns whose values are only known at EXECUTION TIME.

### SP

Static partitioning is preferable over dynamic partitioning when you know the values of partition columns before data is loaded into a Hive table. Dynamic partitioning is better when you only know partition column values during data load. The decision on which type of partitioning to use is not usually clear but there are some key points to consider.

### DP

Dynamic partitioning is suitable in situations when:

* loading data from a hive table that is not yet partitioned. In this case when data is loaded there is no need for partitioning because the table is likely small. When loading data use of dynamic partitioning will resolve these issues. However with time there will be data growth that hurts performance. 
* Values of partition columns are not known. When there are difficulties in identifying values that are unique in a column you cannot use static partitioning. In such situations Hive identifies unique values and automatically creates partitions.
* Due to data growth you decide to change columns used to partition data. This arises when a previous partitioning cannot cope with data growth. This situation is resolved by creation of a new table with additional columns, loading data into a new table and deleting the previous table. During load of data into the new table dynamic partitioning is used.









