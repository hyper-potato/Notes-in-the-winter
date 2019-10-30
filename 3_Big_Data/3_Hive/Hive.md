# Hive (conceptual)

- Use SQL to run queries on large datasets
- Developed at Facebook
- Similar to Pig, Hive runs on client computer that submit jobs (no need to install on Hadoop cluster)
- You write HiveQL (Hive’s query language), which gets converted into MapReduce jobs
  - HiveQL: A subset ANSI SQL-92 standard, plus a few extensions found in MySQL and Oracle SQL dialects
  - Turns HiveQL queries into a directed acyclic graph of MapReduce, Tez, or Spark jobs. 
  - Submits those jobs to Hadoop cluster for execution, In the form of an execution plan 

## Hive execution model

- Initially runs on **MapReduce**, but now also supports **Tez** and **Spark** as execution engines 

- With many of the standard SQL functionalities, Hive is designed for: 
  - Analysts with SQL expertise 
  - BI tools that generate SQL 
  - ETL 

different ways of running hive queries

Hive runs on the client machine 



Why Use Apache Hive? 

- More productive than writing MapReduce directly
  - Five lines of HiveQL might be equivalent to 100 lines or more of Java 

- Brings large-scale data analysis to a broader audience 

  - No software development experience required
    Leverage existing knowledge of SQL 

- Offers interoperability with other systems 

  – ExtensiblethroughUDFs,JDBC/ODBC,andexternalscripts 

- Many business intelligence (BI) tools support Hive
   • Tableau, Datameter, Microstrategy, Pentaho, Qlikview 

- Caveat Emptor 
  - Remember that Hive generates Hadoop jobs, making it ultimately a batch processing platform, not a real-time / interactive platform* 





## Hive vs RDBMS

1. Hive is often considered data warehousing for Hadoop 
2. Hive shares many similarities with an RDBMS but there is at least one important difference 
   1. In an **RDBMS**, you create the table with rigid structure that must be specified before any data is added to the table (called “**schema on write**”). 
   2. with **Hive**, you can store the data in HDFS without knowing its format at all. You only need to specify the format (fields, types, etc) of the data when you need to read it (called “**schema on read**”). 

3. Pros and Cons of **Schema on Read**: 

   – **Pro**: more flexibility and speed on write 

   – **Con**: a conflict between the expected and actual data formats won’t be detected at the time records are added to a table.

4. Client-server RDBMS has many strengths 
   - Very **fast** response time (milliseconds) 
   - Support for transactions, with ACID (Atomicity, Consistency, Isolation, Durability) guarantees. 
   - Allow frequent modification of small number of records 
   - Can serve **thousands of simultaneous clients**

5. Hive does not turn your Hadoop cluster into an RDBMS
   - Initially has no support insert/update/deletion (IUD)
   - Initially has no support for ACID transactions*
   - No referential integrity
   - Batch job – latency is high compared to RDBMS 

| Query Language            | SQL       | HiveQL (subset of SQL) |
| ------------------------- | --------- | ---------------------- |
| Update Individual Records | Yes       | Managed tables only*   |
| Delete Individual Records | Yes       | Managed tables only*   |
| Transactions              | Yes       | Managed tables only*   |
| Index Support             | Extensive | Limited                |
| Latency                   | Very Low  | High**                 |
| Data Size                 | Terabytes | Petabytes              |
| Storage cost              | Very high | Very Low               |

## Hive use cases

- Data preparation
- ETL
- Data mining
- Ad optimization 

Use Case: Sentiment Analysis 

Use Case: Log File Analytics 



## How Hive Loads and Stores Data

Hive's queries operate on tables, just like in an RDBMS.

A table has two components:

- Meta data

  - Specify the structure and location of data

  - Defined when table is created

  - Stored in the **metastore**, contained in an RDBMS (typically Derby or MySQL),  

    **outside of HDFS**

- Data
  - Typically in an **HDFS directory** containing one or more files 
  - Default path: /user/hive/warehouse/<table_name> 
  - Can be in any of several formats for storage and retrieval 



How Hive stores/uses actual and meta data, row format options

1. Using Beeline (The Hive Shell) 

```bash
$ beeline -u jdbc:hive2://

show tables;

$ hive -e 'SELECT price, brand, name FROM dualcore.PRODUCTS ORDER BY
price DESC LIMIT 3'

!exit
```

```bash
# Running a HiveQL Script
$ cat verify_tablet_order.hql
$ vi verify_tablet_order.hql
    `i`
    add `dualcore.` before all table names.
    `ESC`
    `wq` then enter
$ hive -f verify_tablet_order.hql
```



2. through HUE



## Key storage formats and their characteristics

managed versus external Hive tables

- (In Hive 3) **Managed** (internal) tables

  - Fully under Hive control
  - ACID on by default
  - Default storage format: ORC 

- (In Hive 3) **External** tables

  - Outside control and management of data
  - No ACID
  - Default storage format: Text 

  

 ## Hive complex data types (array, struct, map). 











## Data Types

基本数据类型：tinyint, smallint, int,bigint, boolean, float, double, string

复杂数据类型：struct，map，array

### 基本数据类型

|  | 数据类型 | 所占字节 | Example                |
| --- | --- | --- | --- |
| Integer | TINYINT | 1byte，(-128 ~ 127) | 17 |
|  | SMALLINT | 2byte，(-32,768 ~ 32,767) | 5842 |
|  | INT | 4byte,    -2,147,483,648 ~ 2,147,483,647 | 84127213 |
|  | BIGINT | 8byte,   (-9.2 quintillion - ~ 9.2 quintillion) | 632197432180964 |
|  | BOOLEAN |   | TRUE |
| Decimal Types | FLOAT | 4byte单精度 | 3.14159 |
|  | DOUBLE | 8byte双精度 | 3.14159265358979323846 |
|  | Decimal(p,s) | Controls scale/precision of a number | 100.45 (p=5, s=2) |
| Other Simple (Scalar) | STRING | 字符串 | Betty F. Smith |
|  | CHAR(n) | Fixed-length character sequence | Hive␣␣(n=6) |
|  | VARCHAR(n) | Variable length character sequence | Hive (n=10) |
|  | BINARY | Raw bytes (Like VARBINARY in SQL) | N/A                    |
|  | TIMESTAMP | Instant in time (UTC) | 2013-06-14 16:51:05    |
|  | DATE |   | 从Hive0.12.0开始支持 |


default delimiter in hive: `CTRL-A `



### Complex column types in Hive

| Name   | Description & how to Define                                  | Stored Data (suppose $ is the collection item delimitator) | Access members  |
| ------ | ------------------------------------------------------------ | ---------------------------------------------------------- | --------------- |
| ARRAY  | Ordered list of values, all of the same type<br/> e.g. `departments array<string>` | `finance$marketing$hr`                                     | departments[0]  |
| MAP    | Key/value pairs, each of the same type <br>e.g. `prices map<string,int>` | `shoe#50$shirt#75`                                         | prices['shirt'] |
| STRUCT | Named fields, of possibly mixed types<br/> e.g. `addr struct<city:string,state:string,zip:int>` | `Minneapolis$MN$55455`                                     | addr.city       |



## Table partitioning

dynamic/static partition, 

## Common hive optimization strategies and their rationales

**Uses a Faster Execution Engine** 

Hive uses MapReduce (MR) engine by default, but it can also leverage faster engines 

- Tez: `set hive.execution.engine=tez`;
  - Tez also offers a customizable execution architecture, permitting dynamic performance optimizations, and dramatically improving the speed of execution. 
- Spark: `set hive.execution.engine=spark`; 
  - In memory computing, more customizable execution architecture. 
  - Expect a long delay as Spark initializes after you submit the first query 
  - Subsequent queries run without a delay 
  - Hive on Spark requires more memory on cluster than Hive on MapReduce 

- To see the current engine, use `SET hive.execution.engine`; 



**USE FASTER STORAGE FORMATS** 

- Textfile: default a plain text format, with fields terminated by Control-A's
  - **Good interoperability,** but poor performance

- SequenceFile: Good performance, but poor interoperability

- Avro: Excellent interoperability and performance

**Columnar** **formats**:

- Apache Parquet: **Excellent interoperability and performance**
- RCFile: Poor performance and limited interoperability
- ORCFile: Good performance but limited interoperability 



**Write better Performing Hive Queries** 



## difference between sort by and order by,

 the benefit of bucketing. 

• Understand the concept of Hive SerDe’s and their use cases; the use case of GET_JSON_OBJECT. • Understand how transform() works with external scripts. Different types of UDFs



## Create table

- Hive's default delimiter is `Control-A`, but you may specify an alternate delimiter (see below)

```sql
CREATE TABLE tablename 
(colname DATATYPE, ...)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY char # '\t'
STORED AS {TEXTFILE|RCFILE|AVRO|PARQUET ...};
```



- ROW FORMAT can also be SERDE 

  ROW FORMAT **SERDE serde_name WITH PROPERTIES (property specification)** 

```sql
create table titanic(
passengerid int,survived int,pclass int,name string,
sex string, age int,sibsp int,parch int,ticket string,
fare string,cabin string, embarked string)

ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.OpenCSVSerde'
WITH SERDEPROPERTIES (
"separatorChar"=",", 
"quoteChar"="\"",
"escapeChar"="\\" 
)
tblproperties ("skip.header.line.count"="1");
```



```sql
create table conversations (
	conversationId INT,
	accountNum INT,
	agentName STRING,
	category STRING,
	messages ARRAY<STRUCT<sender:STRING, time:TIMESTAMP, text:STRING>>
) 
ROW FORMAT SERDE 'org.openx.data.jsonserde.JsonSerDe';
```



loading data into a Hive table:

```sql
load data local inpath "/home/cloudera/titanic.csv" into table titanic;
```

 

1) Using `hadoop fs -put` to put loal data file(s) into a Hive table's storage location

2) Using `LOAD DATA LOCAL INPATH` within Hive to load local data file(s) into a Hive table.

3) Using `sqoop import` with the `--hive-import` option to load data from RDBMS into a Hive table

## Creating a Partitioned Table

```sql
CREATE EXTERNAL TABLE customers_by_state
(cust_id INT,
fname STRING,
lname STRING,
address STRING,
city STRING,
zipcode STRING)
PARTITIONED BY (state STRING)
ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
LOCATION '/dualcore/customers';
```







## Sort by or Oder by

In Hive, behind the scene

- ORDER BY, within each reducer, results in fully sorted results
- SORT BY sorts the input before feeding them into reducers, result in partially sorted results
- 

SORT BY can improve performance on large queries in 2 ways: 

– Bringing the benefits of parallelism to the reduce phase 

– In some case, it can eliminate the need for a second MapReduce job dedicated to global ordering of results. 

random sampling <http://www.joefkelley.com/736/>

```python
select * from my_table
sort by rand()
limit 10000;
```



## Bucketing data in Hive

Bucketing data is another way of subdividing data 

- Stores data in separate files 
  - Divides data into buckets in **an effectively random** way 
  - Calculates **hash codes** based on column values
  - Use hash codes to assign records to a bucket 
- Goal: Distribute rows across a predefined number of buckets 
  - Useful for jobs that need samples of data.
  - Joins may be faster if all tables are bucketed on the join column 



```sql
CREATE TABLE movie_bucketed  
(id INT,
     name STRING,
     year INT)
CLUSTERED BY (id) INTO 10 BUCKETS;
```



```sql
SET hive.enforce.bucketing=true;  
INSERT OVERWRITE TABLE movie_bucketed 
   SELECT * FROM movie_orc;
```

```sql
SELECT count(*) FROM orders_bucketed
TABLESAMPLE (BUCKET 2 OUT OF 5 ON order_id);
```





## Text

`SENTENCES`converts input document into arrays of words (one array per each sentence)

`NGRAMS`can take the array of words and turn into n-grams and their estimated frequencies. 

`EXPLODE` takes an array and turn into n-rows

`histogram_numeric(age,n)`Returns an array of struct<x:double, y:double>

`context_ngrams` return the most frequent 3 words phrases that start with "price"

(`ngrams` does not allow specifiction of part of the n-gram. `context_ngrams `does)



`\\d` captures a digit

`^` (inside the square bracket) for negating

`.` will match any character 

`\\.` matches a dot

`[^\\d]+.` match one or more non-digit characters followed by any character

