



# Sqoop

Name is a contraction of “SQL-to-Hadoop” 

```bash
sqoop import \ 
--connect		jdbc:mysql://localhost/company \
--username 	twheeler \
--password 	bigsecret \
--warehouse-dir 	/mydata \
--table			customers
```



Incremental Imports With Sqoop  

- What if new records are added to the database? – You could re-import all records, but this is inefficient 
- Sqoop's incremental append mode imports only new records – Based on value of the last record in a specified column
   – Note that this is **an append, not an update** 

```bash
sqoop import \ 
--connect 		jdbc:mysql://localhost/company \ 
--username 		twheeler \ 
--password 		bigsecret \ 
--warehouse-dir  /mydata \
--incremental 	append \
--check-column 	order_id \ 
--last-value		6713821
```



What if existing records are also modified in the database? – Incremental append mode doesn’t handle this 

Sqoop’s “lastmodified” append mode adds and updates records – Caveat: You must maintain a timestamp column in your table 

```bash
sqoop import \
--connect 		jdbc:mysql://localhost/company \ 
--username 		twheeler \ 
--password 		bigsecret \ 
--warehouse-dir  /mydata \
--incremental 	lastmodified \
--check-column 	last_update_date \ 
--last-value		"2013-06-12 03:15:59"
```

Beneath the hood: Sqoop runs two standalone MapReduce jobs. One that imports the changed rows into a temporary directory in HDFS. The other that merges the changed rows with the records previously imported into a new set of up-to-date records. So its not a true update in the sense that existing records are modified. Instead, the files containing them are replaced. 

