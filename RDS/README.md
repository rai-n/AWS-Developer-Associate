<a name=“aws-rds”></a>
### Amazon Relational Database Service (RDS)

#### Overview
Amazon Relational Database Service (Amazon RDS) is a web service that makes it easier to set up, operate, and scale a relational database in the AWS Cloud. It provides cost-efficient, resizable capacity for an industry-standard relational database and manages common database administration tasks. You can choose from seven popular engines — Amazon Aurora with MySQL compatibility, Amazon Aurora with PostgreSQL compatibility, MySQL, MariaDB, PostgreSQL, Oracle, and SQL Server — and deploy on-premises with Amazon RDS on AWS Outposts.

##### Use Cases
1. Build web and mobile applications: Support growing apps with high availability, throughput, and storage scalability. Take advantage of flexible pay-per-use pricing to suit various application usage patterns.
2. Move to managed databases: Innovate and build new apps with Amazon RDS instead of worrying about self-managing your databases, which can be time consuming, complex, and expensive such as: 
    - Provisioning, OS patching
    - Backups, restoring to a timestamp (Point in Time Restore)
    - Monitoring dashboards
    - Read replicas for better read performance
    - Multi AZ setup for disaster recovery
    - Maintenance windows for upgrades
    - Scaling capability (horizontal and vertical)
    - Storage backed by EBS (gp2 or io1)
3. Break free from legacy databases: Free yourself from expensive, punitive, commercial databases by migrating to Amazon Aurora. When you migrate to Aurora, you get the scalability, performance, and availability of commercial databases at 1/10th the cost.
4. RDS Storage Auto Scaling .
    - Increase storage on RDS DB instance dynamically
    - When RDS detects you are running out of space, it scales automatically up to a maximum storage threshold and if:
        1. Free storage is less than 10% of allocated storage
        2. Low storage lasts at least 5 minutes
        3. 6 hours have passed since last modification 
    - Useful for applications with unpredictable workloads
##### Tips
1. You can use the AWS Database Migration Service (DMS) to easily migrate or replicate your existing databases to Amazon RDS.
2. Amazon RDS Partners help you with database monitoring, security, and performance using Amazon RDS database engines.

For more information about Amazon RDS you can refer to the [official documentation](https://aws.amazon.com/rds/)

<a name=“aws-rds-read-replicas”></a>
### Amazon RDS Read Replicas

#### Overview
Amazon RDS Read Replicas provide enhanced performance and durability for Amazon RDS database (DB) instances. They make it easy to elastically scale out beyond the capacity constraints of a single DB instance for read-heavy database workloads. You can create one or more replicas of a given source DB Instance and serve high-volume application read traffic from multiple copies of your data, thereby increasing aggregate read throughput. Read replicas can also be promoted when needed to become standalone DB instances. For RDS Read Replicas, you are not charged for networking cost of asynchronous replication to another AZ in the same region, but cross region replication is charged.

![](https://i.imgur.com/PKVwixD.png)

##### Use Cases
1. Scaling beyond the compute or I/O capacity of a single DB instance for read-heavy database workloads. You can direct this excess read traffic to one or more read replicas.
2. Serving read traffic while the source DB instance is unavailable. In some cases, your source DB instance might not be able to take I/O requests, for example due to I/O suspension for backups or scheduled maintenance. In these cases, you can direct read traffic to your read replicas.
3. Business reporting or data warehousing scenarios where you might want business reporting queries to run against a read replica, rather than your production DB instance.
4. Implementing disaster recovery. You can promote a read replica to a standalone instance as a disaster recovery solution if the primary DB instance fails.

##### Tips
1. For the MySQL, MariaDB, PostgreSQL, Oracle, and SQL Server database engines, Amazon RDS creates a second DB instance using a snapshot of the source DB instance. It then uses the engines’ native asynchronous replication to update the read replica whenever there is a change to the source DB instance.
2. The read replica operates as a DB instance that allows only read-only connections; applications can connect to a read replica just as they would to any DB instance.

##### Multi AZ Disaster recovery
Amazon Multi AZ helps with diaster recovery by synchronously replicating master DB instance to a standby instance in another AZ. There is only one DNS name and the application automatically failover to standby instance if there are any issues with the master DB such as loss of AZ, network, instance or storage failure. This increases availability. 

![](https://i.imgur.com/ASrkfPo.png)

##### Launching an RDS instance

1. You can choose between "standard create" which lets you configure the database, and "easy create" which uses best practice configuration and lets you quickly deploy the database. There are many relational database engines that you can select.

![](https://i.imgur.com/e9o1RAq.png)

2. For production and dev environments, you might use Multi-AZ deployment to synchronously replicate master DB to a standby database for better availability and disaster recovery. A Multi-AZ DB cluster deployment in Amazon RDS provides a high availability deployment mode of Amazon RDS with two readable standby DB instances. A Multi-AZ DB cluster has a writer DB instance and two reader DB instances in three separate Availability Zones in the same AWS Region. Multi-AZ DB clusters provide high availability, increased capacity for read workloads, and lower write latency when compared to Multi-AZ DB instance deployments.

You should consider using Multi-AZ DB cluster deployments with two readable DB instances if you need additional read capacity in your Amazon RDS Multi-AZ deployment and if your application workload has strict transaction latency requirements such as single-digit milliseconds transactions.
 
![](https://i.imgur.com/bCotxb1.png)

3. After entering the database identifier and password, you can select the type of class. I selected `db.t2.micro` for the instance class, general purpose storage `gp2` with a maximum storage auto scaling of 1000 GB to remain in free tier. I used 

![](https://i.imgur.com/Y7DC30h.png)

4. For connectivity settings, you can automatically connect the DB to an EC2 instance which configures the underlying networking. I used my default VPC and set public access to true so that I can access the DB from my pc using the internet.

![](https://i.imgur.com/gFckQY0.png)

5. Users can authenticate to the database using password, IAM credentials or kerberos authentication. 

![](https://i.imgur.com/G5buj4L.png)

6. Giving an initial DB name creates the database when the instance is launched. The backup options can be used to specify if you want a backup and you can also define the retention period from 0-35 days. You can also select a window where you want the backups to be done. You can also 

![](https://i.imgur.com/xZxuH7h.png)

7. You can also select the type of logs you want to publish to CloudWatch logs, such as audit log, error, general and slow query log for long term retention. You can also choose to enable maintenance window and specify a time for automatic minor DB updates. Selecting "deletion protection" prevents the database from being deleted.

![](https://i.imgur.com/ek8Qjs9.png)

8. You can use a tool like Sql Electron client to connect and interface with the database.

![](https://i.imgur.com/znbt581.png)
![](https://i.imgur.com/bnaFKXo.png)

9. From the RDS instance console, you can also directly create read replicas to increase read capacity, migrate to another region, take snapshots and restore in a different AZ or restore to a point in time. In the "Monitoring" tab you can also see DB analytics such as CPU Utilization.

![](https://i.imgur.com/lR8qD3c.png)
![](https://i.imgur.com/zif4i8Q.png)

For more information about Amazon RDS Read Replicas you can refer to the [official documentation](https://aws.amazon.com/rds/features/read-replicas/)

<a name=“aws-aurora”></a>
### Amazon Aurora

#### Overview
Amazon Aurora is a fully managed relational database engine that’s compatible with MySQL and PostgreSQL. It combines the speed and reliability of high-end commercial databases with the simplicity and cost-effectiveness of open-source databases. With some workloads, Aurora can deliver up to five times the throughput of MySQL and up to three times the throughput of PostgreSQL without requiring changes to most of your existing applications. Aurora storage automatically grows in increments of 10GB, up to 128 TB and have up to 15 replicas and the replication process is faster than MySQL. Aurora costs 20% more than RDS but is more efficient. Failover in Aurora is instantaneous compared to multi-az or cluster for RDS. As well there is:
- Industry compliance
- Push-button scaling
- Automated patching with 0 downtime
- Backtrack (restore data at any point without using backups)
    - Amazon Aurora Backtrack is a feature that allows you to “rewind” a database cluster to a specific point in time without restoring data from a backup. This feature works by using the distributed, log-structured storage system of Amazon Aurora. Each change to your database generates a new log record, identified by a Log Sequence Number (LSN). When you enable the backtrack feature, Aurora provisions a FIFO buffer in the cluster for storage of LSNs, allowing for quick access and recovery times measured in seconds
- Regular backup 
- Isolation and security.

##### Use Cases
1. Modernize enterprise applications: Operate enterprise applications, such as customer relationship management (CRM), enterprise resource planning (ERP), supply chain, and billing applications, with high availability and performance.
2. Build SaaS applications: Support reliable, high-performance, and multi-tenant Software-as-a-Service (SaaS) applications with flexible instance and storage scaling.
3. Deploy globally distributed applications: Develop internet-scale applications, such as mobile games, social media apps, and online services, that require multi-Region scalability and resilience.

##### High Availability and Read Scaling
1. 6 copies of your data is made across 3 AZ
    - 4 copies out of 6 needed for writes
    - 3 copies out of 6 needed for reads
    - self healing with peer-to-peer replication if there is data corruption
    - storage is striped across 100s of volumes
2. There is only once Aurora instance that takes writes (master)
3. If the master doesn't work, failover happens in < 30 seconds

![](https://i.imgur.com/8Ot8FIL.png)

##### Aurora DB Cluster
- Master writes to shared storage volume (Auto expanding 10GB - 128TB)
- Aurora provides "Writer Endpoint" which always points to the master, even if the instance failover. The client is always talking to the writer endpoint. 
- You can set up auto scaling on the read replicas so the read requirements are fulfilled.
- There is also a "Reader Endpoint" which automatically connects to all the read replicas and load balances connections; when the client connects they are redirected to one of the read replicas.

![](https://i.imgur.com/x5MEcll.png)
![](https://i.imgur.com/Pr2rJTE.png)

##### Tips
1. Easily migrate MySQL or PostgreSQL databases to and from Aurora using standard tools, or run legacy SQL Server applications with Babelfish for Aurora PostgreSQL with minimal code change.

![](https://i.imgur.com/ZbjXnfm.png)

##### Security for RDS & Aurora 
1. Data encrypted in volumes at rest 
    - Database master & replica encryption using AWS KMS - must be defined as launch time
    - If the master is not encrypted, the read replicas cannot be encrypted
    - To encrypt an un-encrypted database, go through a DB snapshot & restore as encrypted
2. In-light encryption
    - TLS-ready by default, use the AWS TLS root certificates client-side
3. IAM Authentication
    - IAM roles to connect to your database (instead of username/ password)
4. Security Groups
    - Control Network access to your RDS/ Aurora DB
5. Audit Logs can be enabled and sent to CLoudWatch Logs for longer retention

For more information about Amazon Aurora, you can refer to the official [AWS documentation](https://aws.amazon.com/rds/aurora/)

<a name=“aws-rds-proxy”></a>
### Amazon RDS Proxy

#### Overview
Amazon RDS Proxy is a fully managed, highly available database proxy for Amazon Relational Database Service (RDS) that makes applications more scalable, more resilient to database failures, and more secure. RDS Proxy allows applications to pool and share connections established with the database, improving database efficiency by reducing the stress on database resources like CPU, CRAM and minimize open connections and timeouts. RDS Proxy is never publicly accessible and must be accessed from VPC.

##### Use Cases
RDS Proxy is particularly helpful for applications that have unpredictable workloads, frequently open and close database connections, or require higher availability during transient database failures.

##### Tips
1. RDS Proxy can be enabled for most applications with no code changes.
2. RDS Proxy can enforce AWS Identity and Access Management (IAM) authentication for databases and securely store credentials in AWS Secrets Manager.
3. RDS Proxy can reduce failover times for Aurora and RDS databases by up to 66%.

##### RDS Proxy vs Multi-AZ 

Amazon RDS Proxy and Multi-AZ deployments are two different features that can be used together to improve the scalability, availability, and resilience of your database.

Multi-AZ deployments provide high availability by automatically failing over to a standby replica in a different Availability Zone in the event of a primary DB instance failure or planned maintenance. This can help minimize downtime and data loss. On the other hand, RDS Proxy is a fully managed database proxy that sits between your application and your RDS database. It allows applications to pool and share connections to the database, reducing the overhead of establishing new connections and improving database efficiency and application scalability. RDS Proxy can also reduce failover times for Aurora and RDS databases by up to 66%1.

Using reader/writer endpoints with Aurora allows you to distribute read traffic across multiple read replicas, improving read scalability. However, this approach requires you to manage connections to multiple endpoints in your application code.

In summary, RDS Proxy can provide additional benefits over Multi-AZ deployments and using reader/writer endpoints with Aurora by improving connection management, reducing failover times, and making applications more resilient to database failures.

For more information about Amazon RDS Proxy, you can refer to the official [AWS documentation](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/rds-proxy.html)

<a name=“aws-elasticache”></a>
### Amazon ElastiCache

#### Overview
Amazon ElastiCache is a fully managed in-memory data store and cache service that supports two open-source in-memory engines: Redis and Memcached. ElastiCache can be used to improve the performance of web applications by allowing you to retrieve information from fast, managed, in-memory data stores instead of relying on slower disk-based databases.


##### ElastiCache Strategies
ElastiCache allows you to implement various caching strategies to improve the performance of your application. Two common strategies are Lazy Loading and Write-Through.

1. Lazy Loading, also known as “cache aside,” is a caching strategy that loads data into the cache only when necessary. When your application requests data, it first makes the request to the cache. If the data exists in the cache and is current, the cache returns the data to your application. If the data doesn’t exist in the cache or has expired, your application requests the data from your data store. Your data store then returns the data to your application. Your application next writes the data received from the store to the cache. See pseudocode below:

![](https://i.imgur.com/MgykC7V.png)
![](https://i.imgur.com/gUXT6Cv.png)

2. Write-Through is a caching strategy where data is written to both the cache and the underlying data store at the same time. This ensures that the cache always has the most up-to-date version of the data. However, this approach can increase write latency since writes must be performed on both the cache and the underlying data store.

![](https://i.imgur.com/bz5Idez.png)
![](https://i.imgur.com/bYJmk7s.png)

##### Cache Eviction and Time-to-live (TTL)
Cache eviction refers to the process of removing data from a cache when it is no longer needed or when the cache is full and needs to make room for new data. Time-to-live (TTL) is a common cache eviction strategy that automatically removes data from the cache after a specified period of time has passed. This can help ensure that the data in the cache is up-to-date and relevant.

For example, Hazelcast has a time-to-live-seconds attribute that can be set for entries in a map. Entries that are older than the specified number of seconds and have not been updated within that time will be automatically evicted from the map. Similarly, Spring Boot allows you to set a default TTL for cache entries using the spring.cache.redis.time-to-live property.

- TTL are helpful for any kind of data:
    1. Leaderboards
    2. Comments
    3. Activity streams
- TTL can range from few seconds to hours or days
- If there are too many evictions happening due to memory, you should scale up or out

##### Solutions Architecture 
1. DB Cache
    - Applications query ElastiCache and if there is a miss, the data is fetched from RDS and sent to application and written to ElastiCache
    - Relieves load in RDS
    - Cache must have invalidation strategy to ensure only most current data is used there

![](https://i.imgur.com/ztnSXIq.png)

2. User Session Store
    - User logs into any of the application
    - The application writes the session data into ElastiCache
    - The user hits another instance of our application
    - The instance retrieves the data if the user is already logged in
    - This makes the application stateless
        1. Making an application stateless means that the application does not store any data about client state or session information. Instead, this information is offloaded to a cache or a database so that the application itself does not need to keep track of it. This allows the application to scale horizontally and be tolerant to the failure of an individual node.

        2. Amazon ElastiCache is a service that can be used to offload state from an application. It provides an in-memory cache that can be used to store frequently accessed data, such as session information, at low latency. By using ElastiCache, you can improve the performance of your application and make it stateless.

![](https://i.imgur.com/nXuLy6G.png)

##### Amazon MemoryDB for Redis
Amazon MemoryDB for Redis is a Redis-compatible, durable, in-memory database service that delivers ultra-fast performance with high availability and durability. MemoryDB for Redis is built for applications that require microsecond read and write latencies with strong durability guarantees.

##### Use Cases
1. ElastiCache can be used for a variety of use cases such as lowering total cost of ownership by caching your data and offloading database I/O to reduce operational burden, lower costs, and improve performance of both the database and the application. It can also be used for real-time application data caching by storing frequently used data 2. in-memory for microsecond response times and high throughput to support hundreds of millions of operations per second.

##### Tips
1. ElastiCache integrates with other AWS services such as Amazon CloudWatch for enhanced visibility into key performance statistics associated with your cache.
2. You can use ElastiCache’s built-in data structures to simplify application development.
3. ElastiCache makes setup, scaling, and cluster failure handling much simpler than in a self-managed cache deployment.

##### Redis vs Memcached 

- Similar to RDS, REDIS has Multi AZ deployments with auto failover. It uses Read Replicas to scale reads and have high availability. It has high data durability using AOF persistence. It has backup and restore features as well as support for sets and sorted sets.
    1. Redis can use an Append Only File (AOF) for high data durability. AOF is a logging mechanism that writes every write operation performed on the Redis database to a log file on disk. This log file can be used to reconstruct the database in the event of a crash or failure. When Redis is restarted, it reads the log file and re-executes the write operations in the file to restore the database to its previous state.
    2. AOF provides better data durability than the snapshot persistence option, which only creates point-in-time snapshots of data. However, it is slower and requires more disk space, as it must write every write operation to the log file. The AOF file can be configured to be rewritten in the background when it gets too large, using a process called AOF fsync.
- Memcached in the other hand uses multi-node partitioning of data (sharding). It does not have high availability as there is no replications happening. There is no data persistence, and the ability to backup and restore. It has a multi-threaded architecture.

![](https://i.imgur.com/H5jyopA.png)

##### Creating an ElastiCache Redis cluster

- I disabled cluster mode to stay in the free tier and not use replication across multiple shards.

![](https://i.imgur.com/CKPYfp5.png)

- Using On Cloud location but you can create the Redis instance on premise by specifying an outpost id.

![](https://i.imgur.com/DgttbMW.png)

- I used `cache.t2.micro` and set the number of replicas to zero to remain in free tier.

![](https://i.imgur.com/dVtOdDQ.png)

- Then just like RDS, the network and subnets are defined. 

![](https://i.imgur.com/0MriXZf.png)

- Then for security, you can enable encryption at rest using AWS KMS as well as encryption in transit where you can use access control such as Redis Auth. 

![](https://i.imgur.com/vpUnvIC.png)

- Just like RDS, you can also set up backup and retention periods. 

![](https://i.imgur.com/lIXI6xP.png)

- Just like RDS, you can define the maintenance window for updates, as well as auto upgrade for minor versions.

![](https://i.imgur.com/9ULBfo7.png)

- Just like RDS, you can also enable logs for logging queries based on some criteria.

![](https://i.imgur.com/n0PXf9U.png)

##### Summary
1. Lazy loading / Cache aside is easy to implement and works for many situations as a foundation, especially on read side. 
2. Write-through is usually combined with lazy loading as targeted for queries or workloads that benefit from this optimization to improve cache staleness.
3. Setting a TTL is usually not a bad idea, except when you're using Write-through. Set it to a sensible value for the application.
4. Only cache things that make sense (user profile, blogs, etc)

For more information about Amazon ElastiCache, you can refer to the official [AWS documentation](https://aws.amazon.com/elasticache/)

<a name=“aws-memorydb-for-redis”></a>
### MemoryDB for Redis Overview

##### Overview
Amazon MemoryDB for Redis is a durable, in-memory database service that delivers ultra-fast performance. It is purpose-built for modern applications with microservices architectures and is compatible with Redis, a popular open source data store. With MemoryDB, all of your data is stored in memory, which enables you to achieve microsecond read and single-digit millisecond write latency and high throughput.

![](https://i.imgur.com/7N50AaX.png)

##### Tips
1. MemoryDB for Redis can be used as a high-performance primary database for your microservices applications, eliminating the need to separately manage both a cache and durable database.
2. You can build applications quickly with Redis, Stack Overflow’s “Most Loved” database for five consecutive years.
3. MemoryDB also stores data durably across multiple Availability Zones (AZs) using a Multi-AZ transactional log to enable fast failover, database recovery, and node restarts.
 
For more information about Amazon Aurora, you can refer to the official [AWS documentation](https://aws.amazon.com/memorydb/)
