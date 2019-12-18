## Syntactic and Semantic Checking

Rules of syntax specify how language elements are sequenced to form valid statements. Thus, *syntactic checking* verifies that keywords, object names, operators, delimiters, and so on are placed correctly in your SQL statement. For example, the following embedded SQL statements contain syntax errors:

```sql
-- misspelled keyword WHERE
EXEC SQL DELETE FROM EMP WERE DEPTNO = 20;
-- missing parentheses around column names COMM and SAL
EXEC SQL INSERT INTO EMP COMM, SAL VALUES (NULL, 1500);
```

Rules of semantics specify how valid external references are made. Thus, *semantic checking* verifies that references to database objects and host variables are valid and that host-variable datatypes are correct. For example, the following embedded SQL statements contain semantic errors:

```sql
-- nonexistent table, EMPP
EXEC SQL DELETE FROM EMPP WHERE DEPTNO = 20;
-- undeclared host variable, emp_name
EXEC SQL SELECT * FROM EMP WHERE ENAME = :emp_name;
```