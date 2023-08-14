# Redis

**Redis**, which is an open source, in-memory data store. We can use it as a database, cache, streaming engine, and message broker.

## Key features

### 1. In-memory data storage

Redis stores data in memory, which allows for extremely fast read and write operations. However, this also means that its data is lost by default (when server restart), although it does provide options for persistence and backups.

### 2. Data Structures

Redis provides a wide range of data structures that go beyond simple key-value pairs. These structure include string, lists (linked lists), sets, sorted sets, hashes, bitmaps, etc.

### 3. Persistence

While Redis primarily operates in-memory, it offers various persistence options to save data to disk, making it suitable for scenarios where data durability is important. This includes snapshots (point-in-time backups) and append-only files.

### 4. Publish/Subscribe Messaging

Redis includes a pub/sub (publish/subscribe) system that allows different parts of an application to communicate asynchronously through message channels.

### 5. Caching

One of the most common use cases for Redis is caching. By storing frequently accessed data in memory, Redis helps to significantly reduce the load on backend databases and improves response times for applications.

### 6. High Traffic and Low Latency

Redis is known for its high traffic and low latency characteristics, making it a popular choice for real-time applications and situations where quick data access is essential.



----
Find the location of the configuration file 

```bash
$ redis-cli INFO | grep config_file
```
