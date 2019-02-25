# Keep An Eye On Index

### Anatomy of an Index. 
* An Index makes reads faster in a database by making writes slower.
* It is implemented with B-tree, which is a balanced tree with leaf nodes linked together with doubly linkedlists
  * Linkedlists are needed for range queries. 
* The B-tree enables the database to randomly access a row without full table scan thanks to logarithmic scalability. 
* A row in the index points to a physical address where the actual row exists in table. 
* Once we know the physical location of the row, we can load the relevant page into memory to access the row. 
  * It is important to note that the database table loaded on a page by page basis. 
* Use the execution plan to look at the steps the database takes to execute an SQL statement. 
* A concatednated index is one index across muliple columns
* It is important to choose the right column order in a concatenated index. 

### What makes an index slow? 
* Accessing the table for the matching row is the slowest part of the index.

### What should you do when your query is slow?
* An index is much faster when the returned result rows are small in number. 
  * Searching a user in a table using his user id. 
* An index actually slower than full table scan if the returned result row are huge. 
  * Querying all the users who are male. 
  * This is because the heap access part of the index is really slow. 
* Use bind variables as much as possible, because when the connection is established the optimizer creates a query plan and that plan will be used for all the remaining query hits. 
* The more complex the statement the more important using bind parameters becomes. Not using bind parameters is like recompiling a program every time. 
* The optimizer decides to use the index when the result set is smaller. Likewise when the result set grows bigger and bigger it decides against it. 

### Range queries
* Rule of thumb : In concatenated indexes, index for equality comes first and then ranges. 
* Creating index for the column in where clause that has a range constraint will result in much faster query results. 
  * Consider the consumer complaints dataset : 
    * > explain select * from consumer_complaints_medium where complaint_id > 468905 and complaint_id <= 469905;
    * After creating index the cost of execution reduced from 3000 to 10 when the result has only 40 rows. 
  * For queries with much larger number of result rows, the index scan is actually slower, and optimizer will not use it. 
    * > explain select * from consumer_complaints_medium where complaint_id > 468905;
    * The total number of rows is 65000 and the optimizer decided that the Seq Scan is faster than Index Scan. 


### Partial indexes 
* When we know that we query only for a particular value, we can create an index just for that value
 > SELECT message FROM messages WHERE processed = 'NO' AND receiver = ?  
 > CREATE INDEX messages_todo ON (receiver) WHERE prcessed = 'N'
 
### Dealing with dates
* When you deal with dates in queries, either range or equalify quries, it is a good idea to have the date column in a sorted order by indexing. 

### Joins
* Hash Join is used when table is huge and the other table is small. 
  * The smaller table is usually hashed into memory. 
  * If an index can help to reduce the size of the table by random access, the optimizer will choose that. This will enable hash join instead of nested loop join. 
  
### Powers of Indexes
* Through B-tree we can randomly access rows 
* Through linkedlist in the B-tree we can leverage sorted order of the indexes in compound indexes. 
  * This is particularly useful in range scans
* Index saves the database from sorting that needs to happen during order by, group by operation. 
  * > explain select * from consumer_complaints_medium order by complaint_id;
    * The above query with index created uses the index for sort step. The cost reduced from 27000 to 3000. 
    
### Index only access
* If your index has all the rows that the index selects and constraints, there's no need for heap table access and the queries run really fast. 

### Partial Results 
* When you use order by and limit only to top 5, indexes are the most useful because it saves the database from sorting step.
* Pagination can be made very simple in webapps by this manner. Since indexes provide N rows one after the other with out sort step.

### DML and Indexes
* Inserts are generally slower with indexes because the write needs to happen on the indexes as well. 
* Delete is faster with indexes when we have where statements that filters out the rows. But there's a negative side where the row has to be deleted in all indexes. 
* Update is very similar to delete. 

### Myths
* Indexes do not degenerate over time with update and delete statements. 
* Choose concatenated index based on the where clause in queries. An index with three columns can be used when searching for the first column, when searching with the first two columns together, and when searching using all columns.
* Whenever possible avoid select *
  * The more selective in columns you are the smaller hash joins, sorting memory foot print gets. Even helps with index only queries. 
  
  
### Explain query
* Gather phase means the optimizer has decided to execute the query in parallel. 




