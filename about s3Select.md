# S3 Select
* Use S3 select when you want to query for a partitular row in a huge JSON or CSV file you have stored in S3
* Only supports SELECT and WHERE clauses, use primarily for the purpose of reducing the workload on the application side and offloading the work onto S3
* If you have a data lake in year/month/date format, try to change the structure of the data lake to year/month/date/hour/minutes/seconds so that it's the CSV/JSON file size is small enough. This improves the processing time and saves cost. 
* Can be used in place of costly key/value store like Cassandra if the data lake is partitioned on seconds level granular scale. 
* To give more perspective, 
  * Redshift is a Datawarehousing product, Athena and Redshift Spectrum are alternatives for that.
  * Cassandra is a keyvalue store, S3 Select is an alternative for that.
