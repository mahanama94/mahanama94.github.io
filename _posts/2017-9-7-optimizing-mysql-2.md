---
layout: post
title: Optimizing Relational Databases - Queries
---

In a typical application, core logic handling data between the application layer and the persistance layer is
done through queries of which most of are SQL queries. (Sometimes native APIs are used for performance gains)
Hence it is vital to identify how they are processed at the query parsing and optimizing layer and find possible
means of achieving required performance through possible tweaks.

In post I will be sharing my experience in optimizing a relational database (MySQL InnoDB, NDB) with less or no
changes in the code of the application. Through some of tweaks, some queries gained significant performance almost 
halving the total execution time in the server.

## Process 

Query optimization is not a single step process, rather it needs to be done repeatedly to speedup your application.
In the post we will be following a simple 3 step process as,

1. Detect      - Identify the bottlenecks
2. Explain     - Identify how they are executed at db level
3. Optimize    - Taking measures to optimize

In your case you can repeat it as many times you wish. 

## 1. Detect

Detecting stage includes identifying possible bottlenecks in your application. If you have implemented the application or
if you have clear idea of the process flow and associated queries in the database, you can directly move ahead by listing
the queries that will be executed mostly for the next stage. 

If you don't have a clear idea you can use a tool like MySQL workbench to analyze the queries that are executed mostly 
or statements that add a significant cost to the application. By the end of the Detect stage you should have a 
list containing
Mostly executed statements - Under expected loading conditions.
Costly statements - Can use MySQL workbench

### MySQL Workbench

MySQl workbench is an awesome tool if you are onto query optimization. You can view mostly executed queries and 
their statistics ( Avg Execution time, highest time, error %, full table scans,...) easily in real time. You can download 
the tool by following the link below. 

[MySQL -Workbench Download](https://dev.mysql.com/downloads/workbench/)

## 2. Explain 

At the second stage you can analyze each statement you have listed down for performance using **MySQL EXPLAIN** statement 

``` sql
mysql> EXPLAIN SELECT user_name , user_id FROM user INNER JOIN user_contact USING ( user_id ) WHERE user_contact.contact_code = 'Random code' 

+----+-------------+--------------+------------+--------+----------------------+--------------+---------+-----------------------------------+------+----------+----------------------------------------+
| id | select_type | table        | partitions | type   | possible_keys        | key          | key_len | ref                               | rows | filtered | Extra                                  |
+----+-------------+--------------+------------+--------+----------------------+--------------+---------+-----------------------------------+------+----------+----------------------------------------+
|  1 | SIMPLE      | user_contact | p0         | ref    | user_id,contact_code | contact_code | 22      | const                             |    7 |   100.00 | Parent of 2 pushed join@1              |
|  1 | SIMPLE      | user         | p0         | eq_ref | PRIMARY              | PRIMARY      | 4       | db.user_contact.user_id           |    1 |   100.00 | Child of 'service_mt' in pushed join@1 |
+----+-------------+--------------+------------+--------+----------------------+--------------+---------+-----------------------------------+------+----------+----------------------------------------+

```

Output of the explain statement would provide you with insight of the query. Wether the query is using any Index, 
how many lines the query is going to filter and partitions (Vital in the clustered environment)

## 3. Optimize 

In the optimize stage you can optimize the statement use indexes or if you feel like it needs indexing, you can do it. 
Following are some of the steps which I have followed to optimize some queries. 

### Optimize The Join
In the relational world, you cannot ignore the joins unless your database is not normalized. The join queries can 
significantly alter the performance of your application if used without any caution. 

In a typical join the storage engine goes row by row from first table, then retrieve corresponding row from the second table. 
This can cause significant performance impact specially if the table scans are full table scana. Hence It will be 
a good idea to index the filtering column on the first table. Further, if the joining column in the second table is indexed,
the a significant performance can be achieved. 

![Optimize Join - Yoda]({{ site.baseurl }}/images/optimizing-mysql-2/optimize-the-join.jpg "Optimize Join - Yoda")

Folowing is an explain statement Output of the best possible Join statement. The performace gain is mainly due to the 
index columns in both tables which allows the storage engine to directly pick data without performing a scan on any table.
Through a similar optimization, I was able to almost halve the execution time of a query which had been executing 
nearly million times per a day. ( At peak load).

``` sql
mysql> EXPLAIN SELECT user_name , user_id FROM user INNER JOIN user_contact USING ( user_id ) WHERE user_contact.contact_code = 'Random code' 

+----+-------------+--------------+------------+--------+----------------------+--------------+---------+-----------------------------------+------+----------+----------------------------------------+
| id | select_type | table        | partitions | type   | possible_keys        | key          | key_len | ref                               | rows | filtered | Extra                                  |
+----+-------------+--------------+------------+--------+----------------------+--------------+---------+-----------------------------------+------+----------+----------------------------------------+
|  1 | SIMPLE      | user_contact | p0         | ref    | user_id,contact_code | contact_code | 22      | const                             |    7 |   100.00 | Parent of 2 pushed join@1              |
|  1 | SIMPLE      | user         | p0         | eq_ref | PRIMARY              | PRIMARY      | 4       | db.user_contact.user_id           |    1 |   100.00 | Child of 'service_mt' in pushed join@1 |
+----+-------------+--------------+------------+--------+----------------------+--------------+---------+-----------------------------------+------+----------+----------------------------------------+

```

#### NDB Storage
In the case of NDB storage engine, you will have to consider about the network partitioning in the cluster. For instace
the data required to be reqtrieved from the second table might be partitioned among different datanodes. In such cases, 
even the table is indexed, network latencies add up to your query killing the performace. 

In such cases you can tweak the partitioning of the second table by specifying a part of a primary key to 
partition for the NDB engine. By default NDB engine partition based on hash of the primary key. This is useful 
when the second table use parimary key containing first table's primary key. (User table and user_role tables)

``` sql 
ALTER TABLE second_table partition by key(key_part1);
```
It will cause the data to be partitioned according to primary  key of first table for both tables and eliminating any 
network latencies to add up in queries. In case of conflicting requirement of custom partitioning, you will have to 
make your decision based on the Query exection frequencies, table sizes and performance gain you can gain. 



### Index Appropriately

Indexes can make the slow queries lightening fast in the case of large tables. Bu to make most out of it you should be 
aware of the possible index types and their pros and cons. By default MySQL applies a B-Tree index to all the columns
that you index. But if the column filtering is an exact match you can use Hash indexes, which don not require tree 
traversal. 

You can keep the binary tree index if the column is subjected to wildcard ('AB%') and range operations (<, >, <=, >=)
as hash indexes cannot be used for these kind of operations. 

![Power of Indexes - Vader]({{ site.baseurl }}/images/optimizing-mysql-2/you-dont-know.jpg "Power of Indexes - Vader")


### Cache

By default MySQL uses a query cache and the cache hists are based on exact match of the query. 

``` sql 
mysql> select * from employee;

mysql> SELECT * from employee;

``` 
Even two queries are same, mysql cache would consider it as a cache miss. Therefore, it is a wiser to follow a convention 
in your queries to take out of the query cache. 

Further, the query cache allowence should not be too low or too much (100M - 200M) would suffice. If the query cache 
is too low you wolud not be able to retain cache between possible cache hits (Cache expires occur frequently) and if
too large MySQL node be forced to spend resources on managing cache than on executing SQL. 


Cheers !!!

**mahanama94**