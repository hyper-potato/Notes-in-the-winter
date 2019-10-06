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