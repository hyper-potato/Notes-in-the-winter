<u>inclass note</u>  

approach1	

```sql
psdueo
create table raw
(
json string
)
row format delimited ';' 
insert into users
(select * from raw 
 get_json_object(___, '$_type') = 'user'
```



approach2:

```
hadoop

streamtiy

-- extract_users.py

-- extract_review.py

-- extract_business.py
```