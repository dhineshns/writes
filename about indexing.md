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

### Range queries
* Rule of thumb : In concatenated indexes, index for equality comes first and then ranges. 

### Paritial indexes 
* When we know that we query only for a particular value, we can create an index just for that value
 * > SELECT message FROM messages WHERE processed = 'NO' AND receiver = ?
 * > CREATE INDEX messages_todo ON (receiver) WHERE prcessed = 'N'

