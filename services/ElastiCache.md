# Amazon ElastiCache
It's a web service that makes it easy to deploy, operate and scale in-memory cache in the cloud. It improves performance of applications by retrieving data from fast, managed, in-memory caches, instead of relying entirely on slower disk-based databases.

## Useful Links
- [AWS - ElastiCache FAQ](https://aws.amazon.com/elasticache/faqs/)
- [AWS - ElastiCache for Redis - Caching Strategies and Best Practices](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/BestPractices.html)
- [AWS - ElastiCache for Memcached - Caching Strategies and Best Practices](https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/BestPractices.html)
- [Memcached](https://memcached.org/)
- [Redis](https://redis.io/)

## General Notes
- It's compatible with Redis and Memcached.
- Can be used to store critical pieces of data in memory for low latency access
- May include the results of I/O-intensive database queries for read-heavy application workloads (such as gaming and social networking) or the results of computationally-intensive calculations (such as recommendation engine) 
- **Redis**
    - Supports Master/Slave replication and **Multi-AZ**, which can be used to achieve cross AZ redundancy (not available on Memcached)
    - It's managed more like a relation database, because of its replication and persistence features
    - Redis ElastiCache clusters are managed as stateful entities that include failover, similar to how Amazon RDS manages database failover
    - Use cases:
        - More complex data types, such as lists, hashes and sets
        - Sorting and ranking datasets, for example leaderboards
        - Persistence of key store is important
        - Need to run on multi-AZ with failover
        - Pub/Sub capabilities are needed
- **Memcached**
    - As it is designed as a pure caching solution, with no persistence, ElastiCache manages its nodes as a pool that can grow and shrink, similar to EC2 Auto Scaling Group
    - Individual nodes are expendable, and ElastiCache provides additional capabilities, such as automatic node replacement and Auto Discovery
    - Use cases:
        - Object caching is the ultimate goal, for example to offload the database
        - Caching model as simple as possible
        - Large cache nodes, requiring multithreaded performance with utilization of multiple cores
        - Ability to scale out