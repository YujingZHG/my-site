---
title: "The Difference Between Partition and Cluster in BigQuery"
date: 2026-03-04
draft: false
tags: ["BigQuery", "Data Engineering", "SQL", "Performance", "Google Cloud"]
---

When I designed the tables for our customer snapshot,which will be a big table as data grow every week. The first question I asked before writing a single line of SQL was: "How is this table going to be queried? After a hundred thousands records, how long will the query take and how much will it cost?"

Before we create a table, partition and cluster should be considered. Get them right at the start and the queries stay fast and cheap as data grows. For cloud servies like BigQuery which charged based on the scanned amount. If our table has 3 years of daily transactions and we only want last week's data, scan whole data will cost a lot.


## What is Partitioning?

Partition is often used for a sigle column like date to divide data. When you query "last week only," BigQuery opens only that one section and ignoresother sections. Like the index that your don't need to check the detail to see what it is about, this is a index for scan engine.


## What is Clustering?

Cluster will physically put similar data together for engine to skip scanning irrelevant data for a fater scan.


How partition and cluster work together?



## How we set partition and cluster in Big query for a new table
```sql
CREATE TABLE `project.dummy_dataset.customer_trace` (
    dc_id         INTEGER,
    cust_code     STRING,
    rep           STRING,
    snapshot_date DATE,
    ...
)
PARTITION BY snapshot_date
CLUSTER BY dc_id, cust_code;
```


## How to Choose the Cluster Column



## Summary

Partitioning cuts down how many time periods BigQuery reads. Clustering cuts down how many rows within that period it reads. Together, they make your queries faster and cheaper.

The key is not to apply them randomly — match them to how you actually query the table. Get that right from the start, and you won't need to redesign later.

## Reference