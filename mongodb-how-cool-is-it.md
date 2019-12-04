## MongoDB: How cool is it?

#### Issues
A relational database is a digital database based on the relational model of data, as proposed by E. F. Codd in 1970.

Since MySQL’s release in 1995, it has been a popular and inexpensive option.

However, it, of course, contains some weakness that may come from the design structure:

+ Strict schema: It's a regular thing, but it's actually a weakness.
For example, you need to build an application that allows users to create some of their own attributes. So, for each attribute, you need to create a column on the database. It makes the data redundant and difficult to manage. I worked on an application about hotel data, this has a table with more than 500 column that created by user and most of them are empty!!!
<br/>If users want to more complex attribute (nested object for example), it may reach the technology limitation ...
Of course, some relation database solution like PostgreSQL or MySQL released some updates for this issue (JSON type of MySQL and HStore for PostgreSQL) and we can deal with it with some code-based solution. However, it's still a weakness.

+ Vertical Scaling only: in Vertical Scaling, we increase the CPU, RAM and Storage or buy a more robust server in order to support more traffic and load on our database server. Here are its pros and cons
<br/>Pros
    * Less power consumption than running multiple servers
    * Cooling costs are less than scaling horizontally
    * Generally less challenges to implement

  Cons
    * It’s way more costly
    * There is a limit to increasing hardware on a single system
    * Greater risk of hardware failure causing bigger outages

#### Solution
With the above issues, relational database maybe not a good solution for *modern thinking* that it needs a better way to store and manage their data. And MongoDB is one of the candidates.
MongoDB is a cross-platform document-oriented database program. Classified as a NoSQL database program, MongoDB uses JSON-like documents with schemata. MongoDB is developed by MongoDB Inc.

#### Why MongoDB?

+ Generally, MongoDB is faster than relational database: No one can state that: 'MongoDB is faster than relational database' but in my option (and a lot of others), MongoDB is faster than relational database, generally. As most of the normal developers, I worked with MySQL/PostgreSQL first, then I worked with MongoDB. At the first time, I'm very impressed with the performance of this. This was the first time I saw a database query with 0.1 millisecond !!! For some experiments, MongoDB can perform 100x faster than MySQL
+ Dynamic schema: As a JSON-based solution, it's also categorized as schema-less database. Please feel free to push any data you need into database. It will store them all !!!
+ No Joins overhead: Joins is one of the bad performance action of relational database solution. While MongoDB is a document database, it means database that contains data that is self-sufficient and all the information is embedded like a real document. No join need!!!
+ Horizontal Scaling: in Horizontal Scaling, we add more systems with the smaller configuration in terms of CPU, RAM and Storage to distribute the traffic or load across these systems.<br/>
MongoDB supply *sharding* technique, it allows us to add more "nodes" (systems).
In my option, this is the most benefit that MongoDB support. With this feature, we can reduce the latency and get rid of downtime when migrate, upgrade server( I updated a MongoServer from 2.6 to 3.4 without downtime).  So, it's adaptive with some modern concepts such as Load Balancer, Decentralized System, Cloud Services or Micro-services.
Here are Horizontal Scaling pros and cons
<br/>Pros
    * Much cheaper than scaling vertically
    * Easier to run fault-tolerance
    * Easy to upgrade

  Cons
    * Bigger footprint on data center
    * Higher utility cost

+ JSON-friendly database

#### Why not?
Of course, MongoDB is not MySQL replacement. It still has some weakness that we need to consider when choosing database solution
+ None-transaction relation model: This is one of reason that make MongoDB is fast. The transaction concept of relational database handle lot of thing. So, MongoDB action will missing somethings:
    * No transaction rollbacks
    * No transaction locking
    * Asynchronous writing

  **UPDATE**: MongoDB has just released version 4.0 with multi-document ACID transactions. But it take a trade off with performance cost

+ As MongoDB suggestion, this is not a replacement for legacy solution.

+ MongoDB consumes more resource than relational database. As Horizontal Scaling pros and cons, we must take bigger storage for keep track data on a decentralized system. Furthermore, we need to supply more memory for MongoDB. For example, WireTiger storage engine of MongoDB use 2GB to 60% memory for caching as default. Or the performance will be decreased.

#### Some Database suggestions
As my statement's above, MongoDB is not relational database replacement,it is just better than relational database as some points.
Choosing database solution is the most important thing we must make on development phase. Here's my thinking for some cases

+ Well-structure/complex transaction database: Relational database
+ Document/JSON-like/schema-less/high performance/scaling database: MongoDB
+ In-memory performance database: [Redis](https://redis.io/)
+ Searching database: [ElasticSearch](https://www.elastic.co/products/elasticsearch)
+ Graph database: [neo4j](https://neo4j.com/)
+ Linear scalability database: [cassandra](http://cassandra.apache.org/)

#### Reference
+ [https://www.mongodb.com/compare/mongodb-mysql](https://www.mongodb.com/compare/mongodb-mysql)
+ [https://en.wikipedia.org/wiki/MongoDB](https://en.wikipedia.org/wiki/MongoDB)
+ [https://en.wikipedia.org/wiki/Relational_database](https://en.wikipedia.org/wiki/Relational_database)