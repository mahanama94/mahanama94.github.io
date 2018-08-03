---
layout: post
title: Optimizing Relational Databases - Attributes
tags: [Databases, MySQL]
---

When it comes to delivering an optimal product to your customers, optimizing the system becomes a must to ensure that
your clients will not have to go for your competitor or some alternative solution as their systems begins to grow.
For instace if we consider a simple inventory management system, we must take the growth of their system in to
account when developing the system. Since all of the system uses some form of persistance layer in the form of a
database, database optimizing plays a vital role in optimizing performance.

![Optimize MySQL - Drunk Baby]({{ site.baseurl }}/images/optimizing-mysql/optimize-mysql-yoursql.jpg "Optimize MySQL - Drunk Baby")

In the series I will be sharing some of my experience on optimizing relational databases(MySQL with InnoDB, MyISAM, NDB)
to significantly boost performance. Most of the changes suggested in the series can be applied with no or less
change in your code.

## Over Allocation

This is one of the key points the where performance of MySQL is lost significantly. Having too long attributes will degrade
the performance of your application. For instance consider name attribute using varchar(100), in reality none of
the users will have such a long name and most probably would not use such a name.

**Even "Uvuvwevwevwe Onyetenyevwe Ugwemuhwem Osas" has less than 50 characters :D**

Over allocated attributes can increase number of disk seeks significantly in the case of a scan ( Retrieval without
the use of a key) since the number of rows of data that can be fetched in a single disk seek is reduced since
volume of data that ca be fetched in a single disk seek is constant.

## Row Data usage

Similar to the above, the row data usage can degrade the performance of the query reducing the number of rows per
disk seek during queries. For instance some of attributes in the tables might be rarely used. For instace consider
addresses in a employee table. Most of the time it might not be used in your application along with some other columns.
In such situations it is a good idea to split the table into mostly used and rearely used columns and retrieve according
to the requirement. This can show significant improvement specially if the table is highly populated and application
is performaing scans frequently.

## Prefer Numeric

For columns that can be represented either as a numeric or a string, make your preference to numeric over string.
Consider the case of storing mobile number in the database. (10 digits in Sri Lanka). We can either use Integer tpe or
string type for storing these information. If Int was to be used the memory footprint would be 4 Bytes compared to
to 10 Bytes in the case of varchar.

Further Numeric data fields provides faster transafer and conpare operations compared to String data types.

![What If - Morpheus]({{ site.baseurl }}/images/optimizing-mysql/what-if-i-told-you.jpg "What If - Morpheus")

## Match the Attributes

Relational databases rely on relationships among data in different tables for retrieval. When constructing your database
consider the relationships between various attributes among tables and declare those columns with same type of
data, size, character set and collation. This will prevent any character conversions during the query which adds
an unnecessary overhead.

## Shorter Primary Key

Having a long Primary Key leads to a significant waste of disk spaces. In the case of a secondary index, Primary index
is duplicated along with the columns specified for the secondary index records. The wastage can be significant if the
table is having multiple secondary indices along with a long primary key. Possible workaround for the issue
is the use of an auto increment column.


## Some Useful Resources

[MySQL Storage Requirements](https://dev.mysql.com/doc/refman/5.7/en/storage-requirements.html)

[MySQL Integer Types](https://dev.mysql.com/doc/refman/5.7/en/integer-types.html)

[MySQL String Types](https://dev.mysql.com/doc/refman/5.7/en/char.html)

### Hire me

I currently work as a freelancer for projects in mobile applications, web applications, desktop applications, data mining
and machine learning. I provide my services mainly using Ionic framework, PHP, NodeJs, Java, electronJs, Erlang and Python.
You can directly contact me for your projects or can visit my Fiverr profile. Please contact me before placing an order.

[mahanama94 Fiverr](https://www.fiverr.com/mahanama94/)

Cheers !!!

**mahanama94**
