# Database

The DBMS Approach has Numerous Advantages

- **Less redundancy**: all data items can be integrated with minimum duplication
- **Program-data independence**: data is separated from the programs: standard data querying, standard data management, standard everything…
- **Sharing data**: the database can be accessed by the entire organization (and at the same time!)
- **Data consistency**: DBMS has standard built-in mechanisms that reduce data inconsistencies



## Relational Database Concepts

**Equivalent Database Concepts**

- Relation: A relation is a **table** with columns and rows. 
- Tuple <-> Row or record
- Attribute: An attribute is a named **column** of a relation.
- Cardinality <-> Number of rows/tuples it contains. 
- Degree <-> Number of columns/attributes
- Primary key <-> Unique identifier
- Domain: A set of allowable values for one or more attributes. 
- A **relational database** is a collection of normalized relations with distinct relation names. 
- The **intension** of a relation is the structure of the relation including its domains. 
- The **extension** of a relation is the set of tuples currently in the relation.



## Constraints

The DBMS Ensures Integrity Through Constraints  

- **Entity Integrity**

  –Each row of a table must be unique in some way

- Referential Integrity

  –Values in one table must match the values in the related table

### Referential Actions

(how foreign keys help guarantee [referential integrity](https://dev.mysql.com/doc/refman/8.0/en/glossary.html#glos_referential_integrity))

- `RESTRICT`: **Rejects** the delete or update operation for the parent table. Specifying `RESTRICT` (or `NO ACTION`) is the same as omitting the `ON DELETE` or `ON UPDATE` clause.

- `CASCADE`: Delete or update the row from the parent table, and automatically delete or update the matching rows in the child table. Both `ON DELETE CASCADE` and `ON UPDATE CASCADE` are supported. Between two tables, do not define several `ON UPDATE CASCADE` clauses that act on the same column in the parent table or in the child table.

  If a `FOREIGN KEY` clause is defined on both tables in a foreign key relationship, making both tables a parent and child, an `ON UPDATE CASCADE` or `ON DELETE CASCADE` subclause defined for one `FOREIGN KEY` clause must be defined for the other in order for cascading operations to succeed. If an `ON UPDATE CASCADE` or `ON DELETE CASCADE` subclause is only defined for one `FOREIGN KEY` clause, cascading operations fail with an error.

- `SET NULL`: Delete or update the row from the parent table, and set the foreign key column or columns in the child table to `NULL`. Both `ON DELETE SET NULL` and `ON UPDATE SET NULL` clauses are supported. If you specify a `SET NULL` action, *make sure that you have not declared the columns in the child table as `NOT NULL`*.

- `NO ACTION`: A keyword from standard SQL. In MySQL, equivalent to `RESTRICT`. The MySQL Server rejects the delete or update operation for the parent table if there is a related foreign key value in the referenced table. Some database systems have deferred checks, and `NO ACTION` is a deferred check. In MySQL, foreign key constraints are checked immediately, so `NO ACTION` is the same as `RESTRICT`.

- `SET DEFAULT`: This action is recognized by the MySQL parser, but both [`InnoDB`](https://dev.mysql.com/doc/refman/8.0/en/innodb-storage-engine.html) and [`NDB`](https://dev.mysql.com/doc/refman/8.0/en/mysql-cluster.html) reject table definitions containing `ON DELETE SET DEFAULT` or `ON UPDATE SET DEFAULT` clauses.

![](https://miro.medium.com/max/2046/1*q5jfVpmGyDjxnQRjsvO44A.png)







## History of Database and SQL (notice they are not the same)

1. Database Technology has a Long History

   - 1960s: File-based systems
   - 1970s: Primitive systems that attempt to connect data records together
   - **1980s** Relational model (data as smart tables)
     - This is the **dominant** paradigm in the business world today.

2.  Key Individuals Influenced the Relational Model  

   - **Edgar. F. Codd** (1981 Turing Award Winner)’s 

     –“A Relational Model of Data for Large Shared Data Banks“ (1970) paper

     –Database humor: “… so help me Codd!”

   - Peter Pin-Shan Chen

     –The Entity-Relationship Model: Toward a unified view of data. TODS, 1(1):9--36, 1976.

   - **Larry Ellison** learned about:

     **credited with establishing the relational database paradigm as the primary data storage paradigm**

     –Relation Model from Codd’s paper

     –SQL through publications by the IBM’s System R project team

     –Founded company Relational Software Inc.

     ​	- What is this company known as today? (Oracle)

   ​    

3. SQL is almost 40 years old!  

- Originally developed by IBM in mid-1970s

- 1979–Oracle markets first relational DB with SQL

- 1986–ANSI SQL standard released
- 1989, 1992, 1999, 2003–Major ANSI standard updates
- Current–SQL is supported by most major database vendors
- Most databases claim SQL:1999 compliance and partial SQL:2003 compliance. (see http://troels.arvin.dk/db/rdbms/ for details)



## More SQL

4. SQL is considered 4th Generation  

- SQL is **relational**: its queries operate only on tables in a relational database, and produce only tables as the result
- SQL is **nonprocedural**: the user specifies what must be done, but not how it is to be done
- SQL is **unified**: Database administrators (DBAs), programmers, and system administrators all learn and use a single language and set of constructs



5. SQL has 3 dimensions  

- Data Definition Language (DDL)
  - define a database, including **creating**, **altering**, and **dropping** tables and establishing **constraints**
  - add, modify, and drop tables; create views 
  - define and enforce integrity constraints; enforce security restrictions

- Data Manipulation Language (DML)
  - maintain and query a database, such as inserting, deleting, updating, querying records.
  - § Select       **§** **Update**      **§** **Insert**    **§** **Delete** 

- Data Control Language (DCL)
  - control a database, including administering privileges and committing data
  - § Grant     § Revoke





## SQL Basic Rules 

Some basic rules for SQL statements: 

1. There is a set of reserved words that cannot be used as names for database objects. (e.g. SELECT, FROM, WHERE) 
2. SQL is case-insensitive. 
   - Only exception is string constants. 'FRED' not the same as 'fred'. 
3. SQL is free-format and white-space is ignored. 
4. The semi-colon is often used as a statement terminator, although that is not always required. 
5. Date and time constants have defined format: 
   - Dates: 'YYYY-MM-DD' e.g. '1975-05-17' 
   - Times: ‘hh:mm:ss[.f] ' e.g. '15:00:00' 
   - Timestamp: ‘YYYY-MM-DD hh:mm:ss[.f] ' e.g. ‘1975-05-17 15:00:00' 

6. Two single quotes '' are used to represent a single quote character in a character constant. e.g. 'Master''s'.



## Data Type

**charactor** 

The number of bytes of storage occupied by a single character depends on the character encoding you are using. A programming language might support one or two encodings, or might support a wide variety of character encodings. Likewise, an operating system, file system, etc. may support one or two encodings, or might support a wide variety of encodings. The number of bytes used for each character limits the number of unique characters that can be represented.

Here are a few examples:

- ASCII (1 byte per character)

- ISO 10646

- - UCS-2 (2 bytes per character)
  - UCS-4 (4 bytes per character)

- Unicode

- - UTF-8 (variable: 1, 2, 3, or 4 bytes per character)
  - UTF-16 (variable: 2 or 4 bytes per character)
  - UTF-32 (4 bytes per character)



## Questions:

1. What term best describes the following?

```mysql
CREATE TABLE UserPermissions (
  userId INTEGER NOT NULL,
  permissionId INTEGER NOT NULL,
  PRIMARY KEY (userId, permissionId),
  CONSTRAINT FOREIGN KEY (userId) REFERENCES User (userId),
  CONSTRAINT FOREIGN KEY (permissionId) REFERENCES Permission (permissionId));
```

a.Field

b.Tuple

c.DML

**d.Intersection**

e.DCL





2. The Structured Query Language (SQL) can be used to do what? 

   a. query relational database structures for data

   **b. create relational database structures such as tables, columns, and indices**

   c. neither a nor b 

   d. both a & b