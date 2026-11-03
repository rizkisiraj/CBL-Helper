Case Study: Discord’s Database Transformation
=============================================

[![Siddharth](https://miro.medium.com/v2/resize:fill:64:64/1*cAu9Cy01fAJEgFVVkTyk3Q.png)](https://medium.com/@siddharthgov01?source=post_page---byline--a8d5b226a7ab---------------------------------------)

[Siddharth](https://medium.com/@siddharthgov01?source=post_page---byline--a8d5b226a7ab---------------------------------------)

6 min read

·

Apr 23, 2023

[nameless link](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Fp%2Fa8d5b226a7ab&operation=register&redirect=https%3A%2F%2Fmedium.com%2F%40siddharthgov01%2Fcase-study-discords-database-transformation-a8d5b226a7ab&user=Siddharth&userId=49cfcf6cdea7&source=---header_actions--a8d5b226a7ab---------------------clap_footer------------------)

--

[nameless link](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Frepost%2Fp%2Fa8d5b226a7ab&operation=register&redirect=https%3A%2F%2Fmedium.com%2F%40siddharthgov01%2Fcase-study-discords-database-transformation-a8d5b226a7ab&user=Siddharth&userId=49cfcf6cdea7&source=---header_actions--a8d5b226a7ab---------------------repost_header------------------)

[nameless link](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa8d5b226a7ab&operation=register&redirect=https%3A%2F%2Fmedium.com%2F%40siddharthgov01%2Fcase-study-discords-database-transformation-a8d5b226a7ab&source=---header_actions--a8d5b226a7ab---------------------bookmark_footer------------------)

[Listen](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2Fplans%3Fdimension%3Dpost_audio_button%26postId%3Da8d5b226a7ab&operation=register&redirect=https%3A%2F%2Fmedium.com%2F%40siddharthgov01%2Fcase-study-discords-database-transformation-a8d5b226a7ab&source=---header_actions--a8d5b226a7ab---------------------post_audio_button------------------)

Share

Welcome database geeks! Prepare for an intriguing case study as we uncover why Discord chose ScyllaDB over CassandraDB in their quest for improved performance.

![Image by Author](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*hhI0KOuMePZA2ntRODIW1A.png)

### Discord: -

Discord is a popular communication platform (app & website) which combines chat, voice, and video to create an all-in-one solution for communities, teams, and gamers. Users can create and join various servers, through which they can connect and interact. It also offers features such as chat & voice channels, direct messaging, and video calls along with streaming options. It has become extremely popular since its release in 2015, as it supports rich media embedding, file sharing and integrating with other applications. Discord is easy to use, flexible and versatile, making it a preferred choice for various communities and groups around the world.

### Beginning: -

In the beginning Discord used MongoDB (a NoSQL database) to store and retrieve data from it. As MongoDB was a NoSQL database it had the ability to scale horizontally and had reliable performance. But as Discord grew more & more popular MongoDB was unable to satisfy their demands of being a superior database and they had to switch from MongoDB to CassandraDB (commonly known as Cassandra) in 2017.

### After MongoDB: -

After transferring to Cassandra Discord developers were satisfied as Cassandra was providing better scalability, fault tolerance, flexibility, and high availability. But soon again Discord would observe a heavy rise in users and consumers as the service was getting more popular due to its reliable and faster services and soon Discord developers were again facing challenges due to the increase in demand, and they soon had to find an alternative to Cassandra. Read the official Discord [Blog](https://discord.com/blog/how-discord-stores-billions-of-messages) to know the exact details of why they moved to Cassandra.

### Cassandra: -

Cassandra is a distributed, highly scalable and NoSQL database management system which was designed to handle enormous amounts of data across multiple servers and clusters with high availability and fault tolerance. It was originally developed by Facebook (now called META) and later released as an open-source project managed by the Apache Software Foundation.

Just to let you know how reliable and popular Cassandra is, here’s a list of top companies which use CassandraDB — _Netflix, Instagram, Apple, Spotify, eBay, Uber_ and many more.

Each instance of Cassandra is known as a “Node”, and each Node stores anywhere from between 2–4 TB of data. What is interesting about Cassandra is that multiple nodes can be scaled horizontally, and nodes are grouped in a “Cluster” or a “Ring”. Also, for better fault-tolerance & high availability, data is replicated across multiple nodes, so that any single point of failure will not lead to the loss of that particular data.

![Image by Author](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*T_q_fwNXkgeHIPhcAy4VDA.png)

### Working of Cassandra: -

Cassandra uses columnar storage model, where data is stored in columns rather than rows.

**_NOTE: Refer to the diagram in the section “Working of ScyllaDB” to better understand it._**

1.  Each node in Cassandra plays an equal role, there is no dedicated master or coordinator node. The data is partitioned and replicated across multiple nodes in a cluster.
2.  Cassandra uses distributed hash ring, to partition data across the nodes in the cluster. The ring is divided into smaller partitions, and each partition is assigned to a specific node based on the partition key.
3.  When a client writes data to Cassandra, the data is first written to the node that owns the partition based on the partition key. This ‘coordinator node’ is responsible for replicating the data to other nodes in the cluster.
4.  Typically, replicas are shared or distributed across multiple data centers to provide additional fault tolerance.
5.  Each node can act as a coordinator for client requests. When a client submits a read or write request, the coordinator node routes the request to the appropriate replica in the cluster.
6.  The coordinator node also manages query execution, data retrieval and data consistency with respect to the query.
7.  Cassandra uses a ‘memtable’ (in-built data structure) that holds recently written data before it is persisted to disk.
8.  Nodes in Cassandra ring communicate with each other based on a gossip protocol to share information about the ring’s / cluster’s status and location of data replicas.
9.  It also supports [Sharding.](https://en.wikipedia.org/wiki/Shard_(database_architecture))

### CQL: -

CQL (Cassandra Query Language), is a query language similar to SQL. CQL is used to interact with CassandraDB.

*   Inserting and Updating values: -

```
INSERT INTO users (user_id, name, email) VALUES (1, 'Sid', 'sid@whatever.com');
UPDATE users SET email = 'sidOK@whatever.com' WHERE user_id = 1;
```

It is possible to use the “WHERE” clause in CQL. **Another critical point to mention here is that CQL does not support “JOIN Statements” unlike SQL.**

*   Retrieving and Filtering data: -

```
SELECT * FROM users WHERE user_id = 1;
SELECT * FROM users WHERE age > 18 AND city = 'Mumbai';
```

*   Batch Insertions: -

```
BEGIN BATCH
  INSERT INTO users (user_id, name, email) VALUES (2, 'Batman', 'batsy@whatever.com');
  INSERT INTO users (user_id, name, email) VALUES (3, 'Hulk', 'hulksmash@whatever.com');
APPLY BATCH;
```

### After Cassandra: -

Now, as Discord was getting popular it started having issues with Cassandra as well. What problems did Discord face you ask? 🤔

> _Our Cassandra Troubles_
> 
> _We stored our messages in a database called cassandra-messages. As its name suggests, it ran Cassandra, and it stored messages. In 2017, we ran 12 Cassandra nodes, storing billions of messages._
> 
> _At the beginning of 2022, it had 177 nodes with trillions of messages. To our chagrin, it was a high-toil system — our on-call team was frequently paged for issues with the database, latency was unpredictable, and we were having to cut down on maintenance operations that became too expensive to run._
> 
> _What was causing these issues? First, let’s take a look at a message._
> 
> _-_ [_Discord Blog_](https://discord.com/blog/how-discord-stores-trillions-of-messages)

As described by Discord developers themselves, there were many issues you can check out their blog for more details. In short, they faced issues related to scalability, performance, flexibility, consistency, and compatibility.

So, Discord started looking for alternatives again and they found ScyllaDB (a NoSQL database).

### ScyllaDB: -

“ScyllaDB is the monstrously fast and scalable NoSQL database for data-intensive apps that require high performance and low latency.” As said by ScyllaDB itself, it is a fast, reliable, and highly scalable database management system, which is also compatible with Cassandra and Amazon’s DynamoDB APIs. **It also supports CQL.**

ScyllaDB is growing more & more popular due to its high availability and reliability. Companies such as _Discord, Disney+ Hotstar, Starbucks, Samsung, IBM_ and many more use it, due to its capability of performing millions of operations per second and petabyte scaling features. Here’s a quick introduction [video.](https://www.youtube.com/watch?v=MWFWM_LcouY)

### Working: -

The working is extremely similar to Cassandra, like Cassandra ScyllaDB also stores data in nodes and nodes are grouped in a cluster.

![Working of ScyllaDB](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*0arrci-rmOWhD2ZVSrqtHQ.png)

As mentioned previously, the working of ScyllaDB is similar to that of Cassandra. Data is replicated over many nodes so that a single point of failure will not lead to data loss. Simultaneous read and write operations can be performed along with replicating the data using the coordinator node.

### Advantages of ScyllaDB over Cassandra: -

1.  ScyllaDB is written in C++, unlike Cassandra which is written in ‘Java’. As Java uses a [garbage collector](https://en.wikipedia.org/wiki/Garbage_collection_(computer_science)) for memory management, java performs slower compared to C++ which has no garbage collector.
2.  High availability, flexibility, scalability, and fault-tolerance.
3.  Low TCO (Total Cost of Ownership), as ScyllaDB uses less hardware resources and operates at low operational cost.
4.  High ROI (Return on Investment).
5.  Compatibility with Cassandra and DynamoDB. It also supports CQL.
6.  Ease of deployment and management — It uses Scylla Manager tool, which provides automated management of clusters, rolling upgrades, backups, and monitoring.

### Final change: -

Even after switching over to ScyllaDB, Discord still had problems with their internal system architecture. So, to improve the performance of their internal architecture they switched to using Rust instead of Go (Golang).

References: -

1.  [Discord Blog](https://discord.com/blog/how-discord-stores-trillions-of-messages)
2.  [Cassandra Video](https://www.youtube.com/watch?v=ziq7FUKpCS8&t=59s)
3.  [ScyllaDB YouTube Channel](https://www.youtube.com/@ScyllaDB)

**Thank you for reading till the end of my blogpost...! Until next time, may your data queries be snappy and your scaling seamless.** 😉