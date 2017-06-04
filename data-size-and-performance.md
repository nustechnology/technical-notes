<center>
  <img src="assets/images/data-size-and-performance/slowapps.png">
</center>

When QC tells developer that a feature is so slow, I often hear the developer says "It is caused by large number of posts in database. The feature must be slow when we have large data!". Well, the first sentence may be a correct **explanation** of the issue but the second one is **often** not a fact. We don't accept this, at least in NUS Technology. He must do something wrong (or not right yet) when this issue occurs.

The above issue is a popular case when developing a software: the performance goes down when data size grows. So, why does it happen and how to prevent it?

#### DBMS takes much time to search some records

When records count grows, DBMS takes more time to search some records (e.g: search `activities` of an user) because it needs to travel through all records to find matched records. This is simplest case and easiest to be fixed but I have seen many applications have this performance issue!

To fix it, simply add corresponding indexes for search field (e.g: `user_id` in `activities` table). DBMS simply get correct records via this index, no need to travel through all records anymore.

#### Index is correct but DBMS still takes much time for querying records?

This is often caused by very large data, so the index is big and DBMS takes time to travel through this index (DBMS often uses BTREE for indexes).

In this case, you will need to implement [data partitioning](https://en.wikipedia.org/wiki/Partition_(database)). The simplest and most popular partitioning is horizontal partitioning (AKA: data sharding) with range, it breaks table to multiple tables, each table contains records that have a field from X to Y. E.g: create partition for `activities` table by `user_id` with range `10,000` will store all activities with `user_id` from `1` to `10,000` in first table, all activities with user_id from `10,001` to `20,000` in second table, and so on... So, when we querying `activities` of an user with id 100, DBMS needs to find in first table only.

All modern DBMS have supported data partitioning with just some simple commands but please be careful when deciding to use partitioning because using it in a wrong way will need to awful performance. E.g: make partition by `user_id` but you often need to filter only by `activity_type` will be awful because DBMS needs to search on all tables and merge results. Remember a rule: **when using partition by a field, queries always need to include that field in AND combination**

#### Only pull all records from database with no condition but it is slow

Well, you should not pull all records from database in almost cases. I think I don't need to tell you why :) Simply use paging for showing data or find in batches in case you indeed need to get all data (e.g: exporting)

#### Only pull 100 records from database with no condition but it is still slow!!!

Mostly because you have plenty of fields in this table or you have heavy field(s). I saw an app has `routes` table with a JSON field to contain all points and point data, it often contains 2 - 3 MB JSON data and developer retrieves all fields to show only route name! App is frozen when visiting this list.

Simply select only needed fields to prevent this.

#### I have correct indexes and partitions but it is still slow!

DBMS brings indexes to memory (RAM) for fast querying, in your case, your memory may be not enough to contains indexes, so DBMS needs to get indexes from disk and it becomes slow. There are some solutions:

1. Upgrade hardware to have more memory. This is fastest solution but there will be a time that you cannot use this solution anymore if your data size grows too fast, cost growth is not acceptable
2. Archive old data, only allow users to see recent data (1 month, 3 months, 6 months, 1 year, etc. Depend on your app). You can see this behavior in internet banking, they allow us to see transactions in recent 3 or 6 months only. To do this, delete old records in case they are not needed anymore or move old records to another table if they are still useful (most applications use this)

---------------
Above cases are most popular cases that you may meet when working with data that become bigger day by day. There will be more cases but I believe you can find solutions yourself, just keep in mind: **large data should not cause large impact to performance**, if it does, there should be solution(s) for it!