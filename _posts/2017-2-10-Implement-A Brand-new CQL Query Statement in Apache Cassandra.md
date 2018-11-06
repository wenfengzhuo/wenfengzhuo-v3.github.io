---
layout: post
comments: true
categories: distributed system
title: Implement a new brand-new CQL query statement in Apache Cassandra
---
### Introduction
This patch implemented a new type of query that supports query of max timestamp of all rows under a partition key. The following picture shows how to use this new type of query:

The picture shows four scenarios for this query.

- query the max timestamp for a partition key (the key is single)
- query the max timestamp for a partition key (the key is composite)
- query the max timestamp for a non-exist key (will return a empty result)
- query the max timestamp with insufficient number of values for partition key (will raise an error)




![pic](http://i.imgur.com/vo5FW4g.gif)

The solution created a new grammar in CQL and an independent statement. It uses built-in ReadCommand to read all rows, so it follows the same path with SelectStatement. The difference is that it does not require any columns to be returned, which could help to reduce much I/O operations. As the data is offered online, so every row will be read at least once, hence the time complexity for this query will be O(n) (n represents the number of rows in this partition key). As much less data is returned, it will not cause much consumption of memory. Hence, both time complexity and memory complexity will satisfy the requirements.

As for the shortage of this implementation, it uses built-in ReadCommand to read the data, although all the data we need is the metadata of those rows. As the max timestamp is for a partition key, we could maintain a "max timestamp" metadata item for each partition key. When inserting, updating or deleting a row under this partition key, we directly update its "max timestamp". In this way, we could retrieve the max timestamp in O(1) time. But for this solution, it requires extra efforts to make sure the max timestamp is updated correctly due to concurrency, which might cause performance issue. If this is a used heavily by users, then we could implement a more efficient solution otherwise the former solution will be preferred as above described trade-offs.

### Pull Request
[Github PR](https://github.com/wenfengzhuo/cassandra/pull/1){:target="_blank"}
