# example-mongo-redis
## How to Speed up Mongo Queries

As you might already know MongoDB is already fast when performing read queries and specifically when it’s using Indexes

If you are not familiar with Indexes in MongoDB here is a quick summary;

MongoDB has something internally called an index, an index is matched up with an individual collection.

This index is an efficient data structure for looking up sets of records inside of that collection.

So rather than having to look at a record and see if it matches the query and the one after and so on (A full Collection Scan), the index allows us to go directly to the record that we care about.

However, You can very easily write queries that don’t match up with an index or don’t have an index available.
So in those situations, you would very easily run into big performance concerns around your application.

### Now there are two ways to solve this performance concern:

#### 1. Creating Multiple Indexes for each collection :

That might seem like a really obvious thing to do.

However, whenever you add indexes to a collection, that impacts your ability to write to that collection performantly.

Also, You might be making queries inside of an application where you can't really figure out ahead of time what indexes you need for it.
#### 2. Caching Data with Redis
Redis is an open-source, in-memory data structure store, used as a database, cache, and message broker.
Here is an illustration of how Redis is implemented as a cache server :

Any time mongoose issues a query it’s going to first go over to Redis, Redis is going to check to see if that exact query has ever been issued before.

If it hasn’t then the server will take the query and send it over to MongoDB and Mongo is going to execute the query.

You then take the results of that query and store them on Redis

Now any time the same exact query is issued again Mongoose is going to send the same query over to the Redis but this time if Redis sees that the query has already been issued once before it’s not going to send the query to MongoDB.

Instead, it’s going to take the response to that query that it got the last time and immediately send it back over to mongoose.