---
title: SQL server statistics
date: 2023-02-26T00:00:00+00:00
layout: post
permalink: /sql-server-statistics/
categories:
  - sql-server
tags:
  - sql server
---

The query optimizer in SQL server uses statistics to improve query performance. Statistics are large BLOBs containing statistical information about the distribution of values in one or more columns of a table or indexed view. This is done using a histogram which measures the frequency of distinct values in a data set. Statistics is based on a sample of the rows or by doing a full scan of all the rows. The query optimizer uses statistics to estimate the cardinality(number of rows) in the query result to find the optimal query plan.

To enhance cardinality estimates the query optimizer uses densities for queries that return multiple columns from the same table or index view. Density gives information about the number of duplicates in a given column and is calculated as `1/number of distinct values`. Lower density means more unique values.


Statistics is auto updated when `AUTO_UPDATE_STATISTICS` is on. If you want to update statistics more frequently than the automatic thresholds you can do so with the `UPDATE STATISTICS` statement or the stored procedure `sp_updatestats`. Updating statistics may cause query plans to recompile therefore it's not recommended to update statistics too frequently.

The query optimizer automatically create statistics on the key column for index on tables or views when the index is created. When `AUTO_CREATE_STATISTICS` is on the query optimizer automatically creates statistics for single columns in query predicates.

In some cases you may want to create your own statistics object.
- The query predicates contains multiple correlated columns that are not in the same index
- The query selects from a subset of data

Creating a statistics object manually is done with the `CREATE STATISTICS` statement.

`DBCC SHOW_STATISTICS` is useful for displaying the header, histogram and density vector for a statistics object. The header contains info about when the statistics was last updated, how many rows the table had when it was last updated and how many rows that was sampled. It also shows detailed information about the histogram which can be used to compare against actual query plans.
