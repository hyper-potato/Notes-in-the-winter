# Aggregate

mysql automotically excludes NULL


```sql
SELECT MAX(counted) FROM
(
    SELECT COUNT(*) AS counted
    FROM table_actions
    WHERE status = "good"
    GROUP BY user
) AS counts;
```

How to get Max Value for each group?
not a very simple solution using MAX() and GROUP BY clause.
Why we __cannot add any other columns in SELECT list which are not part of aggregation or the GROUP BY clause?__

Below is a one solution, you can find MAX record without using GROUP BY clause and you can also add other columns in the SELECT list.

First, Create a table with sample data:
```sql
CREATE TABLE tbl_EmployeeDetails
(
	EmpID INTEGER PRIMARY KEY 
    ,EmpName VARCHAR(50)
    ,EmpSalary BIGINT
    ,DepartmentName VARCHAR(50)
);

INSERT INTO tbl_EmployeeDetails
VALUES 
(1,'Anvesh',80000,'Sales')
,(2,'Neevan',90000,'Sales')
,(3,'Jenny',50000,'Production')
,(4,'Roy',60000,'Production')
,(5,'Martin',30000,'Research')
,(6,'Mahi',85000,'Research')
,(7,'Kruti',45000,'Research')
,(8,'Manish',75000,'Research');
```

Find max salary values for each department:

```sql
SELECT ED1.*
FROM tbl_EmployeeDetails ED1                   
LEFT JOIN tbl_EmployeeDetails ED2       
  ON ED1.DepartmentName = ED2.DepartmentName 
  AND ED1.EmpSalary < ED2.EmpSalary
WHERE ED2.EmpSalary IS NULL
```