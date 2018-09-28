---
title: SQL – The Differences in Joins
author: Scott
type: post
date: 2017-10-04T13:38:45+00:00
url: /2017/10/04/sql-the-differences-in-joins/
categories:
  - Technology
tags:
  - SQL

---
Oddly enough, the language I would say I have the most experience with has been T-SQL. A DML! Transact SQL is a proprietary extension to Structured Query Language and used with Microsoft&#8217;s SQL Server. For the longest time I&#8217;ve hacked together various reports, stored procedures, and jobs to do a range of things. Be it sending daily formatted HTML reports, to using complex queries that update the back end database for business software.

Also, for the longest time, I solely relied on the LEFT (OUTER) JOIN. I would have benefited greatly from learning the differences between the other types of joins, and probably saved myself a few headaches as well. I&#8217;ll use this post to describe and show examples of the different joins. If anyone ever stumbles across this, I hope you find it useful!

**Joins **are used to query data from additional tables within your select statement. Depending on the type of join used, you can define the results you are looking to see relative to the data contained within each table.

**Inner Join** &#8211; This is the default join used when you only specify JOIN in your select statement, connecting the tables specified only where the rows match on the column predicated. For example, with two tables holding a customer ID, one containing addresses and phone numbers, the other containing order history, you can join the two tables with the customer ID to pull all customer information from both tables where the specific customer ID is present.

**Outer Join **&#8211; Outer join is similar to Inner Join in that it returns all rows where matches are found on the predicate in both tables with the addition of rows where _no_ match is found. This is displayed with rows containing nulls in place column values for the table without the corresponding match. This type of join can be defined more narrowly with the keywords LEFT, RIGHT, or FULL, which specify which tables, or both, to display data where no matches were found. The keywords correspond to which side of the JOIN the tables are on, so depending on how you&#8217;ve written your query, you&#8217;ll want to be extra conscious of which side of the JOIN the table you&#8217;re looking to see results for is on. FULL OUTER JOIN is the default when using an outer join, but it is important to specify for code readability and maintenance and not leave any room for confusion

**Left Outer Join** &#8211; My bread and butter, same as the LEFT JOIN. This join is used to display nulls where no match on the join predicate is found in the table joined on the right. To elaborate with my previous example, if you are looking for Customers who have order history but do not have a phone number, you would SELECT from the order table, LEFT OUTER JOIN the table containing addresses and phone numbers using the customer ID as the join predicate. This would return a result set showing all customers from the order history table as well as either the customers phone numbers or nulls where a corresponding record with the same customer ID was not found.

**Right Outer Join **&#8211; If you were to be requested to pull all customer phone numbers to see if any order history was associated with them, you would SELECT from the order table, RIGHT OUTER JOIN the phone number table and would receive a result set of all phone numbers and any customer order history that exists, or doesn&#8217;t exist in the case of nulls.

These are the most common joins you will see in database development. Depending on the results you are looking for, it&#8217;s smart to get familiar with the different types as otherwise you&#8217;ll be left with finding creative and inefficient ways to pull the same data. Below is a graphic utilizing Venn diagrams to further illustrate the concepts. Where INNER is not specified, it is assumed to be an OUTER join.

<img class="alignnone size-full wp-image-181" src="https://i1.wp.com/scottrchristian.com/wp-content/uploads/2017/10/LEFT-vs-Right-Outer-Join-in-SQL.png?resize=980%2C693&#038;ssl=1" alt="" width="980" height="693" srcset="https://i1.wp.com/scottrchristian.com/wp-content/uploads/2017/10/LEFT-vs-Right-Outer-Join-in-SQL.png?w=1024&ssl=1 1024w, https://i1.wp.com/scottrchristian.com/wp-content/uploads/2017/10/LEFT-vs-Right-Outer-Join-in-SQL.png?resize=300%2C212&ssl=1 300w, https://i1.wp.com/scottrchristian.com/wp-content/uploads/2017/10/LEFT-vs-Right-Outer-Join-in-SQL.png?resize=768%2C543&ssl=1 768w" sizes="(max-width: 980px) 100vw, 980px" data-recalc-dims="1" />