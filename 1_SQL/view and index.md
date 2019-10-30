##   A View is a Virtual Table  

A view in and of itself will not necessarily increase performance.

but it Makes it easier to reason about complex queries

```sql
create view
```



## Indices Speed Up Queries

## Index on multiple columns

One thing to note, clustered indexes should have a unique key (an identity column I would recommend) as the first column. Basically it helps your data insert at the end of the index and not cause lots of disk IO and Page splits.

Secondly, if you are creating other indexes on your data and they are constructed cleverly they will be reused.

e.g. imagine you search a table on three columns

state, county, zip.

- you sometimes search by state only.
- you sometimes search by state and county.
- you frequently search by state, county, zip.

Then an index with state, county, zip. will be used in all three of these searches.

If you search by zip alone quite a lot then the above index will not be used (by SQL Server anyway) as zip is the third part of that index and the query optimiser will not see that index as helpful.

You could then create an index on Zip alone that would be used in this instance.

By the way [We can take advantage of the fact that with Multi-Column indexing the first index column is always usable for searching](https://use-the-index-luke.com/sql/where-clause/the-equals-operator/concatenated-keys) and when you search only by 'state' it is efficient but yet not as efficient as Single-Column index on 'state'

I guess the answer depends on your where clauses of frequently used queries and also group by's.





