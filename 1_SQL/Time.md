Time

time diff in terms of __time__:
`TIMEDIFF(expr1,expr2)`

time diff in terms of __DAYS__
`DATEDIFF(expr1,expr2)`

`GROUP BY YEAR(record_date), MONTH(record_date)`


substract and add/reduce interval
```sql
SELECT 
    YEAR(DATE_SUB('2023-12-10', INTERVAL 1 YEAR)) AS 'DATE_SUB'
    
```